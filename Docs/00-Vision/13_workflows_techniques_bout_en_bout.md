# 🔄 ERP TRANSPORT
## Document 13 — Workflows techniques de bout en bout

**Version : 0.1 — Document de travail**  
**Statut : Pré-conception technique**  
**Documents liés :**
- `04_workflows_metier.md`
- `08_architecture_technique.md`
- `11_modele_donnees_entites_metier.md`
- `12_schema_postgresql_rls.md`

---

# 1. 🎯 Objectif

Les documents précédents ont décrit :

- le métier ;
- les entités ;
- la base de données ;
- l'architecture technique.

Ce document fait le lien entre ces éléments.

L'objectif est de montrer **comment une action utilisateur traverse réellement le système**.

Exemple :

```text
👤 Agent
 ↓
🎨 Interface Flet
 ↓
⚙️ Service métier
 ↓
🧠 Validations
 ↓
📦 Repository
 ↓
🗄️ PostgreSQL
 ↓
🔐 RLS / contraintes
 ↓
✅ Résultat
 ↓
🎨 Interface
```

---

# 2. 🧠 Principe général

Une action métier doit suivre une chaîne claire :

```text
USER
 ↓
VIEW
 ↓
SERVICE
 ↓
DOMAIN RULES
 ↓
REPOSITORY
 ↓
DATABASE
```

La réponse remonte ensuite :

```text
DATABASE
 ↓
REPOSITORY
 ↓
SERVICE
 ↓
VIEW
 ↓
USER
```

---

# 3. 🎫 Workflow 1 — Vendre un billet

C'est l'un des workflows critiques du MVP.

## Vue globale

```text
👤 Agent
   ↓
🔎 Recherche voyage
   ↓
🛣️ Sélection voyage
   ↓
💺 Sélection siège
   ↓
👤 Passager
   ↓
💰 Paiement
   ↓
🎫 Billet
```

---

# 4. 🎨 Étape 1 — Interface

L'agent sélectionne :

- voyage ;
- siège ;
- passager ;
- mode de paiement.

Exemple :

```text
Voyage : Yaoundé → Douala
Date : 15/08/2026
Heure : 08:00
Siège : 12
Passager : Jean Dupont
Montant : 8 000 XAF
Paiement : Espèces
```

L'interface transmet une commande au service.

Conceptuellement :

```text
SellTicketCommand
```

---

# 5. ⚙️ Étape 2 — TicketService

Le service reçoit la demande.

Il vérifie notamment :

```text
☑ utilisateur authentifié
☑ permission ticket.sell
☑ voyage existant
☑ voyage ouvert aux ventes
☑ siège valide
☑ passager valide
☑ montant valide
```

---

# 6. 🔐 Étape 3 — Autorisation

Le système détermine :

```text
auth.uid()
    ↓
Profile
    ↓
Company
    ↓
Agency
    ↓
Permissions
```

Puis vérifie :

```text
ticket.sell
```

et le périmètre.

---

# 7. 💺 Étape 4 — Vérification du siège

Le service demande :

```text
TripSeatRepository
```

> Le siège 12 est-il encore disponible pour le voyage X ?

Réponse :

```text
AVAILABLE
```

Mais cette réponse seule ne suffit pas.

Deux agents peuvent avoir demandé le même siège presque simultanément.

La protection finale doit être assurée par la transaction et la base.

---

# 8. 🔒 Étape 5 — Transaction

Conceptuellement :

```text
BEGIN TRANSACTION

1. verrouiller / réserver le siège
2. créer le billet
3. créer le paiement
4. créer le mouvement caisse
5. mettre le siège à SOLD

COMMIT
```

Si une opération critique échoue :

```text
ROLLBACK
```

---

# 9. 🎫 Étape 6 — Création du billet

Le système crée :

```text
Ticket
├── ticket_number
├── trip_id
├── passenger_id
├── seat_number
├── amount
├── currency
└── status = PAID
```

Le numéro du billet doit être unique.

---

# 10. 💰 Étape 7 — Paiement

Le paiement est enregistré :

```text
Payment
├── ticket_id
├── amount
├── method
├── status
└── paid_at
```

Exemple :

```text
8 000 XAF
CASH
PAID
```

---

# 11. 🏦 Étape 8 — Mouvement de caisse

Si le paiement est en espèces :

```text
CashSession
      ↓
CashTransaction
      ↓
SALE
      ↓
+ 8 000 XAF
```

La caisse est ainsi synchronisée avec la vente.

---

# 12. 🧾 Étape 9 — Audit

Une action importante peut produire :

