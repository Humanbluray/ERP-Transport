# 👥 ERP TRANSPORT
## Document 02 — Utilisateurs & Personas

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Document lié :** `01_vision_cadrage.md`

---

# 1. 🎯 Objectif du document

Ce document identifie les principaux acteurs qui interagiront avec l'ERP Transport et précise, pour chacun :

- son rôle dans l'entreprise ;
- ses objectifs ;
- ses responsabilités ;
- les informations dont il a besoin ;
- les fonctionnalités auxquelles il doit accéder ;
- les actions qu'il peut effectuer.

L'objectif est de concevoir le produit autour des **besoins réels des utilisateurs**, et non autour d'une simple liste de fonctionnalités techniques.

> 💡 **Principe :** chaque fonctionnalité doit répondre à un besoin métier porté par au moins un utilisateur identifié.

---

# 2. 🏢 Les grandes catégories d'utilisateurs

```text
                    ERP TRANSPORT
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
       ▼                 ▼                 ▼
   DIRECTION        EXPLOITATION       OPÉRATIONS
       │                 │                 │
       │                 │        ┌────────┼────────┐
       │                 │        ▼        ▼        ▼
       │                 │     GUICHET  CHAUFFEUR  CONTRÔLE
       │                 │
       └──────────┬──────┘
                  ▼
             ADMINISTRATION
```

À terme, la plateforme pourra également intégrer le **client/passager** comme utilisateur externe.

---

# 3. 👔 Direction / Administrateur de l'entreprise

## Profil

Dirigeant, directeur général, directeur d'exploitation ou responsable disposant d'une vision globale de l'entreprise.

## 🎯 Objectif

Piloter l'activité et prendre des décisions à partir de données fiables.

## Principales questions auxquelles le système doit répondre

- 💰 Combien l'entreprise a-t-elle encaissé aujourd'hui ?
- 📈 Quel est le chiffre d'affaires par période ?
- 🚌 Quels véhicules sont les plus performants ?
- 🛣️ Quelles lignes sont les plus rentables ?
- 🎫 Quel est le taux de remplissage moyen ?
- ⛽ Combien dépense-t-on en carburant ?
- 🔧 Combien coûte la maintenance ?
- ⚠️ Quels véhicules sont immobilisés ?
- 👨‍✈️ Quels chauffeurs ont effectué quels voyages ?
- 📊 Quelle est la rentabilité réelle d'un voyage ?

## Fonctionnalités principales

- Tableau de bord ;
- rapports ;
- statistiques ;
- consultation des agences ;
- consultation de la flotte ;
- consultation des voyages ;
- consultation de la billetterie ;
- consultation des recettes et dépenses ;
- suivi des performances ;
- gestion des utilisateurs selon ses droits.

## 🔐 Niveau d'accès

Élevé.

La direction doit avoir une vision globale, mais les droits d'administration technique de la plateforme doivent rester distincts du rôle de dirigeant.

---

# 4. 🗓️ Responsable d'exploitation

## Profil

Personne chargée de l'organisation quotidienne des opérations de transport.

## 🎯 Objectif

S'assurer que les voyages sont correctement planifiés et exécutés.

## Responsabilités

- créer les voyages ;
- planifier les départs ;
- gérer les horaires ;
- affecter les véhicules ;
- affecter les chauffeurs ;
- contrôler la disponibilité des ressources ;
- gérer les changements ;
- gérer les annulations ;
- suivre les départs ;
- suivre les arrivées ;
- gérer les incidents d'exploitation.

## Exemple

Pour un voyage :

> 🚌 Yaoundé → Douala  
> 📅 15 août 2026  
> 🕐 07h30

Le responsable sélectionne :

> Véhicule : BUS-023  
> Chauffeur : Jean  
> Capacité : 45 places

Le système doit pouvoir vérifier automatiquement :

- que le véhicule est disponible ;
- qu'il n'est pas déjà affecté à un autre voyage ;
- que le chauffeur est disponible ;
- que le véhicule peut être utilisé ;
- que les éventuelles contraintes d'exploitation sont respectées.

