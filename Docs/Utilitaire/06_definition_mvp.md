# 🚀 ERP TRANSPORT
## Document 06 — Définition du MVP

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`

---

# 1. 🎯 Objectif du MVP

Le MVP (**Minimum Viable Product**) est la première version du produit capable de résoudre un problème réel pour une entreprise de transport.

Le MVP ne doit pas être considéré comme une version « incomplète » de l'ERP.

Il doit être considéré comme :

> **La plus petite version du produit capable d'être utilisée en conditions réelles, de produire une valeur mesurable et de permettre de recueillir les premiers retours clients.**

---

# 2. 🧠 Principe directeur

La vision du produit est large :

```text
🏢 ERP TRANSPORT
      │
      ├── Agences
      ├── Flotte
      ├── Voyages
      ├── Billetterie
      ├── Finance
      ├── Maintenance
      ├── Carburant
      ├── Colis
      ├── GPS
      ├── Mobile
      └── Marketplace
```

Mais le MVP doit être beaucoup plus petit.

Notre objectif initial est de couvrir correctement le cycle :

```text
📅 PLANIFIER
     ↓
🛣️ CRÉER UN VOYAGE
     ↓
🚌 AFFECTER UN VÉHICULE
     ↓
👨‍✈️ AFFECTER UN CHAUFFEUR
     ↓
🎫 VENDRE DES BILLETS
     ↓
🛂 EMBARQUER
     ↓
🚦 FAIRE PARTIR LE VOYAGE
     ↓
🏁 TERMINER LE VOYAGE
     ↓
💰 CONTRÔLER LES RECETTES
     ↓
📊 MESURER LA PERFORMANCE
```

Si ce cycle fonctionne correctement, nous avons déjà un produit exploitable.

---

# 3. 🟢 Périmètre fonctionnel du MVP

## 3.1 🏢 Administration

### Inclus

- création de l'entreprise ;
- utilisateurs ;
- rôles ;
- permissions de base ;
- paramètres généraux.

### Objectif

Permettre à une entreprise de créer son environnement et de gérer ses utilisateurs.

---

# 4. 🏪 Gestion des agences

## Inclus

- création d'agence ;
- modification ;
- activation / désactivation ;
- affectation des utilisateurs ;
- informations de contact ;
- consultation de l'activité.

## MVP

Une entreprise doit pouvoir gérer plusieurs agences.

```text
Entreprise
   ├── Yaoundé
   ├── Douala
   └── Bafoussam
```

---

# 5. 📚 Référentiels

## Inclus

### Géographie

- villes ;
- destinations.

### Transport

- lignes ;
- trajets ;
- horaires.

### Commercial

- tarifs.

### Paramètres

- statuts ;
- types d'opérations nécessaires au MVP.

## Hors MVP

- gestion géographique avancée ;
- cartographie ;
- calcul automatique des itinéraires ;
- optimisation des trajets.

---

# 6. 🚌 Gestion de la flotte

## Inclus

### Véhicules

- création ;
- modification ;
- statut ;
- immatriculation ;
- marque ;
- modèle ;
- capacité ;
- kilométrage.

### Disponibilité

- disponible ;
- affecté ;
- en voyage ;
- immobilisé.

### Documents

Une première version pourra permettre d'enregistrer :

- type de document ;
- numéro ;
- date d'émission ;
- date d'expiration.

## Hors MVP initial

- GPS temps réel ;
- télématique ;
- géolocalisation avancée ;
- analyse prédictive ;
- suivi automatique du véhicule.

---

# 7. 👨‍✈️ Gestion des chauffeurs

## Inclus

- profil ;
- coordonnées ;
- permis ;
- catégorie ;
- statut ;
- historique des affectations ;
- disponibilité.

## Règles importantes

Le système doit empêcher :

- affectation d'un chauffeur inactif ;
- double affectation incompatible ;
- affectation non conforme aux règles définies.

---

# 8. 📅 Planification

## Inclus

- calendrier ;
- création de départs ;
- horaires ;
- affectation véhicule ;
- affectation chauffeur ;
- contrôle des conflits ;
- modification ;
- annulation.

## Exemple

```text
📅 15 août 2026 — 07h30

Yaoundé → Douala

