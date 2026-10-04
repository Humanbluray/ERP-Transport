# 🧩 ERP TRANSPORT
## Document 03 — Modules fonctionnels

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`

---

# 1. 🎯 Objectif du document

Ce document présente les grands modules fonctionnels de l'ERP Transport.

L'objectif n'est pas encore de définir les écrans ou la base de données, mais de répondre à trois questions :

1. **Quelles sont les grandes capacités du produit ?**
2. **Quel rôle joue chaque module ?**
3. **Comment les modules communiquent-ils entre eux ?**

> 💡 **Principe :** un module doit représenter une responsabilité métier identifiable. Il ne doit pas être créé uniquement parce qu'une fonctionnalité nécessite un nouvel écran.

---

# 2. 🗺️ Vue d'ensemble fonctionnelle

L'ERP peut être représenté autour de plusieurs domaines :

```text
                         🌐 ERP TRANSPORT
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
     🏢 ADMINISTRATION      🏪 AGENCES          📚 RÉFÉRENTIELS
          │                     │                     │
          └─────────────────────┼─────────────────────┘
                                │
                                ▼
                    📅 PLANIFICATION & EXPLOITATION
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                 🚌 FLOTTE   👨‍✈️ CHAUFFEURS  🛣️ VOYAGES
                    │           │           │
                    └───────────┼───────────┘
                                │
                                ▼
                         🎫 BILLETTERIE
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                💰 FINANCE    📦 COLIS    👤 CLIENTS
                    │           │           │
                    └───────────┼───────────┘
                                ▼
                         📊 REPORTING
                                │
                                ▼
                         🔔 NOTIFICATIONS
```

---

# 3. 🏢 Module Administration

## 🎯 Responsabilité

Gérer la structure de l'entreprise et les utilisateurs qui utilisent le système.

## Fonctionnalités principales

### Entreprise

- informations générales ;
- identité de l'entreprise ;
- paramètres ;
- coordonnées ;
- configuration générale.

### Agences

- création d'une agence ;
- modification ;
- activation / désactivation ;
- coordonnées ;
- horaires ;
- responsables ;
- statut.

### Utilisateurs

- création ;
- activation / désactivation ;
- rattachement à une entreprise ;
- rattachement à une ou plusieurs agences ;
- rôles ;
- permissions.

### Sécurité

- gestion des sessions ;
- historique des connexions ;
- journal des actions sensibles ;
- gestion des accès.

## 🔗 Interactions

Le module Administration est transversal.

```text
Administration
      │
      ├── Agences
      ├── Utilisateurs
      ├── Rôles
      └── Permissions
              │
              ▼
       Tous les autres modules
```

---

# 4. 🏪 Module Agences

## 🎯 Responsabilité

Gérer les différents points d'exploitation ou de vente de l'entreprise.

Une entreprise peut posséder plusieurs agences.

## Fonctionnalités

- créer une agence ;
- gérer ses informations ;
- définir ses horaires ;
- gérer ses agents ;
- gérer ses caisses ;
- consulter ses ventes ;
- consulter ses départs ;
- consulter ses performances.

## Exemple

```text
Entreprise
    │
    ├── Agence Yaoundé
    │      ├── Guichet 1
    │      ├── Guichet 2
    │      └── Caisse
    │
    ├── Agence Douala
    │      ├── Guichet 1
    │      └── Caisse
    │
    └── Agence Bafoussam
           └── Guichet 1
```

## 🔗 Interactions

Les agences sont notamment liées à :

- utilisateurs ;
- caisses ;
- voyages ;
- ventes ;
- réservations ;
- recettes ;
- reporting.

---

# 5. 📚 Module Référentiels

## 🎯 Responsabilité

Centraliser les informations de référence utilisées par les autres modules.

C'est un module particulièrement important pour éviter les doublons et les incohérences.

## Référentiels possibles

### Géographiques

- villes ;
- régions ;
- pays ;
- points de départ ;
- points d'arrivée.

### Transport

- lignes ;
- trajets ;
- arrêts éventuels ;
- catégories de véhicules ;
- types de voyages.

### Commercial

- tarifs ;
- classes ;
- types de billets ;
- conditions tarifaires.

### Opérationnels

- statuts ;
- motifs d'annulation ;
- types d'incidents ;
- types de dépenses.

## Exemple

```text
📍 Ville
   ↓
