# 🏗️ ERP TRANSPORT
## Document 07 — Architecture fonctionnelle

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`
- `06_definition_mvp.md`

---

# 1. 🎯 Objectif du document

Ce document définit l'organisation fonctionnelle globale de l'ERP Transport.

Il répond principalement à la question :

> **Comment organiser toutes les fonctionnalités identifiées précédemment en un système cohérent ?**

À ce stade, nous ne choisissons pas encore définitivement :

- Python ;
- Flet ;
- Supabase ;
- PostgreSQL ;
- une architecture API précise ;
- un fournisseur cloud.

Nous définissons d'abord **l'architecture métier et fonctionnelle**.

---

# 2. 🧠 Principe général

L'ERP doit être conçu comme une plateforme composée de domaines fonctionnels cohérents.

```text
                         🌐 ERP TRANSPORT
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
          🏢 CORE          🚍 EXPLOITATION     💰 FINANCE
              │                 │                 │
              │          ┌──────┼──────┐          │
              │          ▼      ▼      ▼          │
              │        🚌      👨‍✈️    🛣️         │
              │      Flotte  Chauffeurs Voyages   │
              │                              │    │
              │                              ▼    │
              │                         🎫 Billets│
              │                              │    │
              └──────────────────────────────┼────┘
                                             ▼
                                      📊 REPORTING
```

L'idée fondamentale est de séparer les **responsabilités métier** tout en permettant aux domaines de collaborer.

---

# 3. 🧱 Les grands domaines fonctionnels

Nous pouvons regrouper les modules en six grands domaines.

## 3.1 🏢 Administration & Référentiels

Responsabilité :

- entreprise ;
- agences ;
- utilisateurs ;
- rôles ;
- permissions ;
- paramètres ;
- référentiels.

---

## 3.2 🚍 Exploitation

Responsabilité :

- flotte ;
- chauffeurs ;
- planification ;
- voyages ;
- embarquement ;
- incidents.

---

## 3.3 🎫 Commercial / Billetterie

Responsabilité :

- recherche ;
- réservations ;
- billets ;
- sièges ;
- tarifs ;
- passagers.

---

## 3.4 💰 Finance

Responsabilité :

- paiements ;
- caisses ;
- recettes ;
- remboursements ;
- dépenses ;
- rapprochements.

---

## 3.5 🔧 Gestion technique de la flotte

Responsabilité :

- maintenance ;
- carburant ;
- documents ;
- immobilisations ;
- historique des coûts.

---

## 3.6 📊 Pilotage & Reporting

Responsabilité :

- tableaux de bord ;
- indicateurs ;
- rapports ;
- statistiques ;
- alertes.

---

# 4. 🗺️ Architecture fonctionnelle globale

```text
                         🌐 PLATEFORME
                              │
       ┌──────────────────────┼──────────────────────┐
       │                      │                      │
       ▼                      ▼                      ▼
 🏢 ADMINISTRATION       🚍 EXPLOITATION        🎫 COMMERCIAL
       │                      │                      │
       │              ┌───────┼───────┐             │
       │              ▼       ▼       ▼             │
       │            🚌       👨‍✈️     🛣️            │
       │           Flotte Chauffeurs Voyages        │
       │                              │              │
       │                              ▼              │
       │                         🎫 Billetterie      │
       │                              │              │
       └───────────────┬──────────────┘              │
                       │                             │
                       ▼                             ▼
                  💰 FINANCE                    👤 CLIENTS
                       │
                       ▼
                  📊 REPORTING
                       │
                       ▼
                  🔔 ALERTES
```

---

# 5. 🏢 Domaine Administration

Ce domaine constitue le socle organisationnel.

## Modules

```text
🏢 Administration
   ├── Entreprise
   ├── Agences
   ├── Utilisateurs
   ├── Rôles
   ├── Permissions
   └── Paramètres
