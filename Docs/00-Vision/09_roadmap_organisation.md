# 🚀 ERP TRANSPORT
## Document 09 — Roadmap & organisation du développement

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
- `08_architecture_technique.md`

---

# 1. 🎯 Objectif du document

Ce document transforme la vision, les workflows et l'architecture en **plan concret de développement** pour l'équipe.

Il définit :

- 🗓️ les grandes phases ;
- 🏁 les jalons ;
- 👥 l'organisation des 4 développeurs ;
- 📦 les lots fonctionnels ;
- 🧪 la stratégie de validation ;
- 🌿 l'organisation Git ;
- 📋 le backlog initial ;
- 🎯 les objectifs des premières semaines ;
- 🚀 la préparation du premier pilote.

> 💡 L'objectif n'est pas de prédire parfaitement les dates. L'objectif est de donner à l'équipe une direction claire, des priorités et des points de contrôle.

---

# 2. 🧭 Stratégie générale

Le projet doit avancer par étapes :

```text
🎯 Cadrage
   ↓
🏗️ Architecture
   ↓
🧪 Prototype
   ↓
🧱 Fondations
   ↓
🚍 MVP métier
   ↓
🧪 Tests
   ↓
👤 Pilote terrain
   ↓
🔄 Corrections
   ↓
🚀 Première version commerciale
```

Le développement ne doit pas fonctionner selon le modèle :

```text
6 mois de développement
        ↓
🚀 Livraison
        ↓
😱 Découverte des problèmes
```

Mais plutôt :

```text
Petit lot
   ↓
Test
   ↓
Validation
   ↓
Petit lot suivant
```

---

# 3. 🗓️ Vue globale de la roadmap

Pour une équipe de 4 développeurs travaillant régulièrement sur le projet, une première trajectoire indicative pourrait être :

| Phase | Durée indicative | Résultat |
|---|---:|---|
| 🎯 Cadrage | 1-2 semaines | Vision validée |
| 🏗️ Architecture | 1-2 semaines | Architecture validée |
| 🧪 Prototype | 1-2 semaines | Premiers workflows validés |
| 🧱 Fondations | 2-3 semaines | Auth, DB, structure projet |
| 🚍 Exploitation | 4-6 semaines | Flotte, chauffeurs, planning, voyages |
| 🎫 Billetterie | 4-5 semaines | Réservations, sièges, ventes |
| 💰 Finance | 2-3 semaines | Caisses, paiements, recettes |
| 📊 Reporting | 1-2 semaines | Dashboard MVP |
| 🧪 Stabilisation | 3-4 semaines | Tests, corrections |
| 👤 Pilote | 2-4 semaines | Utilisation terrain |
| 🚀 V1 | après pilote | Version commercialisable |

### Estimation

> **Environ 4 à 6 mois pour un MVP sérieux**, avec une marge nécessaire selon la disponibilité réelle de l'équipe et les décisions métier.

---

# 4. 🎯 Phase 0 — Cadrage

## Objectif

Obtenir une compréhension commune du produit avant de coder.

## Livrables

- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`
- `06_definition_mvp.md`

## Résultat attendu

Chaque développeur doit pouvoir répondre à :

> Qu'est-ce que nous construisons ?

> Pour qui ?

> Quel problème résolvons-nous ?

> Qu'est-ce qui entre dans le MVP ?

---

# 5. 🏗️ Phase 1 — Architecture

## Objectif

Valider les choix techniques avant de multiplier le code.

## Livrables

- `07_architecture_fonctionnelle.md`
- `08_architecture_technique.md`

## Décisions à prendre

### Technologie

- Python ;
- Flet ;
- Supabase ;
- PostgreSQL.

### Architecture

- structure `src/` ;
- Domain ;
- Services ;
- Repositories ;
- UI ;
- Router ;
- AppContainer.

### Sécurité

- Auth ;
- rôles ;
- permissions ;
- RLS ;
- isolation multi-tenant.

---

# 6. 🧪 Phase 2 — Prototype

Avant de construire tout le système, réaliser quelques workflows représentatifs.

## Prototype 1

```text
Créer voyage
   ↓
Affecter véhicule
   ↓