🛣️ Ligne
   ↓
📅 Horaire
   ↓
🚌 Voyage
```

## 🔗 Interactions

Les référentiels sont utilisés par :

- planification ;
- voyages ;
- billetterie ;
- finance ;
- reporting.

---

# 6. 🚌 Module Flotte

## 🎯 Responsabilité

Gérer les véhicules de l'entreprise et leur disponibilité opérationnelle.

## Fonctionnalités principales

### Véhicules

- immatriculation ;
- marque ;
- modèle ;
- catégorie ;
- capacité ;
- kilométrage ;
- statut ;
- agence ou parc de rattachement.

### Documents

- assurance ;
- visite technique ;
- carte grise ;
- autres documents ;
- dates d'expiration.

### Exploitation

- disponibilité ;
- affectation ;
- immobilisation ;
- historique.

### Coûts

- carburant ;
- maintenance ;
- autres dépenses liées au véhicule.

## États possibles

```text
🟢 Disponible
🟡 Affecté
🔵 En voyage
🟠 Maintenance
🔴 Immobilisé
⚫ Hors service
```

## 🔗 Interactions

```text
Flotte
  │
  ├── Planification
  ├── Voyages
  ├── Chauffeurs
  ├── Maintenance
  ├── Carburant
  ├── Finance
  └── Reporting
```

---

# 7. 👨‍✈️ Module Chauffeurs

## 🎯 Responsabilité

Gérer les conducteurs et leur relation avec les opérations de transport.

## Fonctionnalités

- profil chauffeur ;
- coordonnées ;
- permis ;
- catégories de permis ;
- dates d'expiration ;
- statut ;
- agence de rattachement ;
- historique des affectations ;
- incidents ;
- performances.

## Exemple

```text
👨‍✈️ Jean

Permis : Catégorie D
Statut : Actif
Agence : Yaoundé
Voyages ce mois : 38
Incidents : 1
```

## 🔗 Interactions

- utilisateurs ;
- voyages ;
- véhicules ;
- incidents ;
- reporting.

---

# 8. 📅 Module Planification

## 🎯 Responsabilité

Préparer les opérations avant leur exécution.

C'est ici que l'entreprise organise ses futurs départs.

## Fonctionnalités

- création des horaires ;
- programmation des départs ;
- calendrier ;
- affectation des véhicules ;
- affectation des chauffeurs ;
- contrôle des conflits ;
- modification ;
- annulation.

## Exemple

```text
📅 15 août — 07h30

Yaoundé → Douala
        │
        ├── 🚌 BUS-023
        └── 👨‍✈️ Jean
```

## Contrôles importants

Le système devra pouvoir détecter :

- véhicule déjà affecté ;
- chauffeur déjà affecté ;
- véhicule indisponible ;
- chauffeur indisponible ;
- maintenance prévue ;
- capacité insuffisante ;
- conflit d'horaire.

---

# 9. 🛣️ Module Voyages / Exploitation

## 🎯 Responsabilité

Gérer le cycle de vie réel d'un voyage.

## Cycle de vie

```text
📝 Brouillon
    ↓
📅 Planifié
    ↓
🎫 Ouvert aux ventes
    ↓
🚌 Embarquement
    ↓
🚦 Départ
    ↓
🛣️ En cours
    ↓
🏁 Arrivé
    ↓
💰 À clôturer
    ↓
✅ Clôturé
```

## Informations d'un voyage

Un voyage pourra notamment contenir :

- ligne ;
- date ;
- heure ;
- agence de départ ;
- destination ;
- véhicule ;
- chauffeur ;
- capacité ;
- tarif ;
- nombre de places vendues ;
- passagers ;
- statut ;
- recettes ;
- dépenses ;
- incidents.

## 🔗 Interactions

Le voyage constitue l'un des objets centraux du système.

```text
                 🛣️ VOYAGE
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
    🚌 Véhicule   👨‍✈️ Chauffeur   🎫 Billets
       │             │             │
       └─────────────┼─────────────┘
                     ▼
                 💰 Finance
                     │
                     ▼
                 📊 Reporting