```

## Responsabilité

Il définit **qui utilise le système et dans quel périmètre**.

Il ne doit pas contenir la logique métier spécifique des voyages ou de la billetterie.

---

# 6. 📚 Domaine Référentiels

Les référentiels fournissent les données communes aux autres domaines.

```text
📚 Référentiels
   ├── Villes
   ├── Destinations
   ├── Lignes
   ├── Tarifs
   ├── Types de véhicules
   ├── Statuts
   └── Paramètres métier
```

## Principe

Les autres modules doivent consommer les référentiels plutôt que recréer leurs propres listes.

Exemple :

```text
🎫 Billetterie
      ↓
📚 Tarif de la ligne
```

---

# 7. 🚍 Domaine Exploitation

C'est le cœur opérationnel de l'entreprise.

```text
🚍 EXPLOITATION
      │
      ├── 🚌 Flotte
      │
      ├── 👨‍✈️ Chauffeurs
      │
      ├── 📅 Planification
      │
      ├── 🛣️ Voyages
      │
      └── ⚠️ Incidents
```

## Principe

L'exploitation doit pouvoir répondre à :

> **Quel véhicule, quel chauffeur, pour quel voyage, à quelle heure et vers quelle destination ?**

---

# 8. 🚌 Sous-domaine Flotte

Le domaine flotte gère le cycle de vie des véhicules.

```text
🚌 Véhicule
   │
   ├── Informations
   ├── Disponibilité
   ├── Documents
   ├── Kilométrage
   ├── ⛽ Carburant
   └── 🔧 Maintenance
```

## Relation avec les voyages

```text
🚌 Véhicule
      ↓
📅 Affectation
      ↓
🛣️ Voyage
```

Le module voyage ne doit pas dupliquer les données du véhicule.

Il référence le véhicule affecté.

---

# 9. 👨‍✈️ Sous-domaine Chauffeurs

```text
👨‍✈️ Chauffeur
   ├── Profil
   ├── Permis
   ├── Disponibilité
   ├── Affectations
   └── Incidents
```

## Relation

```text
👨‍✈️ Chauffeur
      ↓
📅 Affectation
      ↓
🛣️ Voyage
```

---

# 10. 📅 Sous-domaine Planification

La planification prépare les voyages.

Elle orchestre :

- ligne ;
- date ;
- heure ;
- véhicule ;
- chauffeur.

```text
📚 Ligne
   +
📅 Date / heure
   +
🚌 Véhicule
   +
👨‍✈️ Chauffeur
        ↓
    🛣️ Voyage
```

---

# 11. 🛣️ Domaine Voyage

Le voyage est l'un des objets centraux du système.

Il référence notamment :

- ligne ;
- date ;
- heure ;
- véhicule ;
- chauffeur ;
- capacité ;
- tarif ;
- statut.

Mais le voyage ne doit pas devenir un « objet fourre-tout ».

Il doit principalement gérer :

- son cycle de vie ;
- ses affectations ;
- son exploitation.

Les billets, paiements, maintenances et autres données doivent rester dans leurs domaines respectifs.

---

# 12. 🎫 Domaine Commercial

```text
🎫 COMMERCIAL
     │
     ├── 👤 Passagers
     ├── 💺 Sièges
     ├── 🟡 Réservations
     ├── 🎫 Billets
     └── 💵 Tarification
```

## Relation principale

```text
🛣️ Voyage
    ↓
🎫 Offre de places
    ↓
👤 Passager
    ↓
💺 Siège
    ↓
🎫 Billet
```

---

# 13. 💺 Gestion des sièges

La gestion des sièges dépend du voyage.

Il faut distinguer :

### Configuration

Le véhicule possède une configuration de sièges.

### Disponibilité

Le voyage possède une disponibilité de sièges.

Exemple :

```text
🚌 BUS-023
   ↓
Configuration : 35 places
   ↓
🛣️ Voyage V001
   ↓
Siège 12 = disponible
```

Le siège n'est donc pas simplement « vendu dans le véhicule ».

Il est vendu **dans le contexte d'un voyage**.

---

# 14. 🎫 Billetterie

Le billet représente une transaction commerciale liée à un voyage.

```text
👤 Passager
      +
