# 🏗️ ERP TRANSPORT
## Document 08 — Architecture technique

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`
- `06_definition_mvp.md`
- `07_architecture_fonctionnelle.md`

---

# 1. 🎯 Objectif du document

Ce document traduit l'architecture fonctionnelle en une première architecture logicielle.

Il doit permettre à l'équipe de comprendre :

- 🧱 comment organiser le code ;
- 🔐 comment gérer l'authentification et les permissions ;
- 🗄️ comment accéder aux données ;
- 🔄 comment les modules communiquent ;
- 🧪 comment tester ;
- 🚀 comment déployer ;
- 👥 comment permettre à plusieurs développeurs de travailler en parallèle.

> 💡 **Important :** cette architecture est une proposition de départ. Les choix définitifs devront être validés collectivement après discussion technique et prototypage.

---

# 2. 🧭 Orientation technique proposée

Pour le projet actuel, une stack cohérente serait :

```text
🐍 Python
      │
      ├── 🎨 Flet
      │
      ├── 🧠 Domaine / règles métier
      │
      ├── ⚙️ Services
      │
      ├── 📦 Repositories
      │
      └── 🗄️ PostgreSQL
              │
              └── ☁️ Supabase
```

Pour le déploiement :

```text
👤 Utilisateur
      ↓
🎨 Application Flet
      ↓
🌐 Backend / API selon architecture retenue
      ↓
🗄️ PostgreSQL / Supabase
```

Le choix exact entre accès direct sécurisé à Supabase et API métier dédiée devra être validé avant le développement du MVP.

---

# 3. 🧠 Principes architecturaux

L'architecture doit respecter quelques principes simples.

## Principe 1 — Séparer le métier de l'interface

La logique métier ne doit pas être écrite directement dans les composants Flet.

À éviter :

```text
Button.on_click
     ↓
SQL
     ↓
Modification DB
```

Préférer :

```text
UI
 ↓
Service
 ↓
Repository
 ↓
Database
```

---

## Principe 2 — Une responsabilité par couche

Chaque couche doit avoir une mission claire.

```text
🎨 UI
   ↓
⚙️ Services
   ↓
🧠 Domain
   ↓
📦 Repositories
   ↓
🗄️ Database
```

---

## Principe 3 — Les règles métier ne doivent pas dépendre de Flet

Une règle comme :

> « Un siège ne peut être vendu deux fois pour le même voyage »

doit être applicable indépendamment de l'écran utilisé.

Elle doit pouvoir être testée sans lancer l'interface.

---

## Principe 4 — La sécurité doit être côté serveur

Masquer un bouton dans Flet n'est pas suffisant.

Le système doit également vérifier :

- utilisateur authentifié ;
- entreprise ;
- agence ;
- rôle ;
- permission ;
- ressource demandée.

---

# 4. 🧱 Architecture en couches

Une architecture de référence peut être organisée ainsi :

```text
┌─────────────────────────────────────┐
│             🎨 PRESENTATION         │
│          Flet / Views / UI          │
├─────────────────────────────────────┤
│          ⚙️ APPLICATION             │
│          Services / Use Cases       │
├─────────────────────────────────────┤
│            🧠 DOMAIN                │
│ Models / Rules / Business Logic     │
├─────────────────────────────────────┤
│          📦 INFRASTRUCTURE          │
│ Repositories / DB / External APIs   │
├─────────────────────────────────────┤
│            🗄️ DATA                 │
│          PostgreSQL / Supabase      │
└─────────────────────────────────────┘
```

---

# 5. 🎨 Couche Presentation

Cette couche contient ce que l'utilisateur voit.

Avec Flet :

```text
views/
components/
layouts/
theme/
navigation/
```

## Responsabilités

- afficher les données ;
- récupérer les actions utilisateur ;
- afficher les erreurs ;
- gérer la navigation ;
- adapter l'interface aux tailles d'écran.

## Ce qu'elle ne doit pas faire

Une View ne devrait pas :

- écrire directement du SQL ;
- contenir les règles métier complexes ;
- gérer directement les transactions ;
- décider seule si une opération est autorisée.

---

# 6. 🧩 Composants UI

Les composants réutilisables peuvent être organisés par responsabilité.

Exemple :

```text
components/
├── buttons/
├── forms/
├── tables/
├── dialogs/
├── feedback/
├── navigation/
└── common/
```

Pour un ERP, l'objectif est d'éviter que chaque développeur recrée :

- les boutons ;
- les formulaires ;
- les tableaux ;
- les modales ;
- les messages d'erreur.

---

# 7. ⚙️ Couche Application / Services

Cette couche orchestre les opérations métier.

Exemples :

```text
services/
├── auth_service.py
├── trip_service.py
├── ticket_service.py
├── fleet_service.py
├── driver_service.py
├── cash_service.py
└── maintenance_service.py
```

## Exemple

La vente d'un billet pourrait suivre :

```text
ticket_service.sell_ticket()
        │
        ├── vérifier voyage
        ├── vérifier siège
        ├── vérifier permission
        ├── créer billet
        ├── enregistrer paiement
        └── enregistrer événement