```

---

# 10. 🎫 Module Billetterie & Réservations

## 🎯 Responsabilité

Gérer la commercialisation des places disponibles sur les voyages.

## Fonctionnalités

### Recherche

- destination ;
- date ;
- heure ;
- agence ;
- disponibilité.

### Réservation

- sélection voyage ;
- sélection siège ;
- informations passager ;
- durée de validité ;
- statut.

### Vente

- billet ;
- paiement ;
- reçu ;
- attribution du siège.

### Gestion après vente

- modification ;
- annulation ;
- remboursement ;
- changement de voyage ;
- historique.

## Exemple

```text
👤 Client
   ↓
🔎 Recherche
   ↓
🛣️ Voyage
   ↓
💺 Siège
   ↓
💰 Paiement
   ↓
🎫 Billet
```

## Statuts possibles d'un billet

```text
🟡 Réservé
🟢 Payé
🔵 Embarqué
⚪ Utilisé
🔴 Annulé
↩️ Remboursé
```

Les statuts exacts devront être validés lors de la définition des règles métier.

---

# 11. 💺 Module Gestion des sièges

La gestion des sièges peut être intégrée à la billetterie, mais elle représente une responsabilité fonctionnelle suffisamment importante pour être explicitement identifiée.

## Fonctionnalités

- configuration du plan de sièges ;
- capacité ;
- sièges disponibles ;
- sièges réservés ;
- sièges vendus ;
- sièges bloqués ;
- sièges embarqués.

## Exemple

```text
        🚌 BUS-023 — 35 places

    [01] [02]      [03] [04]
    [05] [06]      [07] [08]
    [09] [10]      [11] [12]
    [13] [14]      [15] [16]
          ...
```

> 💡 La conception devra permettre de gérer plusieurs configurations de véhicules sans coder un plan fixe.

---

# 12. 💰 Module Finance & Caisses

## 🎯 Responsabilité

Tracer les flux financiers liés à l'activité.

## Fonctionnalités

### Caisses

- ouverture ;
- opérations ;
- versements ;
- retraits ;
- clôture ;
- rapprochement.

### Recettes

- ventes de billets ;
- colis ;
- autres recettes.

### Dépenses

- carburant ;
- péages ;
- maintenance ;
- frais de mission ;
- autres dépenses.

### Analyse

- recettes par voyage ;
- recettes par agence ;
- recettes par ligne ;
- dépenses par véhicule ;
- marge estimée.

## Exemple

```text
🎫 Billet vendu : 5 000 FCFA
       ↓
💰 Recette voyage
       ↓
🏦 Caisse agence
       ↓
📊 Chiffre d'affaires
```

---

# 13. ⛽ Module Carburant

## 🎯 Responsabilité

Suivre la consommation et le coût du carburant.

## Fonctionnalités

- plein de carburant ;
- quantité ;
- prix ;
- station ;
- véhicule ;
- kilométrage ;
- date ;
- voyage ou mission associé.

## Indicateurs

- litres consommés ;
- coût total ;
- coût/km ;
- consommation moyenne ;
- évolution de la consommation.

## 🔗 Interactions

```text
⛽ Carburant
    ↓
🚌 Véhicule
    ↓
🛣️ Voyage
    ↓
💰 Coût
    ↓
📊 Rentabilité
```

---

# 14. 🔧 Module Maintenance

## 🎯 Responsabilité

Maintenir les véhicules en état opérationnel.

## Fonctionnalités

- maintenance préventive ;
- maintenance corrective ;
- interventions ;
- pièces ;
- main-d'œuvre ;
- coûts ;
- historique ;
- immobilisation.

## Alertes

- prochaine vidange ;
- échéance kilométrique ;
- échéance temporelle ;
- document expirant ;
- véhicule immobilisé.

## Exemple

```text
🚌 BUS-023
      ↓
185 430 km
      ↓
Prochaine maintenance : 190 000 km
      ↓
⚠️ Préparer intervention
```

---

# 15. 📦 Module Colis

## 🎯 Responsabilité

Gérer les colis transportés à l'occasion des voyages.

## Fonctionnalités

- enregistrement expéditeur ;
- destinataire ;
- colis ;
- poids ou quantité ;
- tarif ;
- voyage associé ;
- agence de dépôt ;
- agence d'arrivée ;
- paiement ;
- statut ;
- retrait.

## Cycle

```text
📦 Dépôt
   ↓
