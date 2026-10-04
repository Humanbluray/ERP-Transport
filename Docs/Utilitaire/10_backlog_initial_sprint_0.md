# 🚀 ERP TRANSPORT
## Document 10 — Backlog initial & Sprint 0

**Version : 0.1 — Document de travail**  
**Statut : Préparation de la première réunion technique**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`
- `06_definition_mvp.md`
- `07_architecture_fonctionnelle.md`
- `08_architecture_technique.md`
- `09_roadmap_organisation.md`

---

# 1. 🎯 Objectif du document

Les documents précédents ont défini :

- pourquoi le produit existe ;
- pour qui il est conçu ;
- ce qu'il doit faire ;
- comment les processus métier fonctionnent ;
- quelles règles doivent être respectées ;
- ce qui appartient au MVP ;
- comment organiser le système ;
- comment l'équipe doit travailler.

Ce document transforme maintenant ces éléments en **travail concret**.

L'objectif est de permettre à l'équipe de passer de :

> 💡 « Nous avons une idée »

à :

> 🚀 « Voici les premières tâches que nous allons développer. »

---

# 2. 🧠 Définitions

## Epic

Une grande capacité fonctionnelle.

Exemple :

> 🎫 Gestion de la billetterie

---

## User Story

Une fonctionnalité décrite du point de vue de l'utilisateur.

Format :

> **En tant que [utilisateur], je veux [action], afin de [objectif].**

---

## Critères d'acceptation

Les conditions qui permettent de dire :

> ✅ Cette User Story est correctement terminée.

---

## Task

Une tâche technique nécessaire pour réaliser une User Story.

Exemple :

```text
User Story :
Créer un voyage