```

Le service orchestre.

Il ne devrait pas contenir tout le code SQL.

---

# 8. 🧠 Couche Domain

Cette couche contient les concepts métier et les règles importantes.

Exemples :

```text
domain/
├── models/
├── rules/
├── enums/
├── value_objects/
└── exceptions/
```

## Exemples de concepts

```text
Trip
Vehicle
Driver
Ticket
Passenger
Reservation
CashSession
Payment
Maintenance
Agency
Company
```

---

# 9. 📦 Modèles métier

Les modèles représentent les objets du domaine.

Exemple conceptuel :

```text
Trip
├── id
├── company_id
├── agency_id
├── route_id
├── vehicle_id
├── driver_id
├── departure_at
├── arrival_at
└── status
```

Le modèle métier ne doit pas être confondu automatiquement avec une table SQL.

Cette distinction permettra de conserver une architecture plus propre.

---

# 10. 📦 Couche Repository

Les repositories encapsulent l'accès aux données.

Exemple :

```text
repositories/
├── company_repository.py
├── agency_repository.py
├── trip_repository.py
├── ticket_repository.py
├── vehicle_repository.py
├── driver_repository.py
└── cash_repository.py
```

## Responsabilité

Le repository sait :

> **comment récupérer ou persister les données.**

Le service sait :

> **pourquoi et dans quel processus métier l'opération doit être effectuée.**

---

# 11. 🔄 Exemple Service vs Repository

## Repository

```text
TripRepository
    ├── get_by_id()
    ├── list_upcoming()
    ├── create()
    ├── update()
    └── cancel()
```

## Service

```text
TripService
    ├── create_trip()
    ├── assign_vehicle()
    ├── assign_driver()
    ├── open_sales()
    ├── start_trip()
    └── close_trip()
```

Le service orchestre plusieurs repositories lorsque nécessaire.

---

# 12. 🔐 Authentification

L'authentification peut être confiée à Supabase Auth.

Conceptuellement :

```text
👤 Utilisateur
      ↓
🔐 Supabase Auth
      ↓
🎟️ Session
      ↓
👤 Profil applicatif
      ↓
🏢 Entreprise
      ↓
🏪 Agence
      ↓
🎭 Rôles / Permissions
```

Le système doit distinguer :

### Authentification

> Qui es-tu ?

### Autorisation

> Qu'as-tu le droit de faire ?

Ces deux problèmes ne doivent pas être confondus.

---

# 13. 🛡️ Autorisation

Une permission peut être représentée conceptuellement comme :

```text
Utilisateur
    +
Rôle
    +
Permission
    +
Périmètre
       ↓