🛣️ Voyage
      +
💺 Siège
      +
💰 Paiement
      ↓
🎫 Billet
```

Le billet doit conserver son historique.

---

# 15. 💰 Domaine Finance

```text
💰 FINANCE
    │
    ├── Paiements
    ├── Recettes
    ├── Remboursements
    ├── Dépenses
    └── 🏦 Caisses
```

## Principe important

La finance ne doit pas être indépendante des opérations métier.

Exemple :

```text
🎫 Vente
   ↓
💰 Paiement
   ↓
🏦 Caisse
```

---

# 16. 🏦 Domaine Caisse

La caisse représente le niveau opérationnel des mouvements financiers.

```text
🏦 Caisse
   ├── Ouverture
   ├── Encaissements
   ├── Remboursements
   ├── Ajustements autorisés
   └── Clôture
```

La caisse peut être rattachée à une agence et/ou à un utilisateur selon les règles retenues.

---

# 17. 🔧 Domaine Maintenance

La maintenance reste liée à la flotte mais constitue un sous-domaine métier distinct.

```text
🚌 Véhicule
      ↓
🔧 Maintenance
      ├── Intervention
      ├── Pièces
      ├── Main-d'œuvre
      ├── Coût
      └── Immobilisation
```

Une maintenance peut modifier la disponibilité du véhicule.

---

# 18. ⛽ Domaine Carburant

```text
🚌 Véhicule
      ↓
⛽ Plein
      ├── Quantité
      ├── Prix
      ├── Kilométrage
      └── Date
```

Ces informations peuvent ensuite alimenter :

```text
⛽ Carburant
      ↓
📊 Consommation
      ↓
💰 Coût
      ↓
📈 Rentabilité
```

---

# 19. 📊 Domaine Reporting

Le reporting ne doit pas devenir une source primaire de données.

Il doit exploiter les données produites par les autres domaines.

```text
🛣️ Voyages
🎫 Billets
💰 Finance
🚌 Flotte
⛽ Carburant
🔧 Maintenance
      │
      ▼
📊 Reporting
```

## Principe

> Les tableaux de bord doivent refléter les données opérationnelles réelles.

---

# 20. 🔔 Domaine Notifications

Les notifications peuvent être déclenchées par différents domaines.

```text
🔧 Maintenance
      ↓
🔔 Alerte maintenance

🎫 Billetterie
      ↓
🔔 Billet confirmé

🛣️ Voyage
      ↓
🔔 Voyage modifié

📦 Colis
      ↓
🔔 Colis disponible
```

Le service de notification doit idéalement être transversal plutôt que copié dans chaque module.

---

# 21. 🧾 Domaine Audit

L'audit est également transversal.

```text
🏢 Administration
🛣️ Voyages
🎫 Billetterie
💰 Finance
🚌 Flotte
      │
      ▼
🧾 AUDIT
```

Il enregistre les actions importantes.

---

# 22. 🔗 Dépendances entre domaines

Une architecture fonctionnelle saine doit éviter les dépendances circulaires inutiles.

Exemple acceptable :

```text
📚 Référentiels
      ↓
📅 Planification
      ↓
🛣️ Voyage
      ↓
🎫 Billetterie
      ↓
💰 Finance
```

Mais il faut éviter :

```text
Voyage → Billetterie
   ↑          ↓
   └──────────┘