```text
AuditLog

user = Agent 01
action = TICKET_SOLD
entity = Ticket
entity_id = ...
timestamp = ...
```

---

# 13. 🎨 Étape 10 — Retour UI

Le service retourne un résultat.

```text
SUCCESS
ticket_id
ticket_number
```

L'interface affiche :

```text
✅ Billet vendu

N° : YDE-2026-000124
Siège : 12
Montant : 8 000 XAF
```

---

# 14. 🚨 Vente refusée

Exemple :

```text
Agent
 ↓
Vente siège 12
 ↓
PostgreSQL
 ↓
Conflit
 ↓
❌ ROLLBACK
```

L'utilisateur reçoit :

> « Le siège vient d'être vendu par un autre agent. »

L'interface doit actualiser la disponibilité.

---

# 15. 🟡 Workflow 2 — Réserver un siège

```text
👤 Agent
 ↓
🛣️ Voyage
 ↓
💺 Siège
 ↓
🟡 ReservationService
 ↓
🔒 Transaction
 ↓
TripSeat = HELD
 ↓
expires_at
```

---

# 16. ⏱️ Expiration d'une réservation

Une réservation temporaire doit avoir une échéance.

Exemple :

```text
14:00
 ↓
Réservation créée
 ↓
expires_at = 14:15
```

À 14:15 :

```text
ACTIVE
 ↓
EXPIRED
 ↓
TripSeat = AVAILABLE
```

Cette opération peut être réalisée par :

- tâche planifiée ;
- job ;
- fonction backend ;
- vérification lors d'une nouvelle lecture.

La stratégie devra être décidée.

---

# 17. 🎫 Conversion réservation → billet

```text
Reservation
   ↓
Paiement
   ↓
Ticket
   ↓
TripSeat = SOLD
```

La conversion doit être atomique.

Il ne faut pas avoir :

```text
Paiement réussi
mais
pas de billet
```

ou :

```text
Billet créé
mais
paiement absent
```

---

# 18. 💸 Workflow 3 — Annulation d'un billet

```text
🎫 Ticket
 ↓
Vérification
 ↓
Permission
 ↓
Règles d'annulation
 ↓
Transaction
```

Puis :

```text
Ticket = CANCELLED
Payment = REFUND_PENDING / REFUNDED
TripSeat = AVAILABLE
AuditLog
```

---

# 19. 💰 Workflow 4 — Remboursement

Un remboursement doit être relié au paiement d'origine.

```text
Ticket
 ↓
Original Payment
 ↓
Refund
```

Il faut éviter de simplement créer :

```text
Payment = -8000
```

sans référence claire à l'opération originale.

---

# 20. 🏦 Workflow 5 — Ouverture de caisse

```text
👤 Agent
 ↓
🔐 Auth
 ↓
🏦 Open Cash Session
 ↓
Montant initial
 ↓
ACTIVE
```

Le système doit vérifier les règles :

```text
☑ utilisateur autorisé
☑ agence correcte
☑ pas de session incompatible
```

---

# 21. 💰 Workflow 6 — Encaissement

Lors d'une vente en espèces :

```text
Ticket
 ↓
Payment
 ↓
CashTransaction
 ↓
CashSession
```

Exemple :

```text
Vente : +8 000 XAF
```

---

# 22. 🔒 Workflow 7 — Clôture de caisse

À la fin de la journée :

```text
🏦 CashSession
 ↓
Calcul théorique
 ↓
Saisie montant réel
 ↓
Calcul écart
 ↓
Validation
 ↓
CLOSED
```

Formule :

```text
Solde théorique
=
Ouverture
+ Encaissements
- Remboursements
+/- Ajustements
```

Puis :

```text
Écart = Solde réel - Solde théorique
```

---

# 23. 🚨 Écart de caisse

Exemple :

```text
Théorique : 150 000
Réel      : 147 000

Écart     : -3 000
```

Le système doit conserver l'écart.

Il ne doit pas simplement modifier le solde théorique pour le faire disparaître.

---

# 24. 🛣️ Workflow 8 — Création d'un voyage

```text
👤 Responsable
 ↓
📅 Create Trip
 ↓
Sélection ligne
 ↓
Sélection véhicule
 ↓
Sélection chauffeur
 ↓
Validations
 ↓
Trip = PLANNED
```

---

# 25. 🔍 Validations du voyage

Avant création :

```text
☑ Route active
☑ Vehicle actif
☑ Driver actif
☑ Vehicle disponible
☑ Driver disponible
☑ Date cohérente
☑ Heure cohérente
☑ Tarif valide
```

---

# 26. 🚍 Affectation véhicule

Le système vérifie les chevauchements.