Action autorisée ou refusée
```

Exemple :

```text
🎫 ticket.sell
```

mais avec un périmètre :

```text
Entreprise A
Agence Yaoundé
```

---

# 14. 🏢 Multi-tenant

Le modèle SaaS doit intégrer l'identifiant d'entreprise dans les ressources métier.

Exemple :

```text
companies
   ↓
agencies
   ↓
trips
   ↓
tickets
```

Une approche possible consiste à avoir un `company_id` sur les principales tables métier.

Exemple :

```text
trips
├── id
├── company_id
├── agency_id
├── route_id
└── ...
```

---

# 15. 🔒 Row Level Security

Si Supabase est retenu comme infrastructure de données, la **Row Level Security (RLS)** doit jouer un rôle central dans l'isolation des données.

Le principe :

```text
Utilisateur A
      ↓
Entreprise A
      ↓
RLS
      ↓
Données A uniquement
```

Un utilisateur ne doit pas pouvoir contourner l'interface pour accéder aux données d'une autre entreprise.

> ⚠️ Les règles RLS devront être conçues avec soin et testées explicitement. Elles constituent une couche de sécurité, pas un simple détail de configuration.

---

# 16. 🗄️ PostgreSQL / Supabase

La base de données doit être pensée autour des relations métier.

Exemple simplifié :

```text
companies
   │
   ├── agencies
   │      │
   │      ├── users
   │      └── cash_sessions
   │
   ├── vehicles
   ├── drivers
   ├── routes
   │      │
   │      └── trips
   │             │
   │             ├── tickets
   │             └── passengers
   │
   └── maintenance
```

Les contraintes d'intégrité devront être assurées autant que possible par PostgreSQL :

- clés étrangères ;
- contraintes `UNIQUE` ;
- `CHECK` ;
- index ;
- transactions.

---

# 17. 🔐 Contrainte critique : double vente d'un siège

Cette règle ne doit pas reposer uniquement sur Python.

Exemple conceptuel :

```text
trip_id + seat_number
        ↓
UNIQUE
```

Ainsi, même si deux utilisateurs tentent simultanément de vendre le même siège :

```text
Agent A → siège 12 → ✅
Agent B → siège 12 → ❌
```

La base de données constitue la dernière barrière d'intégrité.

---

# 18. 💰 Transactions financières

Les opérations financières doivent être atomiques autant que possible.

Exemple :

```text
Vente billet
    │
    ├── créer billet
    ├── enregistrer paiement
    └── mouvement caisse
```

Si une étape critique échoue, le système ne doit pas laisser une situation incohérente.

Conceptuellement :

```text
BEGIN
   ↓
Billet
   ↓
Paiement
   ↓
Caisse
   ↓
COMMIT
```

ou :

```text
ROLLBACK
```

en cas d'échec.

---

# 19. 🔄 Événements métier

Certains événements peuvent être centralisés.

Exemples :

```text
TicketSold
TicketCancelled
PaymentRecorded
TripOpened
TripStarted
TripCompleted
VehicleAssigned
VehicleReplaced
CashClosed
MaintenanceCreated
```

Ils pourront ensuite alimenter :

- audit ;
- notifications ;
- reporting ;
- historique.

---

# 20. 🧾 Audit technique

L'audit doit conserver suffisamment d'informations pour répondre à :

```text
Qui ?
Quand ?
Quoi ?
Sur quel objet ?
Avant ?
Après ?
```

Exemple :

```text
audit_logs

id
company_id
user_id
action
entity_type
entity_id
old_data
new_data
created_at
```

Le modèle exact devra être validé avec l'équipe.

---

# 21. 📡 API / couche d'accès

Deux grandes options peuvent être étudiées.

## Option A — Application Flet + Supabase

```text
Flet
  ↓
Supabase Auth
  ↓
Supabase
  ↓