Tasks :
- créer modèle Trip
- créer repository
- créer service
- créer formulaire Flet
- ajouter tests
```

---

# 3. 🗺️ Structure du backlog MVP

Le backlog initial est organisé en Epics :

```text
EPIC 01 — 🏢 Administration
EPIC 02 — 📚 Référentiels
EPIC 03 — 🚌 Flotte
EPIC 04 — 👨‍✈️ Chauffeurs
EPIC 05 — 📅 Planification
EPIC 06 — 🛣️ Voyages
EPIC 07 — 🎫 Billetterie
EPIC 08 — 💺 Sièges
EPIC 09 — 💰 Finance / Caisses
EPIC 10 — 🛂 Embarquement
EPIC 11 — 📊 Dashboard
EPIC 12 — 🧾 Audit
```

Les fonctionnalités V2 ne doivent pas polluer le backlog du MVP.

---

# 4. 🏢 EPIC 01 — Administration

## US-001 — Créer une entreprise

**En tant qu'administrateur**,  
je veux créer une entreprise,  
afin de disposer de mon environnement de travail.

### Critères d'acceptation

- [ ] Le nom de l'entreprise est obligatoire.
- [ ] L'entreprise reçoit un identifiant unique.
- [ ] L'entreprise est créée dans son propre périmètre.
- [ ] Le créateur devient administrateur initial.

**Priorité : 🔴 Critique**

---

## US-002 — Créer une agence

**En tant qu'administrateur**,  
je veux créer une agence,  
afin d'organiser mon réseau d'exploitation.

### Critères

- [ ] L'agence appartient à une entreprise.
- [ ] Son nom est obligatoire.
- [ ] Elle possède un statut.
- [ ] Elle peut être activée ou désactivée.

**Priorité : 🔴 Critique**

---

## US-003 — Créer un utilisateur

**En tant qu'administrateur**,  
je veux créer un utilisateur,  
afin de permettre à un collaborateur d'utiliser le système.

### Critères

- [ ] L'utilisateur est associé à l'entreprise.
- [ ] Son rôle est défini.
- [ ] Son agence éventuelle est définie.
- [ ] Ses accès respectent son périmètre.

**Priorité : 🔴 Critique**

---

## US-004 — Gérer les rôles

**En tant qu'administrateur**,  
je veux attribuer des rôles,  
afin de contrôler les accès.

**Priorité : 🔴 Critique**

---

# 5. 📚 EPIC 02 — Référentiels

## US-010 — Créer une ville

**En tant qu'administrateur**,  
je veux enregistrer une ville,  
afin de l'utiliser dans les lignes et voyages.

---

## US-011 — Créer une ligne

**En tant que responsable exploitation**,  
je veux créer une ligne,  
afin de programmer des voyages.

### Critères

- [ ] Origine obligatoire.
- [ ] Destination obligatoire.
- [ ] Une ligne ne peut pas avoir la même origine et destination.
- [ ] Une ligne peut être activée ou désactivée.

**Priorité : 🔴 Critique**

---

## US-012 — Définir un tarif

**En tant que responsable autorisé**,  
je veux définir un tarif,  
afin que les voyages puissent être vendus.

**Priorité : 🔴 Critique**

---

# 6. 🚌 EPIC 03 — Flotte

## US-020 — Enregistrer un véhicule

**En tant que responsable flotte**,  
je veux enregistrer un véhicule,  
afin de l'utiliser dans la planification.

### Critères

- [ ] Identifiant unique.
- [ ] Immatriculation.
- [ ] Capacité.
- [ ] Statut.
- [ ] Kilométrage initial.

**Priorité : 🔴 Critique**

---

## US-021 — Modifier le statut d'un véhicule

Exemples :

```text
🟢 Disponible
🟡 Affecté
🔵 En voyage
🔴 Immobilisé
```

**Priorité : 🔴 Critique**

---

## US-022 — Consulter la disponibilité des véhicules

Le responsable doit pouvoir identifier rapidement les véhicules disponibles pour une période donnée.

**Priorité : 🔴 Critique**

---

# 7. 👨‍✈️ EPIC 04 — Chauffeurs

## US-030 — Enregistrer un chauffeur

**En tant que responsable exploitation**,  
je veux enregistrer un chauffeur,  
afin de pouvoir l'affecter à un voyage.

---

## US-031 — Gérer le statut d'un chauffeur

Exemples :

```text
🟢 Actif
🟡 Indisponible
🔴 Inactif
```

---

## US-032 — Vérifier la disponibilité d'un chauffeur

Le système doit détecter les conflits d'affectation.

**Priorité : 🔴 Critique**

---

# 8. 📅 EPIC 05 — Planification

## US-040 — Créer un voyage

**En tant que responsable exploitation**,  
je veux créer un voyage,  
afin de programmer un départ.

### Données minimales

- ligne ;
- date ;
- heure ;
- agence ;
- véhicule ;
- chauffeur ;
- tarif.

### Critères

- [ ] La ligne est active.
- [ ] Le véhicule est disponible.
- [ ] Le chauffeur est disponible.
- [ ] Les conflits sont contrôlés.
- [ ] Le voyage reçoit un statut.

**Priorité : 🔴 Critique**

---

## US-041 — Affecter un véhicule

**En tant que responsable exploitation**,  
je veux affecter un véhicule à un voyage.

### Critères

- [ ] Le véhicule est actif.
- [ ] Le véhicule est disponible.
- [ ] Sa capacité est connue.
- [ ] Aucun conflit n'existe.

---

## US-042 — Affecter un chauffeur

Même logique que l'affectation du véhicule.

---

## US-043 — Modifier un voyage

Les modifications doivent respecter les règles métier et les permissions.

**Priorité : 🟠 Haute**

---

## US-044 — Annuler un voyage

L'annulation doit être historisée.

**Priorité : 🟠 Haute**

---

# 9. 🛣️ EPIC 06 — Cycle de vie du voyage

## US-050 — Ouvrir les ventes

**En tant que responsable**,  
je veux ouvrir un voyage aux ventes.

---

## US-051 — Démarrer l'embarquement

Le voyage passe dans un statut permettant le contrôle des passagers.

---

## US-052 — Déclarer le départ

Le système enregistre :

- heure prévue ;
- heure réelle ;
- véhicule ;
- chauffeur ;
- informations utiles.

---

## US-053 — Déclarer l'arrivée

Le système enregistre :

- heure réelle ;
- kilométrage ;
- incidents éventuels.

---

## US-054 — Clôturer le voyage

Le voyage ne doit plus accepter certaines opérations incompatibles avec son état.

---

# 10. 🎫 EPIC 07 — Billetterie

## US-060 — Rechercher un voyage

**En tant qu'agent**,  
je veux rechercher un voyage par destination et date,  
afin de proposer les départs disponibles au passager.

### Critères

- [ ] Origine.
- [ ] Destination.
- [ ] Date.
- [ ] Voyages disponibles.
- [ ] Nombre de places disponibles.

**Priorité : 🔴 Critique**

---

## US-061 — Sélectionner un siège

**En tant qu'agent**,  
je veux sélectionner un siège disponible.

### Critères

- [ ] Les sièges occupés sont clairement identifiés.
- [ ] Un siège réservé n'est pas présenté comme libre.
- [ ] Une double sélection concurrente est empêchée.

**Priorité : 🔴 Critique**

---

## US-062 — Créer un passager

Le système doit permettre d'enregistrer les informations nécessaires à la billetterie.

---

## US-063 — Créer une réservation

**En tant qu'agent**,  
je veux réserver un siège temporairement.

### Critères

- [ ] Le siège est disponible.
- [ ] La réservation possède une échéance.
- [ ] Le siège n'est plus disponible pour une autre réservation concurrente.

---

## US-064 — Vendre un billet

**En tant qu'agent**,  
je veux vendre une place,  
afin d'enregistrer le voyage du passager.

### Critères

- [ ] Le voyage accepte les ventes.
- [ ] Le siège est disponible.
- [ ] Le passager est renseigné.
- [ ] Le tarif est calculé.
- [ ] Le paiement est enregistré.
- [ ] Le billet est généré.
- [ ] Le siège devient occupé.
- [ ] La recette est enregistrée.

**Priorité : 🔴 Critique**

---

## US-065 — Générer un billet

Le billet doit contenir au minimum :

- numéro ;
- passager ;
- voyage ;
- siège ;
- montant ;
- date ;
- statut.

---

## US-066 — Annuler un billet

L'annulation doit respecter les permissions et règles de remboursement.

**Priorité : 🟠 Haute**

---

## US-067 — Rembourser un billet

Le remboursement doit être lié à la transaction initiale.

**Priorité : 🟠 Haute**

---

# 11. 💺 EPIC 08 — Gestion des sièges

## US-070 — Configurer les sièges

Un véhicule doit pouvoir disposer d'une configuration de sièges.

---

## US-071 — Afficher la disponibilité

Exemple :

```text
🟢 Disponible
🟡 Réservé
🔵 Payé
⚪ Embarqué
🔴 Bloqué
```

---

## US-072 — Empêcher la double vente

Cette User Story est critique.

### Test

Deux utilisateurs tentent simultanément :

```text
Voyage V001
Siège 12
```

### Résultat attendu

```text
Utilisateur A → ✅
Utilisateur B → ❌
```

La protection doit être garantie au niveau de la base de données.

**Priorité : 🔴 Critique**

---

# 12. 💰 EPIC 09 — Finance / Caisses

## US-080 — Ouvrir une caisse

L'utilisateur autorisé ouvre une session de caisse avec un fonds initial.

---

## US-081 — Enregistrer un encaissement

Les ventes doivent alimenter automatiquement la caisse.

---

## US-082 — Enregistrer un remboursement

Le remboursement doit être rattaché à l'opération d'origine.

---

## US-083 — Consulter le solde théorique

Le système calcule le solde à partir des opérations.

---

## US-084 — Clôturer la caisse

L'utilisateur indique le montant réellement présent.

Le système calcule :

```text
Solde théorique
-
Solde réel
=
Écart
```

**Priorité : 🔴 Critique**

---

# 13. 🛂 EPIC 10 — Embarquement

## US-090 — Contrôler un billet

Le contrôleur peut rechercher ou scanner le billet.

---

## US-091 — Valider l'embarquement

### Critères

Le billet doit :

- appartenir au voyage ;
- être valide ;
- ne pas être annulé ;
- ne pas avoir déjà été embarqué.

**Priorité : 🔴 Critique**

---

# 14. 📊 EPIC 11 — Dashboard

## US-100 — Consulter l'activité du jour

Indicateurs :

- voyages ;
- billets ;
- recettes ;
- taux de remplissage.

---

## US-101 — Consulter l'activité d'une agence

Les résultats doivent respecter le périmètre de l'utilisateur.

---

## US-102 — Consulter les performances d'une période

Exemples :

- jour ;
- semaine ;
- mois.

**Priorité : 🟠 Haute**

---

# 15. 🧾 EPIC 12 — Audit

## US-110 — Journaliser une action sensible

Le système conserve :

```text
👤 Utilisateur
🕐 Date / heure
🎯 Action
📦 Objet
🔄 Ancienne valeur
➡️ Nouvelle valeur
```

---

## US-111 — Consulter l'historique

Les utilisateurs autorisés peuvent consulter les événements correspondant à leur périmètre.

**Priorité : 🟠 Haute**

---

# 16. 🟢 Priorisation du backlog

## 🔴 P0 — Bloquant / essentiel

À réaliser avant toute démonstration métier sérieuse :

- authentification ;
- entreprises ;
- agences ;
- utilisateurs ;
- rôles ;
- lignes ;
- véhicules ;
- chauffeurs ;
- voyages ;
- sièges ;
- vente billet ;
- paiement ;
- caisse ;
- embarquement.

---

## 🟠 P1 — Important

- annulation ;
- remboursement ;
- dashboard ;
- audit avancé ;
- documents véhicules ;
- maintenance simple ;
- carburant simple.

---

## 🔵 P2 — Après MVP

- colis avancé ;
- Mobile Money ;
- notifications ;
- portail passager ;
- application chauffeur ;
- GPS ;
- analytics avancés.

---

# 17. 🧪 Sprint 0

Le Sprint 0 n'a pas pour objectif de produire beaucoup de fonctionnalités.

Son objectif est de préparer le terrain.

## Durée proposée

> **1 semaine**

---

# 18. 🎯 Objectifs du Sprint 0

À la fin du Sprint 0, l'équipe doit avoir :

```text
☑ Repository GitHub
☑ Structure projet
☑ Environnement Python
☑ Dépendances
☑ Supabase configuré
☑ Base initiale
☑ Authentification de base
☑ CI minimale si retenue
☑ Conventions Git
☑ README
☑ `.env.example`
☑ Première application Flet
☑ Architecture validée
```

---

# 19. 👥 Répartition Sprint 0

## 👤 Lead

- architecture ;
- conventions ;
- structure projet ;
- Router ;
- AppContainer ;
- README ;
- coordination.

## 👨‍💻 Développeur 2

- composants UI ;
- thème ;
- layout ;
- responsive ;
- écran connexion.

## 👨‍💻 Développeur 3

- schéma DB initial ;
- migrations ;
- tables core ;
- contraintes.

## 👨‍💻 Développeur 4

- Supabase Auth ;
- profils ;
- rôles ;
- première politique RLS.

---

# 20. 🏁 Definition of Done du Sprint 0

Le Sprint 0 est terminé lorsque :

```text
👤 Un utilisateur peut
      ↓