## 🔐 Niveau d'accès

Élevé sur les opérations, mais limité sur les données financières sensibles selon la politique de l'entreprise.

---

# 5. 🎫 Agent de guichet

## Profil

Utilisateur travaillant directement avec les clients dans une agence.

## 🎯 Objectif

Vendre et gérer les billets rapidement, avec un minimum d'erreurs.

## Fonctionnalités principales

L'agent doit pouvoir :

- rechercher un voyage ;
- consulter les places disponibles ;
- sélectionner un siège ;
- enregistrer un passager ;
- créer une réservation ;
- vendre un billet ;
- enregistrer le paiement ;
- imprimer ou transmettre le billet ;
- modifier une réservation selon les règles définies ;
- annuler un billet selon les règles définies ;
- consulter son historique ;
- gérer sa caisse selon ses droits.

## Exemple de workflow

```text
Recherche voyage
      ↓
Choix du voyage
      ↓
Choix du siège
      ↓
Informations passager
      ↓
Paiement
      ↓
Billet généré
      ↓
Caisse mise à jour
      ↓
Siège marqué comme occupé
```

## 🔐 Niveau d'accès

Limité au périmètre de son agence et aux opérations qui lui sont attribuées.

---

# 6. 🔧 Responsable de flotte

## Profil

Personne responsable de la disponibilité et de l'état des véhicules.

## 🎯 Objectif

Maintenir une flotte disponible, sûre et économiquement maîtrisée.

## Fonctionnalités principales

- créer un véhicule ;
- modifier ses informations ;
- consulter son historique ;
- suivre le kilométrage ;
- enregistrer le carburant ;
- planifier les entretiens ;
- enregistrer les maintenances ;
- gérer les immobilisations ;
- gérer les documents ;
- suivre les échéances ;
- consulter les coûts liés au véhicule.

## Exemple

```text
🚌 BUS-023

Kilométrage : 185 430 km
Statut : Disponible
Prochaine maintenance : 190 000 km
Assurance : expire dans 24 jours
Dernière maintenance : 175 000 km
```

## 🔐 Niveau d'accès

Élevé sur le module flotte.

---

# 7. 💰 Responsable financier / caissier principal

## Profil

Personne responsable du contrôle des flux financiers liés à l'exploitation.

## 🎯 Objectif

Contrôler les recettes, les dépenses et les mouvements de caisse.

## Fonctionnalités principales

- recettes ;
- dépenses ;
- caisses ;
- versements ;
- remboursements ;
- commissions ;
- rapprochements ;
- clôtures de caisse ;
- rapports financiers ;
- suivi des écarts.

## Principe important

Les opérations financières doivent rester liées aux opérations métier.

```text
🎫 Vente billet
      ↓
💰 Recette
      ↓
🏦 Caisse
      ↓
🚌 Voyage
      ↓
🛣️ Ligne
      ↓
📊 Analyse de rentabilité
```

---

# 8. 👨‍✈️ Chauffeur

## Profil

Personne chargée de conduire un véhicule pour un voyage ou une mission.

## 🎯 Objectif

Exécuter correctement la mission qui lui est attribuée et renseigner les informations nécessaires.

## Fonctionnalités possibles

Dans le MVP, le chauffeur pourra avoir un accès limité permettant de :

- consulter ses missions ;
- consulter le véhicule affecté ;
- confirmer une prise en charge ;
- renseigner le kilométrage ;
- renseigner le carburant ;
- signaler un incident ;
- confirmer le départ ;
- confirmer l'arrivée.

## 🔮 Évolution future

Ces fonctionnalités pourront être regroupées dans une application mobile chauffeur.

```text
📱 APPLICATION CHAUFFEUR

Mes missions
      ↓
Mon véhicule
      ↓
Départ
      ↓
Kilométrage
      ↓
Carburant
      ↓
Incident éventuel
      ↓
Arrivée
```

---

# 9. 🛂 Contrôleur / agent d'embarquement

## Profil