PostgreSQL
```

### Avantages

- architecture plus simple ;
- développement rapide ;
- moins de code backend ;
- intégration naturelle avec Supabase.

### Inconvénients

- logique métier potentiellement dispersée ;
- opérations complexes nécessitant davantage de fonctions serveur ;
- vigilance importante sur la sécurité.

---

## Option B — Flet + API métier + PostgreSQL

```text
Flet
  ↓
API Python
  ↓
Services
  ↓
Repositories
  ↓
PostgreSQL
```

### Avantages

- séparation claire ;
- logique métier centralisée ;
- API réutilisable par mobile et web ;
- contrôle fin des opérations.

### Inconvénients

- plus de code ;
- plus d'infrastructure ;
- plus de temps de développement ;
- maintenance plus importante.

---

# 22. 🎯 Recommandation pour le projet

Pour un ERP destiné à devenir un véritable SaaS multi-entreprises, une architecture avec **couche métier clairement séparée** est préférable à long terme.

Pour le MVP, il n'est cependant pas nécessaire de créer une architecture de microservices.

Une bonne approche pourrait être :

```text
🎨 Flet
   ↓
⚙️ Services métier
   ↓
📦 Repositories
   ↓
🗄️ PostgreSQL / Supabase
```

avec :

- Supabase Auth pour l'identité ;
- PostgreSQL pour les données ;
- RLS pour l'isolation ;
- services Python pour les workflows complexes ;
- repositories pour l'accès aux données.

L'architecture pourra évoluer ensuite vers une API dédiée si les besoins le justifient.

---

# 23. 📁 Structure de projet proposée

Une première structure pourrait être :

```text
transport_erp/
│
├── src/
│   │
│   ├── core/
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── exceptions.py
│   │   ├── logging.py
│   │   └── security.py
│   │
│   ├── domain/
│   │   ├── models/
│   │   ├── enums/
│   │   ├── rules/
│   │   └── exceptions/
│   │
│   ├── repositories/
│   │   ├── company_repository.py
│   │   ├── agency_repository.py
│   │   ├── trip_repository.py
│   │   ├── ticket_repository.py
│   │   ├── vehicle_repository.py
│   │   └── ...
│   │
│   ├── services/
│   │   ├── auth_service.py
│   │   ├── trip_service.py
│   │   ├── ticket_service.py
│   │   ├── fleet_service.py
│   │   ├── finance_service.py
│   │   └── ...
│   │
│   ├── app/
│   │   ├── router.py
│   │   ├── container.py
│   │   └── state.py
│   │
│   └── ui/
│       ├── views/
│       ├── components/
│       ├── layouts/
│       ├── theme/
│       └── navigation/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── docs/
│
├── migrations/
│
├── .env.example
├── pyproject.toml
└── README.md
```

Cette structure est une proposition et pourra être simplifiée ou adaptée.

---

# 24. 🧠 AppContainer / Dependency Injection

L'application peut utiliser un conteneur central pour fournir les dépendances.

Conceptuellement :

```text
AppContainer
│
├── Database
├── Repositories
├── Services
├── Auth
└── Configuration
```

Puis :

```text
View
 ↓
Service
 ↓
Repository
 ↓
Database
```

Cela évite de recréer des connexions et des dépendances partout dans le code.

---

# 25. 🧭 Router

La navigation doit être centralisée.

Exemple :

```text
/login
/dashboard
/agencies
/vehicles
/drivers
/trips
/tickets
/cash
/maintenance
/reports
/settings
```

Le Router détermine quelle vue afficher.

Les vues ne doivent pas connaître toute la structure de l'application.

---

# 26. 🎨 UI et responsive

L'application doit pouvoir évoluer vers :

- 💻 desktop ;
- 📱 mobile ;
- 📟 tablette ;
- 🌐 web.

Le layout devra donc être responsive.

Exemple conceptuel :

```text
Desktop
┌──────────┬──────────────────────┐
│ Sidebar  │       Content        │
│          │                      │
└──────────┴──────────────────────┘