🔐 se connecter
      ↓
🏢 être identifié à une entreprise
      ↓
🏪 être associé à une agence
      ↓
🎭 recevoir un rôle
      ↓
🎨 accéder à une interface protégée
```

Et surtout :

> **Un deuxième utilisateur d'une autre entreprise ne doit pas pouvoir voir les données du premier périmètre.**

---

# 21. 🗓️ Sprint 1

## Objectif

Construire les premières données métier.

### Fonctionnalités

- entreprises ;
- agences ;
- villes ;
- lignes ;
- véhicules ;
- chauffeurs.

### Résultat attendu

```text
🏢 Entreprise
   ↓
🏪 Agence
   ↓
📚 Ligne
   ↓
🚌 Véhicule
   ↓
👨‍✈️ Chauffeur
```

---

# 22. 👥 Répartition Sprint 1

### Lead

- entreprise/agence ;
- intégration ;
- règles communes.

### Développeur 2

- UI administration ;
- villes ;
- lignes.

### Développeur 3

- flotte ;
- véhicules ;
- chauffeurs.

### Développeur 4

- auth ;
- permissions ;
- RLS ;
- tests d'isolation.

---

# 23. 🗓️ Sprint 2

## Objectif

Créer le premier vrai workflow opérationnel.

```text
📚 Ligne
   ↓
