# 🧑‍💻 ERP TRANSPORT
## Document 14 — Standards de développement & règles de code

**Version : 0.1 — Document de travail**  
**Statut : Standard d'équipe proposé**  
**Documents liés :**
- `08_architecture_technique.md`
- `09_roadmap_organisation.md`
- `10_backlog_initial_sprint_0.md`
- `13_workflows_techniques_bout_en_bout.md`

---

# 1. 🎯 Objectif

Ce document définit les règles communes que les quatre développeurs devront suivre.

L'objectif est simple :

> **Le projet doit rester cohérent même lorsque plusieurs personnes écrivent du code en parallèle.**

Sans conventions communes, un projet d'équipe peut rapidement devenir :

```text
👨‍💻 Dev A → sa manière
👨‍💻 Dev B → sa manière
👨‍💻 Dev C → sa manière
👨‍💻 Dev D → sa manière
              ↓
          🍝 Spaghetti
```

Avec des standards :

```text
👨‍💻 A
👨‍💻 B
👨‍💻 C
👨‍💻 D
   ↓
📐 Même architecture
   ↓
🧱 Même conventions
   ↓
🚀 Produit cohérent
```

---

# 2. 🧭 Principe général

Le projet doit privilégier :

- simplicité ;
- lisibilité ;
- testabilité ;
- sécurité ;
- cohérence ;
- faible couplage ;
- responsabilité claire.

Le code doit être écrit pour être **lu et maintenu par les autres**, pas uniquement par son auteur.

---

# 3. 📁 Structure générale

Structure de référence :

```text
transport_erp/
│
├── src/
│   ├── core/
│   ├── domain/
│   ├── repositories/
│   ├── services/
│   ├── app/
│   └── ui/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── migrations/
├── docs/
├── scripts/
│
├── .env.example
├── .gitignore
├── pyproject.toml
└── README.md
```

---

# 4. 🧱 Règle fondamentale des couches

Le sens des dépendances doit rester clair :

```text
UI
 ↓
Application / Services
 ↓
Domain
 ↓
Repositories
 ↓
Infrastructure / Database
```

Éviter les dépendances inverses.

---

# 5. 🎨 Règles UI

Les Views Flet doivent :

- afficher ;
- collecter ;
- déclencher des actions ;
- afficher les résultats.

Elles ne doivent pas contenir de logique métier importante.

### ❌ À éviter

```text
Button
 ↓
SQL direct
 ↓
UPDATE tickets
```

### ✅ Préférer

```text
Button
 ↓
TicketService.sell_ticket()
 ↓
Repository
 ↓
Database
```

---

# 6. 🧩 Composants réutilisables

Les composants génériques doivent être réutilisés.

Exemples :

```text
PrimaryButton
DataTable
SearchField
ConfirmDialog
LoadingState
ErrorState
EmptyState
StatusBadge
```

Éviter de recréer un composant presque identique dans chaque View.

---

# 7. 🎨 Design System

Les éléments visuels communs doivent être centralisés :

```text
ui/theme/
├── colors.py
├── typography.py
├── spacing.py
├── dimensions.py
└── theme.py
```

Les développeurs ne doivent pas inventer chacun leurs propres couleurs ou espacements.

---

# 8. 📱 Responsive

Les écrans doivent être conçus pour les différentes tailles.

Le développeur doit réfléchir :

```text
💻 Desktop
📱 Mobile
📟 Tablet
```

avant de considérer un écran terminé.

---

# 9. 🧠 Domain Models

Les modèles métier doivent rester simples et explicites.

Exemple :

```text
Trip
Vehicle
Ticket
Passenger
Payment
CashSession
```

Ils ne doivent pas devenir des objets contenant toute l'application.

---

# 10. ⚙️ Services

Un service représente généralement une opération métier ou un groupe cohérent d'opérations.

Exemple :

```text
TicketService
TripService
FleetService
FinanceService
```

## Règle

Le service orchestre.

Il ne doit pas devenir :

```text
5000 lignes
```

si plusieurs responsabilités peuvent être séparées.

---

# 11. 📦 Repositories

Les repositories encapsulent l'accès aux données.

Exemple :

```text
TicketRepository
TripRepository
VehicleRepository
PaymentRepository
```

Ils doivent éviter de contenir des règles métier complexes.

### Repository

> « Récupérer le ticket. »

### Service

> « Est-il possible de l'annuler ? »

---

# 12. 🧠 Règles métier

Les règles importantes doivent être identifiables.

Exemples :

```text
TripRules
TicketRules
CashRules
VehicleRules
ReservationRules
```

Une règle comme :

> Un billet déjà embarqué ne peut pas être annulé normalement.

ne doit pas être cachée dans une View.