Mobile
┌──────────────────────┐
│ Header               │
├──────────────────────┤
│ Content              │
├──────────────────────┤
│ Navigation           │
└──────────────────────┘
```

---

# 27. 🧪 Stratégie de tests

Les tests doivent être répartis par niveau.

## Unitaires

Tester les règles et services isolément.

Exemple :

```text
❌ vendre un siège déjà vendu
```

## Intégration

Tester les interactions :

```text
Service
   ↓
Repository
   ↓
Database
```

## End-to-end

Tester les workflows complets :

```text
Créer voyage
   ↓
Vendre billet
   ↓
Encaisser
   ↓
Embarquer
   ↓
Clôturer
```

---

# 28. 👥 Organisation du travail à 4 développeurs

Une répartition possible :

## 👤 Lead / Product / Fullstack

Responsabilités :

- architecture ;
- décisions techniques ;
- coordination ;
- code transverse ;
- domaine voyage ;
- revue de code.

## 👤 Développeur 2 — Commercial

Responsabilités :

- billetterie ;
- réservations ;
- sièges ;
- passagers.

## 👤 Développeur 3 — Exploitation

Responsabilités :

- flotte ;
- chauffeurs ;
- planification ;
- maintenance.

## 👤 Développeur 4 — Finance / Platform

Responsabilités :

- finance ;
- caisses ;
- authentification ;
- reporting ;
- infrastructure selon les besoins.

Cette répartition est indicative.

---

# 29. 🌿 Git et branches

Le développement doit éviter le travail directement sur `main`.

Exemple :

```text
main
 │
 ├── feature/ticket-sale
 ├── feature/trip-planning
 ├── feature/fleet
 └── feature/cash-management
```

Workflow :

```text
Feature branch
      ↓
Pull Request
      ↓
Review
      ↓
Tests
      ↓
Merge
      ↓
main
```

---

# 30. 🧾 Definition of Done

Une fonctionnalité n'est pas terminée simplement parce que :

> « le code fonctionne sur mon PC ».

Elle doit idéalement respecter :

```text
☑ Code écrit
☑ Tests ajoutés
☑ Règles métier respectées
☑ Gestion des erreurs
☑ Permissions vérifiées
☑ Review effectuée
☑ Documentation mise à jour
☑ Pas de régression
```

Cette règle doit être commune aux quatre développeurs.

---

# 31. 🚀 Environnements

Il est recommandé de séparer au minimum :

```text
🧑‍💻 Development
       ↓
🧪 Staging / Test
       ↓
🚀 Production
```

### Development

Travail quotidien des développeurs.

### Staging

Version proche de la production pour tests collectifs.

### Production

Données réelles des entreprises clientes.

> ⚠️ Les données de production ne doivent pas servir de terrain de test improvisé.

---

# 32. 🔐 Gestion des secrets

Les secrets ne doivent jamais être commités dans Git.

Exemple :

```text
❌ SUPABASE_KEY = "..."
❌ DATABASE_PASSWORD = "..."
```

Préférer :

```text
.env
```

et un fichier :

```text
.env.example
```

contenant uniquement les noms des variables.

---

# 33. 📊 Observabilité

Le système devra progressivement disposer de :

- logs ;
- erreurs ;
- métriques ;
- alertes ;
- suivi des performances.

Exemple :

```text
Utilisateur
    ↓
Action
    ↓
Service
    ↓
Erreur
    ↓
Log
    ↓
Monitoring
```

Le niveau de sophistication pourra rester limité dans le MVP.

---

# 34. 💾 Sauvegardes et récupération

Les données métier sont critiques.

Le système devra prévoir :

- sauvegardes automatiques ;
- restauration ;
- stratégie de rétention ;
- procédure en cas d'incident.

Ces aspects doivent être vérifiés avec le fournisseur d'infrastructure choisi.

---

# 35. 🔄 Évolutivité

L'architecture doit permettre d'ajouter progressivement :

```text
MVP
 ↓
