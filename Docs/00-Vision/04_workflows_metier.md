# 🔄 ERP TRANSPORT
## Document 04 — Workflows métier

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`

---

# 1. 🎯 Objectif du document

Ce document décrit les principaux workflows métier de l'ERP Transport.

Un workflow décrit **comment une opération se déroule réellement dans l'entreprise**, depuis son déclenchement jusqu'à sa fin.

L'objectif est de transformer les besoins métier en processus compréhensibles par :

- 👔 les responsables métier ;
- 👨‍💻 les développeurs ;
- 🧪 les testeurs ;
- 🏗️ les responsables de l'architecture.

> 💡 **Principe :** avant de décider comment coder une fonctionnalité, nous devons comprendre précisément comment l'opération fonctionne dans la réalité.

---

# 2. 🗺️ Vue d'ensemble du cycle d'exploitation

Le cycle principal d'une entreprise de transport peut être représenté ainsi :

```text
📚 RÉFÉRENTIEL
      ↓
📅 PLANIFICATION
      ↓
🛣️ CRÉATION DU VOYAGE
      ↓
🚌 AFFECTATION VÉHICULE
      +
👨‍✈️ AFFECTATION CHAUFFEUR
      ↓
🎫 OUVERTURE DES VENTES
      ↓
🎫 RÉSERVATIONS / VENTES
      ↓
🛂 EMBARQUEMENT
      ↓
🚦 DÉPART
      ↓
🛣️ VOYAGE EN COURS
      ↓
🏁 ARRIVÉE
      ↓
💰 CLÔTURE FINANCIÈRE
      ↓
📊 ANALYSE
```

Ce cycle constitue le **workflow métier principal** du produit.

---

# 3. 🛣️ Workflow 01 — Création et planification d'un voyage

## 🎯 Objectif

Préparer un départ en affectant toutes les ressources nécessaires.

## 👤 Acteur principal

🗓️ Responsable d'exploitation

## 🔗 Acteurs secondaires

- 🚌 Responsable flotte ;
- 👨‍✈️ Chauffeur ;
- 🎫 Agent de guichet.

## Étape 1 — Choisir la ligne

Le responsable sélectionne une ligne existante.

Exemple :

> 🛣️ Yaoundé → Douala

La ligne contient notamment :

- origine ;
- destination ;
- distance éventuelle ;
- tarif de référence ;
- durée estimée.

## Étape 2 — Définir la date et l'heure

Exemple :

> 📅 15 août 2026  
> 🕐 07h30

## Étape 3 — Sélectionner le véhicule

Le système propose les véhicules compatibles et disponibles.

Le système doit vérifier :

- véhicule actif ;
- véhicule disponible ;
- absence d'autre affectation incompatible ;
- absence d'immobilisation ;
- documents obligatoires valides ;
- capacité suffisante.

## Étape 4 — Sélectionner le chauffeur

Le système doit vérifier :

- chauffeur actif ;
- disponibilité ;
- absence de conflit ;
- permis compatible ;
- éventuelles restrictions.

## Étape 5 — Créer le voyage

Le système crée le voyage avec un statut initial.

```text
📝 BROUILLON
```

ou directement :

```text
📅 PLANIFIÉ
```

selon la décision d'architecture fonctionnelle.

## Étape 6 — Validation

Le responsable valide le voyage.

Le système peut alors générer :

- la capacité disponible ;
- le plan de sièges ;
- le tarif ;
- les informations nécessaires à la billetterie.

## Workflow

```text
🛣️ Ligne
   ↓
📅 Date / heure
   ↓
🚌 Véhicule
   ↓
👨‍✈️ Chauffeur
   ↓
🔍 Contrôles
   ↓
📝 Voyage
   ↓
✅ Validation
```

---

# 4. 🎫 Workflow 02 — Ouverture des ventes

## 🎯 Objectif

Rendre un voyage disponible à la réservation et à la vente.

## Acteur

🗓️ Responsable d'exploitation

## Conditions préalables

Le voyage doit disposer de :

- ligne ;
- date ;
- heure ;
- véhicule ;
- capacité ;
- tarif ;
- statut autorisant l'ouverture des ventes.

## Action

Le responsable ouvre le voyage aux ventes.

Statut :

```text
📅 Planifié
      ↓