Affecter chauffeur
```

## Prototype 2

```text
Rechercher voyage
   ↓
Choisir siège
   ↓
Vendre billet
```

## Prototype 3

```text
Vente
   ↓
Paiement
   ↓
Caisse
```

## Objectif

Valider :

- ergonomie ;
- architecture ;
- communication entre couches ;
- performance ;
- sécurité ;
- faisabilité technique.

---

# 7. 🧱 Phase 3 — Fondations techniques

## Lot 1 — Projet

- repository GitHub ;
- structure du projet ;
- environnement Python ;
- dépendances ;
- configuration ;
- logging ;
- gestion des erreurs.

## Lot 2 — Base de données

- projet Supabase ;
- PostgreSQL ;
- migrations ;
- contraintes ;
- index ;
- RLS.

## Lot 3 — Authentification

- inscription ;
- connexion ;
- déconnexion ;
- session ;
- profil ;
- entreprise ;
- agence ;
- rôle.

## Lot 4 — UI

- thème ;
- layout ;
- navigation ;
- responsive ;
- composants réutilisables.

---

# 8. 🏢 Phase 4 — Administration

## Fonctionnalités

- entreprise ;
- agences ;
- utilisateurs ;
- rôles ;
- permissions ;
- paramètres.

## Critère de validation

Un administrateur doit pouvoir :

```text
Créer entreprise
      ↓
Créer agence
      ↓
Créer utilisateur
      ↓
Attribuer rôle
      ↓
Connexion
      ↓
Accès limité au périmètre
```

---

# 9. 🚌 Phase 5 — Flotte & chauffeurs

## Flotte

- véhicules ;
- capacité ;
- statut ;
- kilométrage ;
- documents.

## Chauffeurs

- profil ;
- permis ;
- statut ;
- disponibilité.

## Tests critiques

- véhicule indisponible ;
- chauffeur indisponible ;
- conflit d'affectation ;
- capacité.

---

# 10. 📅 Phase 6 — Planification

## Fonctionnalités

- lignes ;
- horaires ;
- calendrier ;
- voyages ;
- affectations.

## Workflow de validation

```text
Créer ligne
   ↓
Créer voyage
   ↓
Choisir véhicule
   ↓
Choisir chauffeur
   ↓
Contrôles
   ↓
Valider
```

Ce workflow doit être stable avant de commencer la billetterie complète.

---

# 11. 🎫 Phase 7 — Billetterie

C'est probablement le lot le plus sensible du MVP.

## Fonctionnalités

- recherche ;
- voyage ;
- sièges ;
- passagers ;
- réservation ;
- vente ;
- paiement ;
- billet ;
- annulation ;
- remboursement.

## Tests critiques

### Test 1

Deux agents vendent le même siège simultanément.

Résultat :

> Une seule transaction réussit.

### Test 2

Vente sur voyage fermé.

Résultat :

> Refus.

### Test 3

Annulation d'un billet payé.

Résultat :

> Billet annulé + opération financière + historique.

---

# 12. 💰 Phase 8 — Finance

## Fonctionnalités

- caisse ;
- paiement ;
- recettes ;
- remboursements ;
- clôture ;
- écarts.

## Workflow

```text
🎫 Vente
   ↓
💰 Paiement
   ↓
🏦 Caisse
   ↓
🔒 Clôture
   ↓