Exemple :

```text
BUS-001

08:00 ───── 12:00
           Voyage A

10:00 ───── 14:00
           Voyage B

          ❌ CONFLIT
```

Le service doit refuser B.

---

# 27. 👨‍✈️ Affectation chauffeur

Même principe :

```text
Driver A

08:00 ───── 12:00
           Voyage A

10:00 ───── 14:00
           Voyage B

          ❌ CONFLIT
```

---

# 28. 🛣️ Workflow 9 — Ouverture des ventes

```text
Trip = PLANNED
      ↓
Open Sales
      ↓
Validation
      ↓
Trip = OPEN
```

Une fois ouvert :

```text
🎫 Billetterie
      ↓
autorisé
```

---

# 29. 🛂 Workflow 10 — Embarquement

L'agent ou contrôleur recherche le billet.

```text
🎫 Ticket
 ↓
🔎 Vérification
 ↓
Trip correct ?
 ↓
Ticket valide ?
 ↓
Déjà embarqué ?
 ↓
🛂 BoardingRecord
```

---

# 30. 🔒 Règles d'embarquement

Un billet doit :

```text
☑ exister
☑ appartenir au voyage
☑ être payé / valide
☑ ne pas être annulé
☑ ne pas avoir déjà été embarqué
```

Sinon :

```text
❌ EMBARQUEMENT REFUSÉ
```

---

# 31. 🛫 Workflow 11 — Départ du voyage

```text
Trip = BOARDING
      ↓
Start Trip
      ↓
actual_departure = NOW()
      ↓
Trip = IN_PROGRESS
```

L'heure réelle doit être enregistrée.

---

# 32. 🏁 Workflow 12 — Arrivée

```text
Trip = IN_PROGRESS
      ↓
Declare Arrival
      ↓
actual_arrival = NOW()
      ↓
Trip = ARRIVED
```

Puis éventuellement :

```text
Trip = CLOSED
```

après les opérations de clôture.

---

# 33. 🔧 Workflow 13 — Déclarer une maintenance

```text
👤 Responsable flotte
 ↓
🚌 Véhicule
 ↓
🔧 Maintenance
 ↓
Coût
 ↓
Kilométrage
 ↓
Historique
```

Si la maintenance immobilise le véhicule :

```text
Vehicle
 ↓
IMMOBILIZED
```

Le véhicule ne doit plus apparaître comme disponible pour de nouveaux voyages.

---

# 34. ⛽ Workflow 14 — Enregistrer un plein

```text
🚌 Vehicle
 ↓
⛽ Fuel Record
 ↓
Quantité
 ↓
Prix
 ↓
Kilométrage
 ↓
Enregistrement
```

Le système peut ensuite calculer :

```text
Coût total
Consommation
Coût / km
```

---

# 35. 🧾 Workflow 15 — Audit

Les actions sensibles doivent produire un événement d'audit.

Exemples :

```text
USER_CREATED
ROLE_CHANGED
TRIP_CREATED
TRIP_CANCELLED
TICKET_SOLD
TICKET_CANCELLED
REFUND_CREATED
CASH_CLOSED
VEHICLE_UPDATED
```

---

# 36. 🔄 Exemple complet : journée d'exploitation

Voici un scénario complet.

```text
07:00
 ↓
🏦 Agent ouvre caisse
 ↓
08:00
 ↓
🛣️ Voyage ouvert
 ↓
08:05
 ↓
🎫 Billet vendu
 ↓
08:10
 ↓
🎫 Billet vendu
 ↓
08:30
 ↓
🛂 Embarquement
 ↓
09:00
 ↓
🛫 Départ
 ↓
13:00
 ↓
🏁 Arrivée
 ↓
18:00
 ↓
🏦 Clôture caisse
 ↓
📊 Dashboard
```

Cette chaîne constitue un excellent scénario de démonstration du MVP.

---

# 37. 🧠 Architecture d'un workflow

Pour chaque opération métier, l'équipe doit pouvoir dessiner :

```text
UI
 ↓
Command / DTO
 ↓
Service
 ↓
Domain validation
 ↓
Repository
 ↓
Transaction
 ↓
PostgreSQL
 ↓
Result
```

---

# 38. 📦 DTO / Command

Il est recommandé d'éviter de faire circuler directement les objets UI vers les repositories.

Exemple :

```text
SellTicketCommand

trip_id
passenger_id
vehicle_seat_id
payment_method
amount
```

Le service reçoit cette commande et décide quoi faire.

---

# 39. ⚙️ Service

Exemple conceptuel :

```text
TicketService.sell_ticket(command)
```

Il orchestre :