🎫 Ouvert aux ventes
```

À partir de ce moment :

- les agents peuvent vendre ;
- les réservations peuvent être créées ;
- les sièges disponibles peuvent être consultés.

---

# 5. 🎫 Workflow 03 — Réservation d'une place

## 🎯 Objectif

Permettre de réserver une place avant paiement définitif.

## Acteur principal

🎫 Agent de guichet

## Étape 1 — Recherche

L'agent recherche :

- destination ;
- date ;
- éventuellement horaire.

## Étape 2 — Sélection du voyage

Le système affiche les voyages disponibles.

Exemple :

```text
Yaoundé → Douala

07h30 — BUS-023 — 12 places disponibles
10h00 — BUS-018 — 27 places disponibles
14h00 — BUS-031 — 31 places disponibles
```

## Étape 3 — Sélection du siège

L'agent consulte le plan de sièges.

```text
🟢 Disponible
🟡 Réservé
🔵 Payé
⚪ Embarqué
🔴 Bloqué
```

## Étape 4 — Informations passager

L'agent renseigne les informations nécessaires.

## Étape 5 — Création de la réservation

Le système crée la réservation.

```text
🟡 RÉSERVÉ
```

Une durée de validité pourra être définie.

Exemple :

> Réservation valable 30 minutes.

Cette durée devra être définie dans les règles métier.

## Workflow

```text
🔎 Recherche
   ↓
🛣️ Voyage
   ↓
💺 Siège
   ↓
👤 Passager
   ↓
🟡 Réservation
```

---

# 6. 💰 Workflow 04 — Vente d'un billet

## 🎯 Objectif

Transformer une réservation ou une sélection directe en billet payé.

## Acteur

🎫 Agent de guichet

## Étape 1 — Sélection

L'agent sélectionne :

- voyage ;
- siège ;
- passager.

## Étape 2 — Calcul du montant

Le système applique le tarif correspondant.

## Étape 3 — Paiement

Le paiement est enregistré.

Dans le MVP, les modes peuvent être limités à :

- espèces ;
- éventuellement paiement électronique selon les moyens disponibles.

Les intégrations Mobile Money pourront être ajoutées ultérieurement.

## Étape 4 — Génération du billet

Le système génère un billet contenant notamment :

- numéro unique ;
- passager ;
- voyage ;
- siège ;
- date ;
- heure ;
- origine ;
- destination ;
- montant ;
- statut ;
- QR Code éventuel.

## Étape 5 — Mise à jour automatique

Une vente doit provoquer automatiquement :

```text
🎫 Billet vendu
      ↓
💺 Siège occupé
      ↓
👤 Passager associé
      ↓
💰 Recette créée
      ↓
🏦 Caisse mise à jour
      ↓
📊 Taux de remplissage actualisé
```

> ⚠️ Le système ne doit pas demander à l'agent de saisir manuellement la recette une deuxième fois.

---

# 7. ❌ Workflow 05 — Annulation d'un billet

## 🎯 Objectif

Annuler correctement une vente ou une réservation selon les règles de l'entreprise.

## Acteur

🎫 Agent ou responsable autorisé.

## Étape 1 — Recherche du billet

Recherche par :

- numéro ;
- nom ;
- téléphone ;
- voyage.

## Étape 2 — Vérification

Le système vérifie :

- statut actuel ;
- voyage concerné ;
- date et heure ;
- conditions d'annulation ;
- utilisateur autorisé.

## Étape 3 — Annulation

Le billet passe à :

```text
🔴 ANNULÉ
```

Le siège peut redevenir disponible selon les règles.

## Étape 4 — Remboursement éventuel

Si le billet était déjà payé, un remboursement peut être nécessaire.

Le système doit conserver la trace :

```text
Billet
   ↓
Annulation
   ↓
Remboursement éventuel
   ↓
Mouvement financier
   ↓
Journalisation
```

> ❓ Les conditions exactes de remboursement devront être définies dans le document des règles métier.

---

# 8. 🔄 Workflow 06 — Modification / changement de voyage

Un passager peut demander à changer :

- de siège ;
- d'horaire ;
- de voyage ;
- éventuellement de date.

Le système devra vérifier :

- disponibilité du nouveau voyage ;
- disponibilité du siège ;
- différence tarifaire éventuelle ;
- autorisation de modification.

## Exemple

```text
Billet actuel
    ↓
Demande de modification
    ↓
Vérification
    ↓
Nouveau voyage
    ↓
Nouveau siège
    ↓