📊 Rapport
```

---

# 13. 📊 Phase 9 — Dashboard

Le dashboard doit être construit **après que les données métier soient fiables**.

## Pourquoi ?

Parce qu'un beau dashboard avec des données incorrectes est plus dangereux qu'une absence de dashboard.

## Indicateurs MVP

- voyages ;
- billets ;
- recettes ;
- taux de remplissage ;
- activité agences ;
- véhicules.

---

# 14. 🧪 Phase 10 — Stabilisation

Cette phase est obligatoire.

## Tests

### Unitaires

- règles métier ;
- services ;
- validations.

### Intégration

- services + repositories ;
- DB ;
- RLS ;
- transactions.

### End-to-end

- planification ;
- vente ;
- embarquement ;
- départ ;
- arrivée ;
- caisse.

---

# 15. 👤 Phase 11 — Pilote terrain

Le produit doit être testé dans une vraie entreprise ou au minimum dans un environnement reproduisant fidèlement les opérations réelles.

## Objectif

Observer :

- comment les agents travaillent ;
- quelles informations ils utilisent ;
- où ils ralentissent ;
- quelles erreurs apparaissent ;
- quelles fonctionnalités manquent ;
- quelles fonctionnalités sont inutiles.

## Principe

> **Le terrain est la source finale de validation du workflow.**

---

# 16. 🚀 Phase 12 — Première version commerciale

Après le pilote :

- corriger les problèmes ;
- renforcer la sécurité ;
- améliorer les performances ;
- stabiliser les workflows ;
- documenter ;
- préparer le support ;
- préparer la tarification.

Le produit peut alors commencer à être proposé à plusieurs entreprises.

---

# 17. 👥 Organisation de l'équipe

Une équipe de quatre personnes peut fonctionner avec une organisation par domaines.

## 👤 Lead / Product / Architecture

Responsabilités :

- vision produit ;
- arbitrages ;
- architecture ;
- coordination ;
- revue de code ;
- domaine Voyage ;
- intégration.

Le lead n'a pas vocation à coder toutes les fonctionnalités.

Son rôle est de maintenir la cohérence globale.

---

## 👨‍💻 Développeur 2 — Commercial

Responsabilités :

- billetterie ;
- réservation ;
- sièges ;
- passagers ;
- parcours agent.

---

## 👨‍💻 Développeur 3 — Exploitation

Responsabilités :

- flotte ;
- chauffeurs ;
- maintenance ;
- planification ;
- affectations.

---

## 👨‍💻 Développeur 4 — Finance / Platform

Responsabilités :

- finance ;
- caisse ;
- authentification ;
- sécurité ;
- reporting ;
- infrastructure selon les besoins.

---

# 18. 🤝 Attention : les domaines ne sont pas des silos

La répartition permet de travailler en parallèle.

Mais :

```text
Billetterie
    ↕
Voyages
    ↕
Finance
```

sont fortement liés.

Il faudra donc organiser régulièrement des points d'intégration.

---

# 19. 🗓️ Rythme de travail recommandé

Pour une petite équipe :

## Daily court

15 minutes maximum.

Chaque membre répond :

1. Que ai-je terminé ?
2. Que vais-je faire ?
3. Qu'est-ce qui me bloque ?

---

## Réunion technique

1 à 2 fois par semaine.

Sujets :

- architecture ;
- décisions ;
- problèmes ;
- dépendances.

---

## Revue produit

Chaque semaine ou toutes les deux semaines.

Démonstration :

```text
Fonction développée
      ↓
Démonstration
      ↓
Retour
      ↓
Validation
```

---

# 20. 📋 Backlog initial

Le backlog doit être découpé en petits éléments.

Exemple :

```text
EPIC : Billetterie

TICKET-001
Créer un voyage

TICKET-002
Afficher les voyages disponibles

TICKET-003
Afficher le plan de sièges

TICKET-004
Sélectionner un siège

TICKET-005
Créer un passager

TICKET-006
Créer une réservation

TICKET-007
Confirmer paiement

TICKET-008
Générer billet

TICKET-009
Annuler billet

TICKET-010
Rembourser billet
```

Chaque ticket doit être suffisamment petit pour être compris et testé.

---

# 21. 🧩 Format recommandé d'une tâche

Chaque tâche devrait contenir :

```text
🎯 Objectif