📦 Colis
 ↓
💳 Paiements
 ↓
📱 Mobile
 ↓
🌐 Portail passager
 ↓
📍 GPS
 ↓
📊 Analytics avancés
 ↓
🤝 Marketplace
```

Sans réécrire entièrement le cœur du système.

---

# 36. 🚫 Microservices : pas au MVP

Le projet ne doit pas commencer par une architecture composée de dizaines de services.

Pour quatre développeurs et un premier produit :

> **Un monolithe modulaire bien conçu est probablement plus adapté.**

Architecture cible initiale :

```text
┌───────────────────────────────┐
│       ERP TRANSPORT           │
│                               │
│ Admin | Exploit | Billetterie │
│ Finance | Flotte | Reporting  │
│                               │
└───────────────┬───────────────┘
                │
                ▼
        PostgreSQL / Supabase
```

Les frontières de domaines doivent néanmoins être suffisamment propres pour permettre une évolution ultérieure.

---

# 37. 🧠 Risques techniques à surveiller

## 🔴 Risque 1 — Logique métier dans l'UI

Conséquence :

> code difficile à tester et maintenir.

## 🔴 Risque 2 — Accès DB partout

Conséquence :

> dépendances fortes et architecture incohérente.

## 🔴 Risque 3 — Permissions uniquement côté interface

Conséquence :

> faille de sécurité.

## 🔴 Risque 4 — Pas de contraintes DB

Conséquence :

> incohérences et doubles ventes.

## 🔴 Risque 5 — Trop d'abstraction trop tôt

Conséquence :

> développement ralenti inutilement.

## 🔴 Risque 6 — Architecture microservices prématurée

Conséquence :

> complexité opérationnelle disproportionnée.

---

# 38. 🧭 Architecture technique cible

Une vue simplifiée :

```text
                 👤 UTILISATEUR
                       │
                       ▼
                  🎨 FLET UI
                       │
                       ▼
                ⚙️ APPLICATION
                       │
                       ▼
                 🧠 SERVICES
                       │
                       ▼
                📦 REPOSITORIES
                       │
                       ▼
             🗄️ POSTGRESQL / SUPABASE
                       │
              ┌────────┴────────┐
              ▼                 ▼
          🔐 AUTH / RLS       📊 DATA
```

Services transversaux :

```text
🔐 Auth
🧾 Audit
🔔 Notifications
📊 Reporting
⚙️ Configuration
```

---

# 39. 🎯 Conclusion

L'architecture technique proposée repose sur un principe simple :

> **L'interface ne doit pas être le cœur de l'application. Le métier doit être au centre.**

La chaîne principale est :

```text
🎨 UI
 ↓
⚙️ Service
 ↓
🧠 Domaine
 ↓
📦 Repository
 ↓
🗄️ Database
```

Cette organisation permettra :

- à plusieurs développeurs de travailler en parallèle ;
- de tester les règles métier ;
- de faire évoluer l'interface ;
- de protéger les données ;
- de conserver une base de code maintenable ;
- d'évoluer progressivement vers une API ou des applications mobiles.

---

## 📌 Prochaine étape

**Document 09 — Roadmap & organisation du développement**

Nous allons maintenant transformer l'ensemble de la documentation en plan de travail pour l'équipe :

- 🗓️ phases du projet ;
- 🏁 jalons ;
- 👥 répartition des responsabilités ;
- 📦 lots fonctionnels ;
- 🧪 stratégie de validation ;
- 🌿 organisation Git ;
- 📋 backlog initial ;
- 🎯 objectifs des premières semaines ;
- 🚀 préparation du pilote.

Ce sera le document qui permettra de passer concrètement de **« nous avons une idée »** à **« voici comment les quatre développeurs vont commencer lundi »**.