Ajustement financier éventuel
    ↓
Historique de modification
```

---

# 9. 🛂 Workflow 07 — Embarquement

## 🎯 Objectif

Vérifier que les passagers présents disposent d'un billet valide.

## Acteur

🛂 Contrôleur / agent d'embarquement

## Méthode

Le contrôleur peut :

- scanner un QR Code ;
- rechercher le numéro du billet ;
- rechercher le passager.

## Vérifications

```text
🎫 Billet
   ↓
Valide ?
   ├── ❌ Non → Refus
   │
   └── ✅ Oui
          ↓
     Passager autorisé
          ↓
       🛂 Embarqué
```

Le billet passe alors à un statut indiquant que le passager a embarqué.

---

# 10. 🚦 Workflow 08 — Départ du voyage

## Acteur principal

👨‍✈️ Chauffeur ou responsable d'exploitation.

## Avant départ

Le système peut vérifier :

- véhicule ;
- chauffeur ;
- nombre de passagers ;
- statut du voyage ;
- éventuels incidents ;
- embarquement.

## Départ

Le responsable confirme le départ.

Le voyage passe :

```text
🛂 Embarquement
      ↓
🚦 Départ
      ↓
🛣️ En cours
```

Le système peut enregistrer :

- heure réelle de départ ;
- kilométrage départ ;
- conducteur ;
- véhicule ;
- nombre de passagers.

---

# 11. 🏁 Workflow 09 — Arrivée et clôture opérationnelle

## Étape 1 — Arrivée

Le chauffeur ou responsable confirme l'arrivée.

Le système enregistre :

- heure réelle d'arrivée ;
- kilométrage arrivée ;
- incidents éventuels.

## Étape 2 — Calculs

Le système peut calculer :

- durée réelle ;
- distance parcourue ;
- nombre de passagers ;
- recettes ;
- dépenses enregistrées.

## Étape 3 — Clôture opérationnelle

Le voyage passe :

```text
🛣️ En cours
      ↓
🏁 Arrivé
      ↓
✅ Clôturé opérationnellement
```

Une clôture financière peut rester nécessaire.

---

# 12. 💰 Workflow 10 — Clôture financière d'un voyage

## 🎯 Objectif

Déterminer le résultat économique du voyage.

## Recettes

Le système agrège notamment :

- billets ;
- colis ;
- autres recettes éventuelles.

## Dépenses

Selon le niveau de détail retenu :

- carburant ;
- péages ;
- frais de mission ;
- dépenses exceptionnelles ;
- autres coûts directement associés.

## Exemple

```text
💰 RECETTES
Billets       175 000
Colis          25 000
              ───────
Total         200 000 FCFA

💸 COÛTS
Carburant      55 000
Péages         10 000
Autres         15 000
              ───────
Total          80 000 FCFA

📊 MARGE ESTIMÉE
              120 000 FCFA
```

> ⚠️ La notion exacte de marge devra être définie. Certains coûts, comme l'amortissement du véhicule ou certains coûts de personnel, ne sont pas nécessairement imputés directement au voyage dans le MVP.

---

# 13. 🏦 Workflow 11 — Ouverture et clôture d'une caisse

## 🎯 Objectif

Contrôler les mouvements financiers d'un agent ou d'une agence.

## Ouverture

```text
👤 Agent
   ↓
🏦 Ouverture caisse
   ↓
💵 Fonds initial
```

## Pendant la journée

```text
🎫 Ventes
   ↓
💰 Encaissements
   ↓
🏦 Caisse
```

Peuvent également intervenir :

- remboursements ;
- retraits ;
- dépenses autorisées ;
- versements.

## Clôture

L'agent indique le montant réellement présent.

Le système compare :

```text
Solde théorique
      VS
Solde réel
      ↓
📊 Écart éventuel
```

Exemple :

> Solde théorique : 450 000 FCFA  
> Solde réel : 445 000 FCFA  
> Écart : -5 000 FCFA

L'écart doit être journalisé et éventuellement soumis à validation.

---

# 14. 🔧 Workflow 12 — Maintenance d'un véhicule

## 🎯 Objectif

Prévenir les pannes et gérer les interventions.

## Déclencheurs

Une maintenance peut être déclenchée :

- par kilométrage ;
- par durée ;
- par incident ;
- par inspection ;
- manuellement.

## Exemple

```text
🚌 BUS-023
      ↓
185 430 km
      ↓