En tant que [utilisateur]
Je veux [action]
Afin de [résultat]
```

Puis :

### Conditions d'acceptation

```text
☑ ...
☑ ...
☑ ...
```

### Règles métier concernées

```text
RM-072
RM-074
RM-076
```

### Tests

```text
☑ Cas nominal
☑ Cas erreur
☑ Cas limite
```

---

# 22. 🌿 Organisation Git

## Branches

```text
main
develop
feature/*
fix/*
```

Une stratégie plus simple peut également être utilisée :

```text
main
   ↑
feature/*
```

Pour une petite équipe, il vaut mieux une stratégie simple et disciplinée qu'une stratégie Git extrêmement complexe.

---

# 23. 🔀 Pull Requests

Aucune fonctionnalité importante ne devrait être fusionnée sans revue.

Workflow :

```text
Développeur
     ↓
Feature branch
     ↓
Commit
     ↓
Pull Request
     ↓
Code Review
     ↓
Tests
     ↓
Merge
```

---

# 24. 🧾 Convention de commit

Les commits doivent expliquer clairement ce qui a changé.

Exemples :

```text
feat: add trip creation
feat: add seat availability
fix: prevent duplicate ticket sale
refactor: extract ticket service
test: add trip validation tests
docs: update ticket workflow
```

Cela facilitera l'historique du projet.

---

# 25. 🚨 Règles d'équipe

Quelques règles doivent être décidées dès le départ.

### Règle 1

❌ Pas de code directement sur `main`.

### Règle 2

❌ Pas de secrets dans Git.

### Règle 3

❌ Pas de SQL dispersé dans les Views.

### Règle 4

❌ Pas de règle métier cachée dans un bouton.

### Règle 5

✅ Toute fonctionnalité importante doit avoir des tests.

### Règle 6

✅ Toute décision architecturale importante doit être documentée.

### Règle 7

✅ Une tâche terminée doit respecter la Definition of Done.

---

# 26. 🧠 ADR — Architecture Decision Records

Les décisions importantes doivent être conservées.

Exemple :

```text
docs/decisions/

ADR-001-monolithe-modulaire.md
ADR-002-supabase.md
ADR-003-multi-tenant.md
ADR-004-rls.md
ADR-005-authentication.md
```

Format :

```text
# ADR-001

## Décision
Nous utilisons un monolithe modulaire.

## Contexte
Équipe de quatre développeurs.

## Raisons
- simplicité ;
- vitesse ;
- maintenance ;
- coût.

## Conséquences
...
```

---

# 27. 🧪 Definition of Done de l'équipe

Une tâche est terminée lorsque :

```text
☑ Fonction développée
☑ Règles métier respectées
☑ Tests écrits
☑ Tests passants
☑ Permissions vérifiées
☑ Erreurs gérées
☑ Code review réalisée
☑ Documentation mise à jour
☑ Intégrée sans régression
```

---

# 28. 🎯 Objectif des 2 premières semaines

Il ne faut pas chercher à développer beaucoup de fonctionnalités.

## Semaine 1

```text
📚 Finaliser cadrage
       ↓
🏗️ Valider architecture
       ↓
📁 Créer repository
       ↓
⚙️ Initialiser projet
       ↓
🗄️ Créer environnement Supabase
       ↓
🔐 Authentification de base
```

## Semaine 2

```text
🏢 Entreprise
   ↓
🏪 Agence
   ↓
👥 Utilisateur
   ↓
🎭 Rôle
   ↓
🚌 Véhicule
   ↓
👨‍✈️ Chauffeur
```

À la fin de ces deux semaines, l'équipe doit avoir une **base technique solide**, pas 50 écrans.

---

# 29. 🎯 Premier jalon fonctionnel

Le premier vrai jalon pourrait être :

> **« Un responsable peut créer un voyage complet. »**

Workflow :

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
   ↓
📅 Voyage
   ↓
✅ Voyage planifié
```

---

# 30. 🎯 Deuxième jalon fonctionnel

> **« Un agent peut vendre une place. »**

Workflow :

```text
🎫 Recherche
   ↓
🛣️ Voyage
   ↓
💺 Siège
   ↓
👤 Passager
   ↓
💰 Paiement
   ↓
🎫 Billet
```

---

# 31. 🎯 Troisième jalon fonctionnel

> **« L'entreprise peut clôturer et analyser une journée d'activité. »**

Workflow :

```text
🎫 Ventes
   +
💰 Caisses
   +
🛣️ Voyages
   ↓
📊 Dashboard
```

Ces trois jalons permettent déjà de démontrer une grande partie de la valeur du produit.

---

# 32. 🧪 Stratégie de démonstration

Toutes les deux semaines, l'équipe devrait être capable de montrer quelque chose de réellement fonctionnel.

Exemple :

### Démo 1

> Créer une entreprise et ses utilisateurs.

### Démo 2

> Créer un voyage.

### Démo 3

> Vendre un billet.

### Démo 4

> Embarquer et clôturer un voyage.

### Démo 5

> Voir les recettes dans le dashboard.

---

# 33. 📈 Indicateurs de progression du projet

Ne pas mesurer uniquement :

> « nombre de lignes de code ».

Mesurer plutôt :

- nombre de workflows fonctionnels ;
- nombre de règles métier couvertes ;
- taux de tests ;
- bugs critiques ;
- temps moyen de résolution ;
- fonctionnalités validées par utilisateur ;
- stabilité du MVP.

---

# 34. 🚨 Gestion du changement

Le projet évoluera.

Une nouvelle demande doit passer par :

```text
💡 Nouvelle idée
      ↓
🎯 Valeur métier
      ↓
📊 Impact
      ↓
⏱️ Effort
      ↓
🚨 Risque
      ↓
📅 MVP / V2 / V3
```

Une demande ne doit pas automatiquement entrer dans le sprint en cours.

---

# 35. 👤 Préparation du premier pilote

Avant le pilote :

### Données

- agences ;
- véhicules ;
- chauffeurs ;
- lignes ;
- tarifs.

### Utilisateurs

- direction ;
- exploitation ;
- agents.

### Formation

Préparer des procédures simples :

```text
📘 Créer un voyage
📘 Vendre un billet
📘 Annuler
📘 Clôturer caisse
📘 Consulter dashboard
```

### Support

Prévoir un canal pour remonter :

- bugs ;
- questions ;
- suggestions.

---

# 36. 🧠 Le rôle du porteur de projet / Lead

Le porteur du projet doit être le garant de la cohérence.

Il ne doit pas être :

> « celui qui donne des ordres aux développeurs ».

Il doit être :

> **celui qui transforme le besoin métier en décisions compréhensibles et aide l'équipe à arbitrer.**

Ses responsabilités :

- vision ;
- priorités ;
- arbitrages ;
- validation métier ;
- coordination ;
- qualité fonctionnelle ;
- cohérence architecture ;
- relation avec les utilisateurs pilotes.

---

# 37. 🏁 Critère de réussite du projet

Le succès ne sera pas :

> « Nous avons développé l'ERP. »

Le vrai succès sera :

> **Une entreprise de transport utilise le produit quotidiennement parce qu'il lui fait gagner du temps, réduit les erreurs et lui donne une meilleure visibilité sur son activité.**

Puis :

```text
👤 1er pilote
   ↓
🏢 2e entreprise
   ↓
🏢 5 entreprises
   ↓
🏢 20 entreprises
   ↓
📈 SaaS rentable
```

---

# 38. 🎯 Conclusion

Nous disposons maintenant d'une trajectoire complète :

```text
01 🎯 Vision
      ↓
02 👥 Utilisateurs
      ↓
03 🧩 Modules
      ↓
04 🔄 Workflows
      ↓
05 📐 Règles métier
      ↓
06 🚀 MVP
      ↓
07 🏗️ Architecture fonctionnelle
      ↓
08 💻 Architecture technique
      ↓
09 🗓️ Roadmap
```

La documentation permet maintenant de passer de la vision au développement.

La prochaine étape n'est plus de produire une longue liste de fonctionnalités.

Il faut maintenant transformer cette documentation en **backlog réel**, puis préparer la première réunion officielle avec les développeurs.

---

## 📌 Étape suivante recommandée

**Document 10 — Backlog initial & Sprint 0**

Ce document pourra contenir :

- 📋 les premières User Stories ;
- 🎫 les Epics ;
- 🧪 les critères d'acceptation ;
- 🏷️ les priorités ;
- 👥 l'affectation aux développeurs ;
- 🗓️ le Sprint 0 ;
- 🗓️ le Sprint 1 ;
- 🎯 les livrables de la première réunion technique.

L'objectif sera d'arriver à la réunion avec quelque chose de très concret :

> **« Voici ce que nous allons construire, voici pourquoi, voici comment nous allons travailler et voici ce que chacun peut commencer à prendre en charge. »**