🚌 BUS-023
👨‍✈️ Jean
🎫 35 places
```

---

# 9. 🛣️ Gestion des voyages

## Inclus

- création ;
- planification ;
- ouverture des ventes ;
- embarquement ;
- départ ;
- arrivée ;
- clôture ;
- annulation.

## Statuts MVP proposés

```text
📝 Brouillon
📅 Planifié
🎫 Ouvert aux ventes
🛂 Embarquement
🚦 En cours
🏁 Arrivé
✅ Clôturé
❌ Annulé
```

Les transitions seront contrôlées par les règles métier.

---

# 10. 🎫 Billetterie

La billetterie est l'un des modules centraux du MVP.

## Inclus

### Recherche

- origine ;
- destination ;
- date ;
- horaire.

### Vente

- sélection voyage ;
- sélection siège ;
- passager ;
- tarif ;
- paiement ;
- billet.

### Réservation

- réservation temporaire ;
- expiration ;
- confirmation par paiement.

### Après-vente

- annulation ;
- remboursement selon règles ;
- changement de siège ;
- historique.

---

# 11. 💺 Gestion des sièges

## Inclus

- capacité du véhicule ;
- plan de sièges ;
- disponibilité ;
- réservation ;
- vente ;
- embarquement.

## Principe critique

Le système doit empêcher une double vente.

```text
Siège 12

Agent A → vente → ✅
Agent B → vente → ❌
```

Le contrôle doit fonctionner même lorsque plusieurs agents travaillent simultanément.

---

# 12. 💰 Finance / caisses

## Inclus

### Caisse

- ouverture ;
- opérations ;
- encaissements ;
- remboursements ;
- clôture ;
- solde théorique ;
- solde réel ;
- écarts.

### Recettes

- billets ;
- autres recettes simples si nécessaire.

### Rapports

- recettes par agence ;
- recettes par voyage ;
- recettes par période.

## Hors MVP

- comptabilité générale ;
- bilan ;
- compte de résultat complet ;
- fiscalité automatisée ;
- rapprochement bancaire avancé.

---

# 13. 📊 Dashboard MVP

Le tableau de bord doit rester simple.

## Direction

Exemples d'indicateurs :

```text
🛣️ Voyages aujourd'hui       24
🎫 Billets vendus            684
💰 Recettes                   3,42 M FCFA
📈 Taux de remplissage        81 %
🚌 Véhicules actifs           42
⚠️ Alertes                     7
```

## Agence

- ventes ;
- recettes ;
- départs ;
- occupation ;
- caisse.

---

# 14. 🧾 Audit MVP

Le MVP doit déjà conserver les actions sensibles.

## À journaliser

- connexion ;
- création/modification voyage ;
- annulation billet ;
- remboursement ;
- modification tarif ;
- changement véhicule ;
- changement chauffeur ;
- clôture caisse.

L'interface d'administration avancée pourra venir plus tard.

---

# 15. 🟡 Fonctionnalités importantes mais secondaires

Ces fonctionnalités ont de la valeur mais ne doivent pas bloquer la première version.

## 🔧 Maintenance

MVP simplifié possible :

- historique ;
- interventions ;
- coût ;
- prochaine échéance.

Mais la maintenance avancée pourra être intégrée après validation du cœur du produit.

## ⛽ Carburant

Une première version simple peut être envisagée :

- quantité ;
- montant ;
- véhicule ;
- kilométrage.

Les analyses avancées peuvent attendre.

## 🛂 Contrôle embarquement

Un contrôle simple par numéro de billet peut être inclus.

Le QR Code peut être ajouté rapidement si cela ne ralentit pas le MVP.

---

# 16. 🔵 Fonctionnalités V2

Après validation du MVP :

## 📦 Colis

- gestion complète ;
- suivi ;
- notifications ;
- retrait ;
- tarification avancée.

## 💳 Paiements

- Mobile Money ;
- paiement en ligne ;
- intégration des moyens de paiement pertinents.

## 📱 Applications mobiles

- chauffeur ;
- contrôleur ;
- passager.

## 🔔 Notifications

- SMS ;
- WhatsApp ;
- email ;
- push.

## 📊 Analytics

- rentabilité par véhicule ;
- rentabilité par ligne ;
- performance chauffeur ;
- analyse carburant ;
- analyse maintenance.

---

# 17. 🔵 Fonctionnalités V3

## 🌐 Réservation publique

Portail web permettant aux passagers de :

- rechercher ;
- réserver ;
- payer ;
- recevoir leur billet.

## 📍 GPS

- position ;
- historique ;
- suivi ;
- alertes.

## 🧠 Optimisation

- optimisation des affectations ;
- analyse prédictive ;
- prévision de demande.

## 🤝 Marketplace

- plusieurs transporteurs ;
- recherche centralisée ;
- réservation multi-opérateurs.

---

# 18. 🔴 Hors périmètre initial

Les éléments suivants ne doivent pas entrer dans le MVP sauf décision exceptionnelle :

- ❌ marketplace ;
- ❌ GPS temps réel ;
- ❌ IA ;
- ❌ comptabilité complète ;
- ❌ optimisation automatique avancée ;
- ❌ programme de fidélité complexe ;
- ❌ intégrations multiples ;
- ❌ application mobile complète ;
- ❌ plateforme publique multi-transporteurs.

---

# 19. 👥 MVP par utilisateur

| Utilisateur | Fonctionnalités MVP |
|---|---|
| 👔 Direction | Dashboard, rapports, agences, flotte, voyages, recettes |
| 🗓️ Exploitation | Planning, voyages, affectations |
| 🧑‍💼 Responsable agence | Activité agence, agents, départs, caisse |
| 🎫 Agent | Recherche, réservation, vente, passagers, caisse |
| 🔧 Flotte | Véhicules, disponibilité, documents de base |
| 👨‍✈️ Chauffeur | Missions et informations de base |
| 🛂 Contrôleur | Vérification billet / embarquement |
| 👤 Passager | Gestion par l'agent dans le MVP |
| ⚙️ Admin plateforme | Administration SaaS |

---

# 20. 🔄 Le workflow MVP complet

Le MVP doit permettre de réaliser le scénario suivant sans intervention extérieure :

```text
1️⃣ Créer une entreprise
        ↓