---

# 13. 🚨 Gestion des erreurs métier

Utiliser des exceptions métier explicites.

Exemples :

```text
TripNotFoundError
TripNotOpenError
SeatUnavailableError
UnauthorizedActionError
CashSessionNotOpenError
TicketAlreadyCancelledError
RefundNotAllowedError
VehicleUnavailableError
DriverUnavailableError
```

Cela permet à l'UI de présenter des messages adaptés.

---

# 14. 🎨 Traduction des erreurs pour l'utilisateur

Une exception technique ne doit pas être affichée directement.

### ❌

```text
IntegrityError: duplicate key violates constraint...
```

### ✅

> ⚠️ Ce siège vient d'être vendu par un autre agent.

---

# 15. 🗄️ Accès à la base

Le SQL doit être centralisé dans les repositories ou dans une couche d'accès clairement définie.

Éviter :

```text
View
 ↓
SQL
```

ou :

```text
Service
 ↓
50 requêtes SQL dispersées
```

---

# 16. 🔒 Transactions

Les opérations critiques doivent être atomiques.

Exemple :

```text
Vente billet
 ├── Ticket
 ├── Payment
 ├── CashTransaction
 └── Seat
```

Ces opérations doivent être traitées comme une unité logique.

---

# 17. 🔐 Sécurité

Les développeurs doivent appliquer le principe :

> **Never trust the client.**

Tout ce qui vient de l'interface doit être considéré comme potentiellement invalide.

Exemples :

```text
company_id
agency_id
user_id
amount
ticket_id
trip_id
```

doivent être validés côté serveur / backend et protégés par les mécanismes DB appropriés.

---

# 18. 🏢 Multi-tenant

Chaque fonctionnalité doit répondre à :

> À quelle entreprise appartient cette donnée ?

Avant de créer une requête métier, vérifier :

```text
company_id
agency_id
permissions
```

---

# 19. 🛡️ RLS

Les développeurs doivent comprendre que RLS n'est pas un simple paramètre Supabase.

Toute nouvelle table exposée au client doit être examinée sous l'angle :

```text
SELECT
INSERT
UPDATE
DELETE
```

Une table sans politique correctement pensée constitue un risque.

---

# 20. 🧾 Audit

Les opérations sensibles doivent être auditables.

Exemples :

```text
Ticket sold
Ticket cancelled
Refund
Role changed
Cash closed
Trip cancelled
Vehicle status changed
```

Le développeur doit déterminer si une nouvelle fonctionnalité nécessite un événement d'audit.

---

# 21. 🧪 Tests obligatoires

Toute fonctionnalité métier importante doit avoir des tests.

Au minimum :

```text
☑ Cas nominal
☑ Cas invalide
☑ Cas limite
☑ Cas permission
```

Pour les opérations concurrentes :

```text
☑ Cas simultané
```

---

# 22. 🧪 Tests unitaires

Les tests unitaires doivent tester les règles indépendamment de l'interface.

Exemple :

```text
test_cannot_cancel_boarded_ticket()
```

Pas besoin de lancer Flet pour ce type de test.

---

# 23. 🧪 Tests d'intégration

Ils vérifient :

```text
Service
 ↓
Repository
 ↓
PostgreSQL
```

Exemples :

- transaction ;
- contraintes ;
- RLS ;
- données relationnelles.

---

# 24. 🧪 Tests End-to-End

Tester un workflow complet.

Exemple :

```text
Créer voyage
 ↓
Ouvrir ventes
 ↓
Vendre billet
 ↓
Encaisser
 ↓
Embarquer
 ↓
Départ
 ↓
Arrivée
```

---

# 25. 🧪 Données de test

Ne pas utiliser les données réelles des clients pour les tests.

Créer un environnement :

```text
Demo Transport
```

avec :

- agences ;
- véhicules ;
- chauffeurs ;
- lignes ;
- voyages ;
- passagers.

---

# 26. 🐍 Style Python

Adopter une convention commune basée sur les standards Python.

Principes :

- noms explicites ;
- fonctions courtes ;
- imports propres ;
- typage lorsque pertinent ;
- docstrings pour les éléments publics ;
- pas de code mort ;
- pas de duplication inutile.

---

# 27. 🏷️ Nommage

### Classes

```text
PascalCase
```

Exemple :

```text
TicketService
TripRepository
CashSession
```

### Fonctions

```text
snake_case
```

Exemple :

```text
sell_ticket()
get_available_seats()
close_cash_session()
```

### Constantes

```text
UPPER_SNAKE_CASE
```

Exemple :

```text
DEFAULT_CURRENCY
MAX_TICKET_COUNT
```

---

# 28. 📦 Imports