```

si cette dépendance devient structurelle et crée un couplage difficile à maintenir.

---

# 23. 🧠 Principe de responsabilité

Chaque domaine doit avoir une responsabilité claire.

| Domaine | Responsabilité |
|---|---|
| 🏢 Administration | Organisation et accès |
| 📚 Référentiels | Données de référence |
| 🚌 Flotte | Véhicules |
| 👨‍✈️ Chauffeurs | Conducteurs |
| 📅 Planification | Préparation des opérations |
| 🛣️ Voyages | Cycle de vie des voyages |
| 🎫 Billetterie | Vente et réservation |
| 💰 Finance | Flux financiers |
| 🔧 Maintenance | Entretien véhicules |
| ⛽ Carburant | Consommation |
| 📊 Reporting | Analyse |
| 🔔 Notifications | Communication |
| 🧾 Audit | Traçabilité |

---

# 24. 🏢 Architecture multi-tenant

Le produit est conçu comme un SaaS.

```text
                         🌐 PLATEFORME
                              │
                ┌─────────────┼─────────────┐
                ▼             ▼             ▼
          🏢 ENTREPRISE A 🏢 ENTREPRISE B 🏢 ENTREPRISE C
                │             │             │
             Agences       Agences       Agences
             Flotte        Flotte        Flotte
             Voyages       Voyages       Voyages
             Billets       Billets       Billets
```

## Principe fondamental

Toutes les ressources métier doivent appartenir à un **périmètre d'entreprise**.

Le système doit pouvoir déterminer :

```text
Utilisateur
    ↓
Entreprise
    ↓
Agence / périmètre
    ↓
Ressource
```

Cette règle sera essentielle lors de la conception de la base de données et de la sécurité.

---

# 25. 👥 Portée des utilisateurs

Un utilisateur peut avoir différents niveaux de portée.

Exemple :

```text
🌐 Plateforme
      ↓
🏢 Entreprise
      ↓
🏪 Agence
      ↓
👤 Utilisateur
```

La permission ne doit donc pas être seulement :

> « Peut-il vendre un billet ? »

mais potentiellement :

> « Peut-il vendre un billet **pour cette agence et cette entreprise** ? »

---

# 26. 🔄 Flux fonctionnels principaux

## Flux A — Exploitation

```text
📚 Référentiel
      ↓
📅 Planification
      ↓
🚌 Flotte + 👨‍✈️ Chauffeur
      ↓
🛣️ Voyage
```

## Flux B — Commercial

```text
🛣️ Voyage
      ↓
🎫 Billetterie
      ↓
👤 Passager
      ↓
💺 Siège
      ↓
🎫 Billet
```

## Flux C — Finance

```text
🎫 Billet
      ↓
💰 Paiement
      ↓
🏦 Caisse
      ↓
📊 Finance
```

## Flux D — Technique

```text
🚌 Véhicule
      ↓
⛽ Carburant
      +
🔧 Maintenance
      ↓
📊 Coût véhicule
```

---

# 27. 🧠 Architecture orientée domaines

Même si le produit est initialement développé comme une seule application, les responsabilités doivent être séparées conceptuellement.

```text
ERP TRANSPORT
│
├── Administration
├── Référentiels
├── Flotte
├── Chauffeurs
├── Planification
├── Voyages
├── Billetterie
├── Finance
├── Maintenance
├── Carburant
├── Reporting
├── Notifications
└── Audit
```

Cela permet de faire évoluer le produit plus facilement.

> 💡 Il n'est pas nécessaire de construire immédiatement des microservices. Une architecture modulaire bien séparée peut parfaitement convenir au MVP.

---

# 28. 🧩 Architecture fonctionnelle du MVP

Pour le MVP, nous pouvons réduire l'ensemble à :

```text
                   🚀 MVP
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
   🏢 CORE       🚍 EXPLOITATION  🎫 COMMERCIAL
       │             │             │
       │          🚌 Flotte       🎫 Billets
       │          👨‍✈️ Chauffeurs  💺 Sièges
       │          📅 Planning     👤 Passagers
       │          🛣️ Voyages
       │
       └─────────────┬─────────────┘
                     ▼
                  💰 FINANCE
                     │
                     ▼
                  📊 DASHBOARD