📅 Voyage
   ↓
🚌 Véhicule
   ↓
👨‍✈️ Chauffeur
   ↓
✅ Voyage planifié
```

### Fonctionnalités

- création voyage ;
- affectation véhicule ;
- affectation chauffeur ;
- contrôle des conflits ;
- calendrier ;
- statuts.

---

# 24. 🗓️ Sprint 3

## Objectif

Commencer la billetterie.

```text
🛣️ Voyage
   ↓
💺 Sièges
   ↓
👤 Passager
   ↓
🎫 Réservation
```

### Fonctionnalités

- recherche voyage ;
- plan de sièges ;
- passager ;
- réservation.

---

# 25. 🗓️ Sprint 4

## Objectif

Finaliser le premier cycle de vente.

```text
🎫 Réservation
   ↓
💰 Paiement
   ↓
🎫 Billet
   ↓
🏦 Caisse
```

### Fonctionnalités

- vente ;
- paiement ;
- billet ;
- caisse ;
- double vente ;
- tests de concurrence.

---

# 26. 🗓️ Sprint 5

## Objectif

Faire fonctionner le voyage de bout en bout.

```text
🎫 Billets
   ↓
🛂 Embarquement
   ↓
🚦 Départ
   ↓
🏁 Arrivée
   ↓