Éviter les imports circulaires.

Une dépendance comme :

```text
Service A → Service B
Service B → Service A
```

doit être considérée comme un signal d'alerte architectural.

---

# 29. 🧩 DTO / Commands

Les opérations complexes peuvent utiliser des objets de commande.

Exemple :

```text
SellTicketCommand
CreateTripCommand
CloseCashSessionCommand
```

Avantage :

```text
UI
 ↓
Command
 ↓
Service
```

Cela rend les entrées explicites.

---

# 30. 📖 Nommage des méthodes

Préférer :

```text
sell_ticket()
cancel_ticket()
refund_ticket()
open_cash_session()
close_cash_session()
assign_vehicle()
assign_driver()
```

à des noms vagues :

```text
process()
handle()
do_action()
manage()
```

sauf lorsque le contexte les justifie réellement.

---

# 31. 📏 Taille des fonctions

Une fonction qui fait :

```text
500 lignes
```

est presque toujours un signal qu'elle doit être découpée.

Préférer :

```text
validate_trip()
check_vehicle()
check_driver()
create_trip()
```

---

# 32. 🚫 DRY avec discernement

Éviter la duplication inutile.

Mais ne pas créer une abstraction complexe uniquement parce que deux lignes se ressemblent.

Principe :

> **Duplication temporaire compréhensible > abstraction prématurée incompréhensible.**

---

# 33. 🧠 YAGNI

Ne pas développer une fonctionnalité simplement parce qu'elle pourrait être utile un jour.

Exemple :

```text
MVP
 ↓
Pas de système international complexe
 ↓
Pas de marketplace
 ↓
Pas de microservices
```

On construit ce dont le produit a réellement besoin.

---

# 34. 🔀 Git

Branches :

```text
main
feature/*
fix/*
```

Exemple :

```text
feature/ticket-sale
feature/trip-planning
fix/cash-closing
```

---

# 35. 📝 Commits

Format recommandé :

```text
feat: add trip creation
feat: add ticket sale
fix: prevent duplicate seat sale
refactor: split ticket service
test: add cash closing tests
docs: update booking workflow
```

Un commit doit idéalement représenter une modification cohérente.

---

# 36. 🔀 Pull Requests

Une Pull Request doit expliquer :

```text
🎯 Objectif
🧩 Ce qui a changé
🧪 Tests réalisés
⚠️ Points particuliers
```

Éviter les PR gigantesques.

---

# 37. 👀 Code Review

Le reviewer ne doit pas uniquement vérifier :

> « Ça marche ? »

Il doit aussi regarder :

- architecture ;
- sécurité ;
- lisibilité ;
- tests ;
- performances ;
- cohérence avec les règles métier.

---

# 38. 🚫 Aucun merge sans tests

Une PR importante ne doit pas être fusionnée si :

```text
❌ Tests cassés
❌ Erreurs de lint
❌ Régression connue
❌ RLS non vérifiée
```

sauf décision explicite et documentée.

---

# 39. 📋 Definition of Done

Une fonctionnalité est terminée lorsque :

```text
☑ Code
☑ Tests
☑ Validation métier
☑ Permissions
☑ Gestion erreurs
☑ Review
☑ Documentation
☑ Intégration
```

---

# 40. 🧾 Documentation technique

Les décisions importantes doivent être documentées.

Exemple :

```text
docs/
├── architecture/
├── workflows/
├── database/
├── decisions/
└── api/
```

---

# 41. 🧠 ADR

Une décision architecturale importante doit pouvoir être expliquée.

Exemple :

```text
ADR-001

Décision :
Monolithe modulaire.

Pourquoi :
- équipe de 4 ;
- MVP ;
- simplicité ;
- vitesse.
```

---

# 42. 🚨 Logs

Les logs doivent être utiles.

Un bon log permet de comprendre :

```text
Quand ?
Qui ?
Quelle action ?
Quel résultat ?
Quelle erreur ?
```

Éviter les logs contenant des données personnelles ou financières inutiles.

---

# 43. 🔐 Secrets

Interdit :

```text
SUPABASE_KEY = "clé réelle"
```

dans Git.

Utiliser :

```text
.env
```

et :

```text
.env.example
```

---

# 44. 🌳 Environnements

Minimum :

```text
Development
Staging
Production
```

Les développeurs doivent savoir sur quel environnement ils travaillent.

---

# 45. 🚀 Déploiement

Le déploiement doit idéalement être reproductible.

Objectif :

```text
Git
 ↓
Build
 ↓
Tests
 ↓
Deploy
```

À terme, une CI/CD pourra automatiser une grande partie du processus.

---

# 46. 🧪 Avant chaque release

Checklist :