```text
1. Authorization
2. Trip validation
3. Seat validation
4. Passenger validation
5. Payment validation
6. Transaction
7. Audit
```

---

# 40. 📦 Repository

Le repository fournit les opérations nécessaires.

Exemple :

```text
TripRepository
    get_by_id()

TripSeatRepository
    get_for_update()
    mark_sold()

TicketRepository
    create()

PaymentRepository
    create()

CashRepository
    add_transaction()
```

---

# 41. 🗄️ Transaction DB

Les opérations critiques doivent être atomiques.

Exemple :

```text
BEGIN

lock seat
validate seat
create ticket
create payment
create cash transaction
mark seat SOLD
create audit

COMMIT
```

Si erreur :

```text
ROLLBACK
```

---

# 42. 🚨 Gestion des erreurs

Les erreurs doivent être explicites.

Exemples métier :

```text
TripNotFound
TripNotOpen
SeatUnavailable
PassengerNotFound
UnauthorizedAction
CashSessionNotOpen
TicketAlreadyCancelled
RefundNotAllowed
VehicleUnavailable
DriverUnavailable
```

L'UI transforme ensuite ces erreurs en messages compréhensibles.

---

# 43. 🎨 Exemple UI d'erreur

Erreur technique :

```text
SeatUnavailable
```

Message utilisateur :

> ⚠️ Ce siège vient d'être réservé par un autre agent. Actualisez la disponibilité.

Il faut éviter d'afficher :

```text
IntegrityError duplicate key...
```

---

# 44. 🧪 Tests de workflow

Chaque workflow critique doit avoir au minimum :

### Cas nominal

```text
Tout est correct
→ SUCCESS
```

### Cas métier invalide

```text
Siège occupé
→ REFUS
```

### Cas sécurité

```text
Utilisateur non autorisé
→ REFUS
```

### Cas concurrence

```text
Deux ventes simultanées
→ une seule réussit
```

### Cas erreur technique

```text
DB indisponible
→ ROLLBACK
```

---

# 45. 🔥 Workflow critique n°1

La vente d'un billet doit être considérée comme un **workflow de niveau critique**.

Pourquoi ?

Parce qu'elle touche simultanément :

```text
🎫 Ticket
💺 Seat
💰 Payment
🏦 Cash
🧾 Audit
📊 Reporting
```

Une erreur peut avoir un impact financier direct.

---

# 46. 🔥 Workflow critique n°2

La clôture de caisse.

Elle touche :

```text
💰 Finance
🏦 Cash
🧾 Audit
👤 User
```

Elle doit donc être fortement sécurisée.

---

# 47. 🔥 Workflow critique n°3

L'embarquement.

Il garantit que :

```text
Billet vendu
      ≠
Passager réellement embarqué
```

Cette distinction sera importante pour les statistiques d'exploitation.

---

# 48. 🧠 Workflow métier vs workflow technique

Il faut conserver deux niveaux de documentation.

### Métier

```text
Agent vend un billet
```

### Technique

```text
UI
 ↓
TicketService
 ↓
Transaction
 ↓
PostgreSQL
```

Les deux sont nécessaires.

---

# 49. 📋 Standard de documentation des workflows

Pour chaque futur workflow, utiliser :

```text
# Nom

## Objectif

## Acteur

## Préconditions

## Entrées

## Étapes

## Validations

## Transaction

## Résultat

## Erreurs

## Audit

## Tests
```

Cela deviendra un standard utile pour l'équipe.

---

# 50. 🏁 Conclusion

Les workflows techniques montrent comment les différentes parties du système collaborent.

Le principe central est :

```text
👤 Utilisateur
 ↓
🎨 UI
 ↓
⚙️ Service
 ↓
🧠 Métier
 ↓
📦 Repository
 ↓
🗄️ PostgreSQL
 ↓
🔐 Sécurité + contraintes
 ↓
✅ Résultat
```

Les opérations financières et concurrentes doivent être protégées par des transactions et des contraintes au niveau de la base.

---

## 📌 Prochaine étape

**Document 14 — Standards de développement & règles de code**

Il définira les conventions que les quatre développeurs devront suivre :

- 📁 structure des fichiers ;
- 🐍 conventions Python ;
- 🎨 conventions Flet ;
- 🧠 services / repositories ;
- 🗄️ accès DB ;
- 🔐 sécurité ;
- 🧪 tests ;
- 🌿 Git ;
- 📝 documentation ;
- 🔎 code review ;
- 🚨 gestion des erreurs.

L'objectif sera d'éviter qu'au bout de trois mois, chacun ait développé « son propre style d'ERP » dans le même projet.