🏪 Agence départ
   ↓
🛣️ Voyage
   ↓
🏪 Agence arrivée
   ↓
📱 Notification
   ↓
👤 Retrait
```

Ce module pourra constituer une source de revenus complémentaire pour les entreprises de transport.

---

# 16. 📊 Module Reporting & Dashboard

## 🎯 Responsabilité

Transformer les données opérationnelles en informations utiles à la décision.

## Indicateurs possibles

### Activité

- nombre de voyages ;
- nombre de billets ;
- taux de remplissage ;
- nombre de passagers.

### Finance

- chiffre d'affaires ;
- recettes ;
- dépenses ;
- marge ;
- évolution.

### Flotte

- véhicules actifs ;
- véhicules immobilisés ;
- coûts de maintenance ;
- consommation.

### Exploitation

- départs ;
- retards ;
- annulations ;
- incidents.

## Exemple de dashboard

```text
📊 TABLEAU DE BORD

🎫 Billets vendus       684
🚌 Voyages              24
💰 Recettes             3,42 M FCFA
📈 Taux remplissage     81 %
⛽ Carburant            1,24 M FCFA
🔧 Véhicules maintenance 3
⚠️ Alertes              7
```

---

# 17. 🔔 Module Notifications

## 🎯 Responsabilité

Informer les utilisateurs lorsqu'une action ou un événement nécessite leur attention.

## Notifications possibles

### Exploitation

- voyage modifié ;
- voyage annulé ;
- véhicule remplacé ;
- chauffeur remplacé.

### Flotte

- maintenance proche ;
- assurance expirante ;
- visite technique expirante.

### Client

- réservation confirmée ;
- billet émis ;
- voyage modifié ;
- départ imminent ;
- colis disponible.

## Canaux possibles

Dans le MVP :

- notifications internes.

Plus tard :

- 📱 SMS ;
- 💬 WhatsApp ;
- 📧 Email ;
- 🔔 notifications push.

---

# 18. 🧾 Module Audit & Journalisation

Ce module mérite d'être prévu dès le départ, même si son interface peut rester simple dans le MVP.

## 🎯 Responsabilité

Conserver une trace des actions importantes effectuées dans le système.

## Exemples

```text
👤 Jean
14:32
Annulation billet #B-00452

👤 Marie
14:35
Modification tarif ligne Yaoundé-Douala

👤 Paul
15:02
Clôture caisse agence Yaoundé
```

## Actions particulièrement sensibles

- modification de tarif ;
- annulation de billet ;
- remboursement ;
- suppression ou modification d'une recette ;
- clôture de caisse ;
- modification d'un voyage ;
- modification d'une affectation ;
- modification des permissions.

---

# 19. 🔗 Interactions entre les modules

Le système doit être pensé comme un ensemble cohérent et non comme une collection d'applications indépendantes.

## Exemple 1 — Création d'un voyage

```text
📚 Référentiel
      ↓
🛣️ Ligne
      ↓
📅 Planification
      ↓
🚌 Véhicule
      +
👨‍✈️ Chauffeur
      ↓
🛣️ Voyage
```

## Exemple 2 — Vente d'un billet

```text
🛣️ Voyage
      ↓
💺 Siège disponible
      ↓
👤 Passager
      ↓
💰 Paiement
      ↓
🎫 Billet
      ↓
🏦 Caisse
      ↓
📊 Reporting
```

## Exemple 3 — Maintenance

```text
🚌 Véhicule
      ↓
🔧 Maintenance
      ↓
⛔ Immobilisation
      ↓
📅 Planification
      ↓
⚠️ Remplacement éventuel
      ↓
🛣️ Voyage
```

---

# 20. 🧠 Le voyage comme objet central

Le voyage constitue un élément central de l'ERP.

Il fait le lien entre :

- la planification ;
- la flotte ;
- les chauffeurs ;
- la billetterie ;
- les passagers ;
- les recettes ;
- les dépenses ;
- les incidents ;
- le reporting.

```text
                         🛣️ VOYAGE
                              │
       ┌──────────┬───────────┼───────────┬──────────┐
       ▼          ▼           ▼           ▼          ▼
    🚌 Flotte  👨‍✈️ Chauffeur 🎫 Billets 👤 Passagers 💰 Finance
       │          │           │           │          │
       └──────────┴───────────┴───────────┴──────────┘
                              │
                              ▼
                         📊 Reporting