```text
☑ Tests verts
☑ Migrations vérifiées
☑ RLS vérifiée
☑ Variables d'environnement
☑ Logs
☑ Sauvegarde
☑ Smoke tests
```

---

# 47. 🚨 Gestion des bugs

Un bug critique doit être classé.

### 🔴 P0

Bloque le fonctionnement ou crée un risque financier/sécurité.

### 🟠 P1

Fonction importante dégradée.

### 🟡 P2

Problème non bloquant.

### 🔵 P3

Amélioration / confort.

---

# 48. 🔥 Exemples P0

```text
❌ Double vente possible
❌ Accès Company A aux données B
❌ Paiement enregistré mais billet absent
❌ Caisse incohérente
❌ Remboursement incorrect
```

Ces bugs passent avant les nouvelles fonctionnalités.

---

# 49. 🧠 Règle de priorité

Lorsqu'un choix oppose :

```text
Nouvelle fonctionnalité
        VS
Correction d'un bug critique
```

le bug critique gagne.

---

# 50. 🤝 Communication entre développeurs

Un développeur doit signaler rapidement :

- blocage ;
- changement d'architecture ;
- dépendance ;
- risque ;
- décision nécessaire.

Éviter :

> « Je vais essayer de régler ça seul pendant trois jours. »

---

# 51. 🧠 Règle de synchronisation

Lorsqu'un développeur modifie une partie transverse :

```text
core
database
domain
auth
security
UI shared components
```

il doit prévenir l'équipe.

---

# 52. 🚨 Pas de refactoring sauvage

Ne pas réorganiser massivement le projet dans une feature sans discussion.

Exemple :

```text
Feature ticket
     ↓
Refactor complet de l'application
```

❌ Mauvaise idée.

Préférer :

```text
Feature
 ↓
Refactor nécessaire
 ↓
ADR / discussion si impact important
```

---

# 53. 🧪 Prototype avant architecture complexe

Lorsqu'une technologie ou une solution est incertaine :

```text
Question
 ↓
Petit prototype
 ↓
Mesure
 ↓
Décision
```

Ne pas passer deux semaines à débattre sans preuve lorsqu'un prototype de quelques heures peut répondre.

---

# 54. 👥 Rôle du Lead

Le Lead doit notamment :

- protéger l'architecture ;
- arbitrer ;
- maintenir les standards ;
- organiser les revues ;
- débloquer l'équipe ;
- garder le focus MVP.

Mais :

> ❌ Le Lead ne doit pas devenir le goulot d'étranglement de toutes les décisions.

Les développeurs doivent pouvoir décider localement lorsque la décision reste dans leur domaine.

---

# 55. 🧠 Règle d'escalade

Une décision peut être prise par le développeur si :

```text
Impact faible
+
Pas de changement architectural
+
Pas de risque sécurité
```

Elle doit être discutée si :

```text
Impact transversal
OU
Sécurité
OU
Base de données
OU
Architecture
OU
Coût important
```

---

# 56. 🏗️ Exemple de bonne collaboration

Développeur :

> « J'ai besoin d'ajouter `trip_seats` pour gérer correctement les réservations concurrentes. Cela touche la DB, RLS et le service billetterie. Je propose cette structure... »

Lead :

> revue + discussion

Équipe :

> décision

Puis :

```text
ADR
 ↓
Migration
 ↓
Code
 ↓
Tests
```

---

# 57. 🧭 Principe final

Le standard d'équipe peut être résumé ainsi :

```text
🧠 Comprendre
 ↓
🎯 Définir
 ↓
🧱 Concevoir
 ↓
💻 Coder
 ↓
🧪 Tester
 ↓
👀 Relire
 ↓
🔀 Intégrer
 ↓
🚀 Livrer
```

---

# 58. 🏁 Conclusion

L'objectif de ces standards n'est pas de créer une bureaucratie.

L'objectif est de permettre à quatre développeurs de produire **un seul produit cohérent**.

La règle fondamentale :

> **La vitesse d'une équipe ne vient pas du fait que chacun code vite. Elle vient du fait que chacun peut travailler rapidement sans casser le travail des autres.**

---

## 📌 Prochaine étape

**Document 15 — Plan de réunion officielle avec l'équipe**

Nous allons maintenant préparer précisément la rencontre :

- 🎤 ordre de présentation ;
- ⏱️ durée ;
- 🧠 messages importants à transmettre ;
- 🎯 ce que tu dois présenter en tant que porteur du projet ;
- 👥 ce que les développeurs doivent challenger ;
- 📝 décisions à prendre ;
- 📋 livrables de la réunion ;
- 🚀 lancement du Sprint 0.

Ce document pourra pratiquement servir de **script de réunion** pour votre première rencontre officielle.