✅ Clôture
```

---

# 27. 🗓️ Sprint 6

## Objectif

Pilotage et stabilisation.

- dashboard ;
- audit ;
- rapports ;
- corrections ;
- tests ;
- optimisation UX.

---

# 28. 🧪 Tests prioritaires

Les tests suivants doivent être considérés comme critiques.

### 🔐 Sécurité

- isolation entreprise ;
- isolation agence ;
- permissions.

### 🎫 Billetterie

- double vente ;
- réservation expirée ;
- billet annulé ;
- voyage fermé.

### 💰 Finance

- paiement ;
- remboursement ;
- caisse ;
- écart.

### 🛣️ Voyage

- conflit véhicule ;
- conflit chauffeur ;
- transition de statut.

---

# 29. 🚨 Dépendances importantes

Certaines fonctionnalités ne doivent pas être développées trop tôt.

```text
Voyage
   ↓
Billetterie
   ↓
Paiement
   ↓
Caisse
```

Donc :

> ❌ Ne pas commencer la caisse complexe avant d'avoir stabilisé la vente.

De même :

```text
Véhicule
   ↓
Configuration sièges
   ↓
Voyage
   ↓
Billetterie
```

---

# 30. 🧠 Règle d'or du backlog

Une User Story doit être :

- suffisamment petite ;
- testable ;
- compréhensible ;
- rattachée à une règle métier ;
- rattachée à un domaine ;
- priorisée.

Éviter :

> ❌ « Développer la billetterie »

Préférer :

> ✅ « Afficher les sièges disponibles d'un voyage »

---

# 31. 🏷️ Estimation des tâches

L'équipe peut utiliser une estimation simple :

```text
XS = quelques heures
S  = environ 1 jour
M  = 2-3 jours
L  = 4-5 jours
XL = trop gros → découper
```

Une tâche estimée XL doit être découpée.

---

# 32. 📊 Mesure de capacité

Au début, ne cherchez pas à atteindre une vitesse théorique.

Pendant les deux ou trois premiers sprints, mesurez simplement :

```text
Travail prévu
      VS