Personne chargée de vérifier les passagers avant ou pendant l'embarquement.

## 🎯 Objectif

S'assurer que les passagers présents disposent d'un billet valide.

## Fonctionnalités

- rechercher un billet ;
- scanner un QR Code ;
- vérifier le statut du billet ;
- confirmer l'embarquement ;
- identifier les billets annulés ou invalides ;
- consulter la liste des passagers du voyage.

```text
QR CODE
   ↓
Recherche billet
   ↓
Billet valide ?
   ├── ✅ Oui → Embarquement autorisé
   └── ❌ Non → Embarquement refusé
```

---

# 10. 🧑‍💼 Responsable d'agence

## Profil

Responsable d'une agence physique ou d'un point de vente.

## 🎯 Objectif

Piloter l'activité quotidienne de son agence.

## Responsabilités possibles

- supervision des agents ;
- suivi des ventes ;
- suivi des réservations ;
- suivi de la caisse ;
- consultation des départs ;
- gestion des opérations locales ;
- suivi des performances de l'agence.

## Indicateurs utiles

- billets vendus ;
- chiffre d'affaires ;
- taux de remplissage ;
- réservations ;
- annulations ;
- recettes par agent ;
- écarts de caisse.

---

# 11. 👤 Passager / client

## Profil

Personne utilisant le service de transport.

## 🎯 Objectif

Trouver un voyage, réserver ou acheter un billet et voyager avec un minimum de friction.

## Dans le MVP

Le passager pourra être principalement géré par l'agent de guichet.

Le système devra néanmoins prévoir :

- identité ;
- téléphone ;
- voyage ;
- siège ;
- billet ;
- paiement ;
- historique.

## 🔮 Évolution future

Le passager pourra disposer de son propre espace :

- recherche de voyages ;
- réservation ;
- paiement ;
- billet électronique ;
- QR Code ;
- historique ;
- annulation selon les règles ;
- notifications.

---

# 12. ⚙️ Administrateur de la plateforme SaaS

Ce rôle est différent de l'administrateur d'une entreprise cliente.

## 🎯 Objectif

Administrer l'environnement global de la plateforme.

## Fonctionnalités

- gestion des entreprises clientes ;
- gestion des abonnements ;
- gestion des plans ;
- supervision globale ;
- paramètres de plateforme ;
- journaux d'activité ;
- sécurité ;
- gestion des accès.

> ⚠️ **Ce rôle ne doit pas être confondu avec l'administrateur d'une entreprise de transport.**

---

# 13. 🔐 Synthèse des rôles

| Acteur | Fonction principale | Accès principal |
|---|---|---|
| 👔 Direction | Pilotage et décision | Dashboard, rapports, finances |
| 🗓️ Responsable exploitation | Organisation des voyages | Planning, voyages, affectations |
| 🧑‍💼 Responsable agence | Gestion d'une agence | Ventes, agents, caisse, départs |
| 🎫 Agent guichet | Vente et réservation | Billetterie, passagers, caisse |
| 🔧 Responsable flotte | Gestion des véhicules | Flotte, maintenance, carburant |
| 💰 Responsable financier | Contrôle financier | Recettes, dépenses, caisses |
| 👨‍✈️ Chauffeur | Exécution des missions | Missions, véhicule, incidents |
| 🛂 Contrôleur | Contrôle embarquement | Billets, passagers |
| 👤 Passager | Achat / réservation | Voyages, billets |
| ⚙️ Admin plateforme | Administration du SaaS | Entreprises, abonnements, sécurité |

---

# 14. 🔐 Principe de séparation des responsabilités

> **Un utilisateur ne doit accéder qu'aux informations et actions nécessaires à son rôle.**

Exemple :

```text
👔 DIRECTION
   ├── Dashboard
   ├── Finance
   ├── Exploitation
   ├── Flotte
   └── Rapports

🗓️ EXPLOITATION
   ├── Planning
   ├── Voyages
   ├── Véhicules
   └── Chauffeurs

🎫 AGENT
   ├── Billetterie
   ├── Réservations
   ├── Passagers
   └── Caisse

🔧 FLOTTE
   ├── Véhicules
   ├── Maintenance
   ├── Carburant
   └── Documents

👨‍✈️ CHAUFFEUR
   ├── Mes missions
   ├── Mon véhicule
   └── Incidents
```