```

Les modules avancés peuvent rester en dehors du premier noyau.

---

# 29. 🧱 Frontières fonctionnelles

Les frontières doivent être suffisamment claires pour que deux développeurs puissent travailler sur deux domaines différents sans modifier constamment le code de l'autre.

Exemple :

### Développeur A

> 🚌 Flotte

### Développeur B

> 🎫 Billetterie

### Développeur C

> 🛣️ Voyages

### Développeur D

> 💰 Finance / infrastructure

Ils devront néanmoins collaborer sur les contrats entre domaines.

---

# 30. 🤝 Contrats entre domaines

Exemple :

La billetterie a besoin de savoir si une place est disponible.

Elle ne doit pas nécessairement connaître toute la logique interne de la flotte.

Elle demande conceptuellement :

```text
🎫 Billetterie
      │
      │ « Donne-moi les sièges disponibles
      │  pour le voyage V001 »
      ▼
🛣️ Voyage / Gestion des places
      │
      ▼
💺 Disponibilité
```

Même principe pour la finance :

```text
🎫 Billetterie
      │
      │ « Enregistrer paiement »
      ▼
💰 Finance
```

Cette notion de contrat sera très importante dans l'architecture technique.

---

# 31. 📱 Interfaces futures

L'architecture fonctionnelle doit permettre plusieurs interfaces.

## Aujourd'hui

```text
💻 Application ERP
```

## Demain

```text
💻 ERP Web / Desktop
       +
📱 Chauffeur
       +
📱 Contrôleur
       +
🌐 Portail passager
       +
📱 Application client
```

Toutes ces interfaces doivent pouvoir utiliser les mêmes règles métier centrales.

---

# 32. 🔐 Sécurité fonctionnelle

La sécurité doit être considérée comme une fonction transversale.

```text
👤 Utilisateur
      ↓
🔐 Authentification
      ↓
🎭 Rôle
      ↓
🛡️ Permissions
      ↓
🏢 Périmètre entreprise
      ↓
🏪 Périmètre agence
      ↓
📦 Ressource
```

La sécurité ne doit donc pas dépendre uniquement de l'interface.

> ⚠️ Masquer un bouton n'est pas une sécurité suffisante. Le serveur doit également contrôler les permissions.

---

# 33. 🧪 Architecture et testabilité

Chaque domaine doit pouvoir être testé indépendamment autant que possible.

Exemple :

```text
🎫 Billetterie
   ├── Vente billet
   ├── Annulation
   ├── Réservation
   └── Siège
```

Tests possibles :

- double vente ;
- réservation expirée ;
- billet annulé ;
- voyage fermé ;
- tarif incorrect.

---

# 34. 🚫 Ce que cette architecture ne décide pas encore

Ce document ne fixe pas encore :

- le langage ;
- le framework ;
- la base de données ;
- le fournisseur cloud ;
- l'hébergement ;
- la structure exacte des tables ;
- les API ;
- les bibliothèques ;
- la stratégie de déploiement.

Ces éléments appartiendront à l'architecture technique.

---

# 35. 🎯 Conclusion

L'architecture fonctionnelle fournit une carte du produit.

Elle permet à l'équipe de comprendre :

> **où chaque responsabilité doit vivre, quelles sont ses relations avec les autres domaines et quelles frontières doivent être respectées.**

Le principe central est :

```text
UNE RESPONSABILITÉ
       ↓
UN DOMAINE CLAIR
       ↓
DES CONTRATS ENTRE DOMAINES
       ↓
UNE ARCHITECTURE MAINTENABLE
```

Le produit peut être développé comme une application unique au départ, tout en conservant une séparation fonctionnelle suffisamment forte pour évoluer plus tard.

---

## 📌 Prochaine étape

**Document 08 — Architecture technique**

Nous pourrons maintenant traduire cette architecture fonctionnelle en architecture logicielle.

Nous aborderons :

- 🐍 Python ;
- 🎨 Flet ;
- 🌐 API ;
- 🗄️ PostgreSQL / Supabase ;
- 🔐 Authentification ;
- 🛡️ RLS / sécurité ;
- 🧱 couches `core`, `domain`, `services`, `repositories`, `views` ;
- 📦 modèles ;
- 🔄 flux de données ;
- 🧪 tests ;
- 🚀 déploiement ;
- 👥 organisation du code pour les 4 développeurs.

Ce document sera la première vraie passerelle entre **la vision produit que tu portes** et **le travail technique de l'équipe**.