2️⃣ Créer une agence
        ↓
3️⃣ Créer une ligne
        ↓
4️⃣ Créer un véhicule
        ↓
5️⃣ Créer un chauffeur
        ↓
6️⃣ Planifier un voyage
        ↓
7️⃣ Affecter véhicule + chauffeur
        ↓
8️⃣ Ouvrir les ventes
        ↓
9️⃣ Vendre des billets
        ↓
🔟 Encaisser
        ↓
1️⃣1️⃣ Embarquer
        ↓
1️⃣2️⃣ Départ
        ↓
1️⃣3️⃣ Arrivée
        ↓
1️⃣4️⃣ Clôturer le voyage
        ↓
1️⃣5️⃣ Clôturer la caisse
        ↓
1️⃣6️⃣ Consulter les résultats
```

> 🎯 **Ce scénario constitue le test ultime du MVP.**

---

# 21. 🧪 Critères de réussite du MVP

Le MVP pourra être considéré comme fonctionnel lorsque :

### Fonctionnel

- une entreprise peut être créée ;
- plusieurs agences peuvent être gérées ;
- des utilisateurs peuvent être créés ;
- une flotte peut être enregistrée ;
- des chauffeurs peuvent être enregistrés ;
- un voyage peut être planifié ;
- un véhicule et un chauffeur peuvent être affectés ;
- les billets peuvent être vendus ;
- les sièges sont correctement gérés ;
- les paiements sont enregistrés ;
- les caisses peuvent être clôturées ;
- un voyage peut être exécuté ;
- les recettes peuvent être consultées.

### Sécurité

- les données des entreprises sont isolées ;
- les permissions fonctionnent ;
- les actions sensibles sont journalisées.

### Fiabilité

- aucune double vente de siège ;
- aucune incohérence entre billet et caisse ;
- les statuts sont respectés ;
- les opérations importantes sont traçables.

### Utilisabilité

Un agent de guichet doit pouvoir vendre un billet rapidement sans formation technique complexe.

---

# 22. ⏱️ Estimation initiale du développement

Avec une équipe de **4 développeurs**, une première estimation pourrait être :

| Phase | Durée indicative |
|---|---:|
| 🎯 Cadrage détaillé | 1-2 semaines |
| 🏗️ Architecture + setup | 1-2 semaines |
| 🔐 Auth / multi-tenant / rôles | 2-3 semaines |
| 🏢 Administration / agences | 1-2 semaines |
| 📚 Référentiels | 1-2 semaines |
| 🚌 Flotte / chauffeurs | 2-3 semaines |
| 📅 Planification / voyages | 3-4 semaines |
| 🎫 Billetterie / sièges | 3-5 semaines |
| 💰 Caisses / finance | 2-3 semaines |
| 📊 Dashboard | 1-2 semaines |
| 🧪 Tests / corrections | 2-4 semaines |
| 🚀 Déploiement pilote | 1-2 semaines |

### Estimation globale

> **Environ 4 à 6 mois pour un MVP sérieux**, selon le niveau d'expérience de l'équipe, les choix techniques, la disponibilité des développeurs et surtout la stabilité des règles métier.

Cette estimation devra être réévaluée après l'architecture technique et le découpage en tâches.

---

# 23. 🚨 Risques de dérive du MVP

Le projet risque de grossir rapidement.

Exemples :

> « Puisqu'on gère les véhicules, ajoutons le GPS. »

> « Puisqu'on a les passagers, faisons une application mobile. »

> « Puisqu'on a les voyages, créons une marketplace. »

> « Puisqu'on a les finances, faisons toute la comptabilité. »

Ces idées peuvent être excellentes.

Mais elles ne doivent pas toutes entrer dans le MVP.

## Règle proposée

Avant d'ajouter une fonctionnalité au MVP, poser trois questions :

1. 🎯 Est-elle indispensable au workflow principal ?
2. 💰 Apporte-t-elle une valeur immédiate au premier client ?
3. 🚀 Empêche-t-elle le produit de fonctionner correctement si elle est absente ?

Si la réponse est non aux trois questions :

> **La fonctionnalité est candidate pour une version ultérieure.**

---

# 24. 🧭 Définition du « MVP terminé »

Le MVP n'est pas terminé parce que :

> « Toutes les pages sont développées. »

Il est terminé lorsque :

> **Une véritable entreprise de transport peut l'utiliser pour gérer quotidiennement son cycle de planification, de billetterie et de suivi des recettes sans devoir contourner le système avec Excel ou du papier pour les opérations essentielles.**

Cette définition est fondamentale.

---

# 25. 🎯 Objectif du premier pilote

Le premier objectif commercial ne doit pas être :

> « Obtenir 1 000 clients. »

Il doit être :

> **Trouver une première entreprise pilote prête à utiliser le produit dans une partie réelle de son activité et à nous dire ce qui fonctionne ou non.**

Le pilote permettra de valider :

- les workflows ;
- les règles métier ;
- l'ergonomie ;
- la fiabilité ;
- les indicateurs ;
- les besoins réellement prioritaires.

---

# 26. 🧠 Principe de validation

Le développement devra fonctionner par cycles courts :

```text
💡 Besoin
   ↓