```

Cette centralité devra être prise en compte lors de la modélisation fonctionnelle et technique.

---

# 21. 🏗️ Périmètre fonctionnel proposé pour le MVP

Pour éviter de construire trop large dès le départ, le MVP pourrait se concentrer sur :

## 🟢 Priorité 1 — indispensable

- 🏢 Entreprise / utilisateurs / rôles ;
- 🏪 Agences ;
- 📚 Référentiels ;
- 🚌 Véhicules ;
- 👨‍✈️ Chauffeurs ;
- 📅 Planification ;
- 🛣️ Voyages ;
- 🎫 Billetterie ;
- 💺 Sièges ;
- 💰 Caisses / recettes ;
- 📊 Dashboard de base.

## 🟡 Priorité 2 — important mais peut être progressif

- 🔧 Maintenance ;
- ⛽ Carburant ;
- 📄 Documents véhicules ;
- 🛂 Contrôle embarquement ;
- 🔔 Notifications ;
- 🧾 Audit avancé.

## 🔵 Priorité 3 — évolution

- 📦 Colis avancés ;
- 💳 Mobile Money ;
- 📱 application chauffeur ;
- 👤 espace passager ;
- 📍 GPS ;
- 🌐 réservation en ligne ;
- 📊 analytics avancés.

## 🔴 Hors MVP initial

- marketplace ;
- optimisation automatique des trajets ;
- IA ;
- comptabilité complète ;
- intégrations multiples ;
- fonctionnalités complexes de fidélité.

---

# 22. ❓ Questions à valider avant la conception détaillée

L'équipe devra notamment discuter :

### Voyages

- Un voyage est-il toujours lié à une ligne prédéfinie ?
- Peut-on modifier le véhicule après la vente de billets ?
- Peut-on modifier l'horaire après ouverture des ventes ?
- Que se passe-t-il lorsqu'un voyage est annulé ?

### Billetterie

- Un siège peut-il être réservé sans paiement ?
- Combien de temps une réservation reste-t-elle valide ?
- Comment gérer un changement de siège ?
- Comment gérer un remboursement ?

### Véhicules

- Comment gérer plusieurs types de plans de sièges ?
- Un véhicule peut-il avoir plusieurs configurations ?
- Quelles règles rendent un véhicule indisponible ?

### Finance

- Une vente de billet alimente-t-elle immédiatement la caisse ?
- Comment gérer les remboursements ?
- Comment gérer les commissions des agents ?
- Comment gérer les écarts de caisse ?

### Multi-agence

- Un voyage peut-il être vendu depuis plusieurs agences ?
- Une agence peut-elle vendre un voyage créé par une autre agence ?
- Comment répartir les recettes ?

Ces réponses seront transformées dans le document suivant en **règles métier explicites**.

---

# 23. 🎯 Conclusion

Les modules fonctionnels définissent les grandes responsabilités de l'ERP.

Le système devra conserver une architecture fonctionnelle cohérente :

> **Référentiels → Planification → Voyage → Billetterie → Exploitation → Finance → Reporting**

avec la flotte, les chauffeurs, la maintenance et les autres domaines connectés à ce cycle.

La priorité du projet sera de construire un **workflow opérationnel complet et fiable**, plutôt qu'une grande quantité de fonctionnalités isolées.

---

## 📌 Prochaine étape

**Document 04 — Workflows métier**

Nous détaillerons les principaux processus de bout en bout :

1. 🛣️ Création et planification d'un voyage ;
2. 🎫 Réservation et vente d'un billet ;
3. 🚌 Préparation et départ d'un voyage ;
4. 🏁 Arrivée et clôture d'un voyage ;
5. 💰 Clôture de caisse ;
6. 🔧 Maintenance d'un véhicule ;
7. ⛽ Enregistrement du carburant ;
8. 📦 Gestion d'un colis ;
9. ❌ Annulation / modification / remboursement ;
10. 📊 Production des indicateurs de performance.

Ces workflows serviront ensuite de base aux **règles métier, aux cas d'usage et à la conception technique**.