Seuil : 190 000 km
      ↓
⚠️ Maintenance à préparer
      ↓
📅 Planification intervention
      ↓
⛔ Immobilisation
      ↓
🔧 Intervention
      ↓
💰 Coût
      ↓
✅ Véhicule disponible
```

La maintenance doit alimenter l'historique du véhicule et les statistiques de coûts.

---

# 15. ⛽ Workflow 13 — Enregistrement du carburant

## 🎯 Objectif

Tracer les consommations de carburant et leur coût.

## Données principales

- véhicule ;
- date ;
- quantité ;
- prix unitaire ;
- montant ;
- kilométrage ;
- station ;
- voyage ou mission éventuelle.

## Workflow

```text
⛽ Plein
   ↓
🚌 Véhicule
   ↓
📏 Kilométrage
   ↓
💰 Coût
   ↓
📊 Consommation
```

Le système pourra ensuite calculer :

- coût/km ;
- litres/100 km ;
- coût moyen ;
- évolution de consommation.

---

# 16. 📦 Workflow 14 — Gestion d'un colis

## 🎯 Objectif

Permettre à une agence d'enregistrer un colis qui sera transporté.

## Dépôt

L'agent enregistre :

- expéditeur ;
- destinataire ;
- téléphone ;
- description ;
- quantité ;
- poids si nécessaire ;
- montant ;
- agence d'arrivée.

## Association au voyage

Le colis est affecté à un voyage.

```text
📦 Colis
   ↓
🏪 Agence départ
   ↓
🛣️ Voyage
   ↓
🏪 Agence arrivée
```

## Arrivée

Le colis est marqué comme disponible.

```text
📦 En transit
      ↓
🏁 Arrivé
      ↓
📱 Destinataire informé
      ↓
👤 Retrait
      ↓
✅ Livré
```

---

# 17. 🔁 Workflow 15 — Remplacement d'un véhicule

Cas important pour une entreprise de transport.

## Situation

Un véhicule affecté à un voyage tombe en panne avant le départ.

## Workflow

```text
🚌 Véhicule initial
      ↓
⚠️ Panne
      ↓
⛔ Indisponible
      ↓
🔍 Recherche véhicule disponible
      ↓
🚌 Nouveau véhicule
      ↓
🔄 Mise à jour du voyage
      ↓
🎫 Billets conservés
      ↓
📢 Information éventuelle
```

Le système devra éviter de perdre les réservations existantes.

La capacité du nouveau véhicule doit également être vérifiée.

---

# 18. 👨‍✈️ Workflow 16 — Remplacement d'un chauffeur

Même logique en cas d'indisponibilité d'un chauffeur.

```text
👨‍✈️ Chauffeur initial
      ↓
⚠️ Indisponible
      ↓
🔍 Recherche chauffeur disponible
      ↓
👨‍✈️ Nouveau chauffeur
      ↓
🔄 Mise à jour du voyage
      ↓
🧾 Journalisation
```

Le système doit vérifier :

- disponibilité ;
- permis ;
- statut ;
- éventuelles restrictions.

---

# 19. ⚠️ Workflow 17 — Gestion d'un incident

Un incident peut être :

- panne ;
- accident ;
- retard ;
- problème passager ;
- problème administratif ;
- problème de véhicule.

## Workflow

```text
⚠️ Incident
      ↓
📝 Déclaration
      ↓
📸 Informations / preuves éventuelles
      ↓
👤 Responsable
      ↓
🔧 Action corrective
      ↓
✅ Résolution
      ↓
📊 Historique
```

Le niveau de détail pourra être limité dans le MVP.

---

# 20. 📊 Workflow 18 — Production des indicateurs

Les données opérationnelles doivent être agrégées automatiquement.

## Exemple

```text
🎫 Billets
   +
🛣️ Voyages
   +
🚌 Véhicules
   +
⛽ Carburant
   +
🔧 Maintenance
   +
💰 Finance
        ↓
     📊 REPORTING