Travail réellement terminé
```

Après quelques sprints, l'équipe pourra estimer sa capacité réelle.

---

# 33. 🧭 Préparation de la première réunion officielle

La réunion avec les développeurs devrait suivre cet ordre :

### 1️⃣ Vision

Pourquoi construisons-nous ce produit ?

### 2️⃣ Problème

Quels problèmes des entreprises de transport voulons-nous résoudre ?

### 3️⃣ Utilisateurs

Qui utilisera le système ?

### 4️⃣ Workflow

Comment fonctionne une opération réelle ?

### 5️⃣ MVP

Qu'est-ce qui entre dans la première version ?

### 6️⃣ Architecture

Comment le produit sera organisé ?

### 7️⃣ Organisation

Comment allons-nous travailler à quatre ?

### 8️⃣ Backlog

Que faisons-nous en premier ?

### 9️⃣ Discussion

Questions, objections, propositions.

### 🔟 Décisions

Ce qui est validé devient notre référence.

---

# 34. 📝 Règle importante pour la réunion

Les développeurs doivent pouvoir challenger le projet.

Ils doivent être encouragés à poser des questions comme :

> « Pourquoi cette règle ? »

> « Que se passe-t-il dans ce cas ? »

> « Est-ce réellement nécessaire au MVP ? »

> « Cette architecture risque-t-elle de nous bloquer ? »

> « Comment un agent travaille-t-il réellement ? »

> « Quelle est la priorité commerciale ? »

Le but de la réunion n'est pas de leur présenter un cahier des charges figé.

Le but est de construire **une compréhension commune du produit**.

---

# 35. 🎯 Résultat attendu après la réunion

À la fin de la réunion, l'équipe doit disposer de :

```text
☑ Vision commune
☑ Périmètre MVP
☑ Architecture validée
☑ Rôles de chacun
☑ Backlog initial
☑ Sprint 0
☑ Règles de collaboration
☑ Prochain rendez-vous
```

---

# 36. 🏁 Conclusion

Nous disposons maintenant d'un premier backlog structuré.

La séquence de travail devient :

```text
📚 Documentation
      ↓
📋 Backlog
      ↓
🎯 Sprint 0
      ↓
🧱 Fondations
      ↓
🚍 Premier workflow
      ↓
🎫 Billetterie
      ↓
💰 Finance
      ↓
🧪 Tests
      ↓
👤 Pilote
```

Le projet entre progressivement dans une logique de développement réel.

---

## 📌 Prochaine étape

**Document 11 — Modèle de données / Entités métier**

Nous allons commencer à identifier les objets fondamentaux du système :

- 🏢 Company ;
- 🏪 Agency ;
- 👤 User / Profile ;
- 🚌 Vehicle ;
- 👨‍✈️ Driver ;
- 📚 Route ;
- 🛣️ Trip ;
- 💺 Seat ;
- 👤 Passenger ;
- 🟡 Reservation ;
- 🎫 Ticket ;
- 💰 Payment ;
- 🏦 Cash Session ;
- 🔧 Maintenance ;
- ⛽ Fuel ;
- 🧾 Audit Log.

L'objectif ne sera pas encore d'écrire toutes les tables SQL, mais de comprendre **les entités, leurs relations et leur responsabilité** avant de passer au schéma de base de données.