📋 Spécification
   ↓
💻 Développement
   ↓
🧪 Test
   ↓
👤 Test utilisateur
   ↓
📊 Retour
   ↓
🔄 Amélioration
```

Nous ne devons pas attendre la fin de six mois pour découvrir que le workflow d'un agent de guichet ne correspond pas à la réalité du terrain.

---

# 27. 🎯 Conclusion

Le MVP doit rester volontairement limité.

Il doit néanmoins être **complet sur son cœur de métier** :

> **Planifier → affecter → vendre → embarquer → voyager → clôturer → analyser.**

La qualité du MVP sera plus importante que le nombre de fonctionnalités.

Une première version simple mais réellement utilisée par une entreprise de transport aura beaucoup plus de valeur qu'un ERP immense que personne n'utilise.

---

## 📌 Prochaine étape

**Document 07 — Architecture fonctionnelle**

Nous allons maintenant passer du « quoi » au « comment le produit est organisé ».

Nous définirons notamment :

- 🧱 les couches fonctionnelles ;
- 🧩 les domaines/modules ;
- 🔗 les dépendances ;
- 🗃️ les grandes entités métier ;
- 🔄 les flux entre modules ;
- 🔐 les frontières de responsabilité ;
- 🌐 la logique multi-tenant ;
- 📱 les interfaces futures.

Ce document préparera la discussion technique avec les développeurs sans encore imposer une technologie particulière.