Les permissions devront être pensées dès la conception et non ajoutées après le développement.

---

# 15. 🏢 Multi-entreprises / Multi-tenant

Le produit devra être pensé dès le départ comme un **SaaS multi-entreprises**.

Une même plateforme pourra héberger plusieurs entreprises de transport.

```text
                    🌐 PLATEFORME
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
     ENTREPRISE A   ENTREPRISE B   ENTREPRISE C
          │              │              │
       Agences        Agences        Agences
       Véhicules      Véhicules      Véhicules
       Voyages        Voyages        Voyages
       Utilisateurs   Utilisateurs   Utilisateurs
```

Les données d'une entreprise devront être strictement isolées de celles des autres entreprises.

Cette décision aura des conséquences importantes sur :

- 🗄️ le modèle de données ;
- 🔐 l'authentification ;
- 👥 les rôles ;
- 🛡️ les permissions ;
- 🔒 la sécurité ;
- 💳 les abonnements ;
- 💰 la facturation.

---

# 16. ❓ Points à valider avec l'équipe

Lors de la réunion de cadrage, plusieurs questions devront être discutées.

### Organisation

- Une entreprise peut-elle avoir plusieurs agences ?
- Une agence peut-elle vendre des billets pour plusieurs lignes ?
- Une même ligne peut-elle être exploitée depuis plusieurs agences ?

### Utilisateurs

- Un utilisateur peut-il appartenir à plusieurs agences ?
- Un agent peut-il travailler dans plusieurs agences ?
- Les rôles sont-ils fixes ou configurables par entreprise ?
- Faut-il prévoir des permissions personnalisées ?

### Chauffeurs

- Un chauffeur peut-il être affecté à plusieurs voyages dans une même journée ?
- Quelles règles doivent empêcher les conflits d'affectation ?
- Un chauffeur peut-il être rattaché à une agence particulière ?

### Véhicules

- Un véhicule peut-il être affecté à plusieurs voyages dans une journée ?
- Comment gérer une panne après affectation ?
- Que se passe-t-il lorsqu'un véhicule doit être remplacé ?

### Passagers

- Faut-il conserver un historique des voyages d'un passager ?
- Un passager peut-il avoir plusieurs réservations ?
- Quelles informations sont obligatoires pour acheter un billet ?

### Sécurité

- Qui peut annuler un billet ?
- Qui peut modifier un tarif ?
- Qui peut clôturer une caisse ?
- Qui peut modifier une recette ?
- Toutes les actions sensibles doivent-elles être journalisées ?

---

# 17. 🎯 Conclusion

La conception de l'ERP doit partir des utilisateurs et de leurs interactions avec le système.

L'objectif n'est pas de créer un écran pour chaque fonctionnalité, mais de construire une plateforme dans laquelle :

> **chaque utilisateur dispose des outils nécessaires à son travail, avec les informations pertinentes et les droits correspondant à ses responsabilités.**

Cette approche permettra ensuite de construire les workflows métier, puis de déduire les modules fonctionnels, les règles métier et enfin l'architecture technique.

---

## 📌 Prochaine étape

**Document 03 — Modules fonctionnels**

Objectif :

> Identifier les grands modules de l'ERP, leur périmètre, leurs responsabilités et leurs interactions.

Modules à étudier notamment :

- 🏢 Administration ;
- 🏪 Agences ;
- 📍 Référentiel lignes / destinations ;
- 🚌 Flotte ;
- 👨‍✈️ Chauffeurs ;
- 📅 Planification ;
- 🛣️ Voyages ;
- 🎫 Billetterie ;
- 💰 Finance / caisses ;
- 🔧 Maintenance ;
- 📦 Colis ;
- 📊 Reporting ;
- 🔔 Notifications.