```

## Indicateurs possibles

### Commercial

- billets vendus ;
- taux de remplissage ;
- chiffre d'affaires ;
- réservations ;
- annulations.

### Exploitation

- nombre de voyages ;
- retards ;
- annulations ;
- incidents.

### Flotte

- disponibilité ;
- immobilisation ;
- coûts ;
- consommation ;
- maintenance.

### Financier

- recettes ;
- dépenses ;
- marge ;
- performance par ligne ;
- performance par agence.

---

# 21. 🔗 Workflow global intégré

Tous les workflows doivent fonctionner ensemble.

```text
                    📚 RÉFÉRENTIELS
                           │
                           ▼
                    📅 PLANIFICATION
                           │
                           ▼
                     🛣️ VOYAGE
                    ╱      │      ╲
                   ╱       │       ╲
                  ▼        ▼        ▼
             🚌 FLOTTE  👨‍✈️ CHAUFFEUR 🎫 BILLETS
                  │        │        │
                  │        │        ▼
                  │        │     👤 PASSAGERS
                  │        │        │
                  │        │        ▼
                  │        │      💰 CAISSE
                  │        │        │
                  ▼        ▼        ▼
               ⛽ CARBURANT / 🔧 MAINTENANCE
                           │
                           ▼
                       💰 FINANCE
                           │
                           ▼
                       📊 REPORTING
```

Ce schéma représente la logique générale que l'équipe devra garder en tête pendant la conception.

---

# 22. 🧠 Événements métier importants

Un point important pour la suite de l'architecture est d'identifier les événements significatifs.

Exemples :

```text
🎫 Billet vendu
🎫 Billet annulé
💰 Paiement enregistré
🚌 Véhicule affecté
🚌 Véhicule remplacé
👨‍✈️ Chauffeur affecté
🛣️ Voyage ouvert
🚦 Voyage départ
🏁 Voyage arrivé
💰 Caisse clôturée
🔧 Maintenance créée
⚠️ Incident déclaré
📦 Colis déposé
📦 Colis livré
```

Ces événements pourront déclencher automatiquement des actions dans d'autres parties du système.

---

# 23. ⚠️ Cas particuliers à anticiper

Les workflows nominaux ne suffisent pas.

L'équipe devra également réfléchir aux situations exceptionnelles :

- voyage annulé ;
- véhicule en panne ;
- chauffeur indisponible ;
- retard important ;
- billet remboursé ;
- réservation expirée ;
- siège doublement réservé ;
- erreur de caisse ;
- paiement enregistré mais billet non généré ;
- billet généré mais paiement échoué ;
- perte de connexion ;
- modification du tarif ;
- changement de capacité du véhicule ;
- agence temporairement fermée.

Ces cas feront partie du prochain document consacré aux **règles métier**.

---

# 24. 🧪 Workflows et tests

Chaque workflow devra pouvoir être transformé en scénarios de test.

Exemple :

### Cas nominal

> Un agent vend un billet pour un voyage disponible.

Résultat attendu :

- billet créé ;
- siège occupé ;
- paiement enregistré ;
- recette créée ;
- caisse mise à jour.

### Cas d'erreur

> Deux agents tentent de vendre le même siège simultanément.

Résultat attendu :

> Une seule vente doit être validée.

### Cas d'erreur

> Un agent tente de vendre un billet sur un voyage fermé.

Résultat attendu :

> La vente est refusée.

Cette approche permettra de relier directement :

**Workflow → Règle métier → Test → Fonctionnalité.**

---

# 25. 🎯 Conclusion

Les workflows constituent le pont entre la vision métier et la conception technique.

Ils permettent de comprendre :

- qui fait quoi ;
- dans quel ordre ;
- avec quelles données ;
- quelles validations sont nécessaires ;
- quels événements doivent être enregistrés ;
- quelles actions doivent être automatiques ;
- quels cas d'erreur doivent être gérés.

Le projet ne devra donc pas être conçu uniquement à partir des écrans.

> **Nous devons d'abord modéliser les processus métier, puis construire les écrans et les services nécessaires pour les supporter.**

---

## 📌 Prochaine étape

**Document 05 — Règles métier**

Nous allons transformer les workflows précédents en règles explicites.

Exemples :

- Un véhicule indisponible ne peut pas être affecté à un voyage.
- Un siège ne peut être vendu qu'une seule fois pour un voyage donné.
- Un billet payé ne peut pas être simplement supprimé.
- Un voyage fermé ne peut plus accepter de nouvelles ventes.
- Une caisse clôturée ne peut plus être modifiée librement.
- Un chauffeur ne peut pas être affecté à deux voyages incompatibles.
- Un remboursement doit être lié au billet initial.

Ce document sera particulièrement important pour les développeurs, car il constituera une partie essentielle du **contrat fonctionnel du produit**.
