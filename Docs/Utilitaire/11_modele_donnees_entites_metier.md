# 🗄️ ERP TRANSPORT
## Document 11 — Modèle de données & entités métier

**Version : 0.1 — Document de travail**  
**Statut : Préparation de la conception de la base de données**  
**Documents liés :**
- `01_vision_cadrage.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`
- `05_regles_metier.md`
- `06_definition_mvp.md`
- `07_architecture_fonctionnelle.md`
- `08_architecture_technique.md`
- `10_backlog_initial_sprint_0.md`

---

# 1. 🎯 Objectif du document

Ce document identifie les principales **entités métier** du système et leurs relations.

Il ne s'agit pas encore de produire le schéma SQL définitif.

L'objectif est d'abord de répondre à :

- Quels sont les objets importants du métier ?
- À quoi servent-ils ?
- Qui dépend de qui ?
- Quelles relations existent ?
- Quelles informations doivent être historisées ?
- Quelles contraintes doivent être garanties par la base ?

> 💡 **Principe :** on ne commence pas par créer des tables. On commence par comprendre le métier que les tables doivent représenter.

---

# 2. 🧠 Vue globale du modèle

Une première représentation peut être :

```text
🏢 COMPANY
   │
   ├── 🏪 AGENCY
   │      │
   │      ├── 👤 USERS / PROFILES
   │      └── 🏦 CASH SESSIONS
   │
   ├── 🚌 VEHICLES
   │      │
   │      ├── 💺 SEAT CONFIGURATION
   │      ├── 🔧 MAINTENANCE
   │      └── ⛽ FUEL
   │
   ├── 👨‍✈️ DRIVERS
   │
   ├── 📚 ROUTES
   │      │
   │      └── 🛣️ TRIPS
   │              │
   │              ├── 🎫 TICKETS
   │              │      ├── 👤 PASSENGER
   │              │      └── 💰 PAYMENT
   │              │
   │              └── 💺 SEATS
   │
   └── 🧾 AUDIT LOGS
```

Cette vue sera progressivement affinée.

---

# 3. 🏢 Entity — Company

## 🎯 Responsabilité

`Company` représente l'entreprise cliente du SaaS.

Exemple :

> Une société de transport exploitant plusieurs agences.

## Informations possibles

```text
Company
├── id
├── name
├── legal_name
├── registration_number
├── phone
├── email
├── address
├── status
├── created_at
└── updated_at
```

## Relations

```text
Company
├── Agencies
├── Users
├── Vehicles
├── Drivers
├── Routes
├── Trips
├── Tickets
├── Payments
└── AuditLogs
```

## Règle

Toutes les données métier d'une entreprise doivent être isolées des autres entreprises.

---

# 4. 🏪 Entity — Agency

## 🎯 Responsabilité

Une agence représente un point d'exploitation de l'entreprise.

Exemples :

- agence Yaoundé ;
- agence Douala ;
- agence Bafoussam.

## Informations

```text
Agency
├── id
├── company_id
├── name
├── code
├── address
├── phone
├── status
├── created_at
└── updated_at
```

## Relation

```text
Company
   │
   └── 1 ─── N ─── Agency
```

Une agence appartient à une seule entreprise.

---

# 5. 👤 Entity — User / Profile

Il est utile de distinguer :

### Authentification

Gérée par le système d'identité, par exemple Supabase Auth.

### Profil applicatif

Contient les informations métier de l'utilisateur.

```text
Profile
├── id
├── auth_user_id
├── company_id
├── agency_id
├── role_id
├── first_name
├── last_name
├── phone
├── status
├── created_at
└── updated_at
```

## Relation

```text
Auth User
    ↓
Profile
    ↓
Company
    ↓
Agency
```

---

# 6. 🎭 Entity — Role

## 🎯 Responsabilité

Définir le rôle fonctionnel d'un utilisateur.

Exemples :

```text
SUPER_ADMIN
COMPANY_ADMIN
OPERATIONS_MANAGER
AGENCY_MANAGER
TICKET_AGENT
FLEET_MANAGER
CONTROLLER
DRIVER
ACCOUNTANT
```

La liste définitive sera validée selon le modèle de permissions retenu.

---

# 7. 🛡️ Entity — Permission

Les permissions représentent les actions autorisées.

Exemples :

```text
company.read
agency.manage
vehicle.manage
trip.create
trip.start
ticket.sell
ticket.cancel
payment.refund
cash.close
report.view
```

Une approche RBAC peut être utilisée :

```text
User
 ↓
Role
 ↓
Permissions
```

Pour les besoins plus fins, un périmètre pourra être ajouté :

```text
Permission
   +
Scope
   ↓
Autorisation
```

---

# 8. 📚 Entity — City

## 🎯 Responsabilité

Représenter une ville ou destination utilisable dans les lignes.

```text
City
├── id
├── name
├── country
├── region
├── status
└── created_at
```

Pour le MVP, le modèle peut rester simple.

---

# 9. 🛣️ Entity — Route

## 🎯 Responsabilité

Une `Route` représente une liaison commerciale entre deux points.

Exemple :

> Yaoundé → Douala

```text
Route
├── id
├── company_id
├── origin_city_id
├── destination_city_id
├── code
├── default_duration
├── status
├── created_at
└── updated_at
```

## Relation

```text
City
  ↓
Route
  ↓
Trip
```

---

# 10. 💰 Entity — Fare / Tariff

## 🎯 Responsabilité

Définir le prix applicable à une ligne ou à un contexte donné.

```text
Fare
├── id
├── company_id
├── route_id
├── amount
├── currency
├── valid_from
├── valid_until
├── status
└── created_at
```

## ⚠️ Point à décider

Le tarif peut évoluer.

Il faut donc éviter de dépendre uniquement du tarif « actuel ».

Un billet doit conserver **le montant réellement vendu**.

---

# 11. 🚌 Entity — Vehicle

## 🎯 Responsabilité

Représenter un véhicule exploité par l'entreprise.

```text
Vehicle
├── id
├── company_id
├── registration_number
├── internal_code
├── brand
├── model
├── capacity
├── status
├── current_mileage
├── created_at
└── updated_at
```

## États possibles

```text
🟢 AVAILABLE
🟡 ASSIGNED
🔵 IN_TRIP
🔴 IMMOBILIZED
⚫ INACTIVE
```

La liste exacte devra être définie.

---

# 12. 💺 Entity — Seat Configuration

Un point important :

> Le siège ne doit pas être pensé uniquement comme une donnée de voyage.

Le véhicule possède une **configuration physique**.

Exemple :

```text
Vehicle
   ↓
Seat Configuration
   ├── Seat 01
   ├── Seat 02
   ├── Seat 03
   └── ...
```

## Informations possibles

```text
Seat
├── id
├── vehicle_id
├── seat_number
├── row_number
├── column_number
├── seat_type
└── status
```

## Exemple

```text
[01] [02]

[03] [04]

[05] [06]
```

La représentation graphique pourra être adaptée selon le véhicule.

---

# 13. 🧠 Pourquoi séparer véhicule et voyage ?

Un véhicule peut effectuer plusieurs voyages dans le temps.

```text
BUS-023
   │
   ├── Voyage V001
   ├── Voyage V002
   ├── Voyage V003
   └── Voyage V004
```

Mais la disponibilité du siège dépend du voyage.

Donc :

```text
Vehicle
   ↓
Seat configuration

Trip
   ↓
Seat availability
```

Cette distinction est essentielle pour éviter une mauvaise modélisation.

---

# 14. 👨‍✈️ Entity — Driver

```text
Driver
├── id
├── company_id
├── first_name
├── last_name
├── phone
├── license_number
├── license_category
├── license_expiry
├── status
├── created_at
└── updated_at
```

Un chauffeur appartient à une entreprise.

---

# 15. 📅 Entity — Trip

Le `Trip` est l'une des entités centrales.

## Responsabilité

Représenter un départ programmé ou réalisé.

```text
Trip
├── id
├── company_id
├── agency_id
├── route_id
├── vehicle_id
├── driver_id
├── fare_id
├── scheduled_departure
├── actual_departure
├── scheduled_arrival
├── actual_arrival
├── status
├── created_at
└── updated_at
```

## Relations

```text
Route
  ↓
Trip
  ├── Vehicle
  ├── Driver
  ├── Tickets
  └── Boarding records
```

---

# 16. 🔄 Trip Status

Le voyage doit avoir un cycle de vie contrôlé.

Proposition :

```text
DRAFT
  ↓
PLANNED
  ↓
OPEN
  ↓
BOARDING
  ↓
IN_PROGRESS
  ↓
ARRIVED
  ↓
CLOSED
```

Alternative :

```text
CANCELLED
```

peut intervenir à certains moments.

Les transitions autorisées devront être définies explicitement.

---

# 17. 👤 Entity — Passenger

Le passager représente la personne transportée.

```text
Passenger
├── id
├── company_id
├── first_name
├── last_name
├── phone
├── id_type
├── id_number
├── created_at
└── updated_at
```

## ⚠️ Question importante

Il faudra décider si un même passager doit être réutilisable entre plusieurs voyages.

Une approche possible :

```text
Passenger
   ↓
Ticket
   ↓
Trip
```

Cela permet de conserver un historique.

---

# 18. 🟡 Entity — Reservation

La réservation représente une intention de voyage avant confirmation définitive.

```text
Reservation
├── id
├── company_id
├── trip_id
├── passenger_id
├── seat_id / seat_number
├── status
├── expires_at
├── created_by
├── created_at
└── updated_at
```

## États possibles

```text
🟡 ACTIVE
🟢 CONFIRMED
🔴 EXPIRED
⚫ CANCELLED
```

La réservation ne doit pas être confondue avec le billet.

---

# 19. 🎫 Entity — Ticket

Le billet représente le droit au transport résultant de la vente.

```text
Ticket
├── id
├── company_id
├── trip_id
├── passenger_id
├── seat_number
├── reservation_id
├── ticket_number
├── amount
├── currency
├── status
├── issued_at
├── cancelled_at
└── created_by
```

## États possibles

```text
🟢 PAID
🔵 BOARDED
🔴 CANCELLED
```

D'autres états pourront être ajoutés.

---

# 20. 🔒 Contrainte critique du billet

Pour un voyage donné, un siège ne doit pas être vendu deux fois.

Conceptuellement :

```text
UNIQUE(
    trip_id,
    seat_number
)
```

Mais attention :

> Cette contrainte doit tenir compte des réservations temporaires, des annulations et de la stratégie de réservation retenue.

Elle devra donc être conçue avec soin au niveau PostgreSQL.

---

# 21. 💰 Entity — Payment

Le paiement représente le règlement financier d'une opération.

```text
Payment
├── id
├── company_id
├── ticket_id
├── amount
├── currency
├── method
├── status
├── reference
├── paid_at
└── created_by
```

## Modes possibles

```text
CASH
MOBILE_MONEY
CARD
OTHER
```

Le MVP peut commencer avec un nombre limité de modes.

---

# 22. 🏦 Entity — Cash Session

Une session de caisse représente une période pendant laquelle un agent ou une caisse enregistre des opérations.

```text
CashSession
├── id
├── company_id
├── agency_id
├── user_id
├── opening_amount
├── closing_amount
├── expected_amount
├── variance
├── status
├── opened_at
└── closed_at
```

## Cycle

```text
OPEN
 ↓
ACTIVE
 ↓
CLOSED
```

---

# 23. 💸 Entity — Cash Transaction

Pour éviter de mélanger la session de caisse et les mouvements financiers, il peut être utile de distinguer :

```text
CashSession
      ↓
CashTransaction
```

Exemple :

```text
CashTransaction
├── id
├── cash_session_id
├── type
├── amount
├── reference_type
├── reference_id
├── created_at
└── created_by
```

Types possibles :

```text
SALE
REFUND
DEPOSIT
WITHDRAWAL
ADJUSTMENT
```

---

# 24. 🔧 Entity — Maintenance

```text
Maintenance
├── id
├── company_id
├── vehicle_id
├── type
├── description
├── mileage
├── started_at
├── completed_at
├── cost
├── status
└── created_at
```

Elle doit alimenter l'historique du véhicule.

---

# 25. ⛽ Entity — Fuel Record

```text
FuelRecord
├── id
├── company_id
├── vehicle_id
├── trip_id
├── quantity
├── unit_price
├── total_amount
├── mileage
├── station
├── fueled_at
└── created_by
```

Le lien avec `Trip` peut être optionnel selon le contexte.

---

# 26. 🛂 Entity — Boarding Record

L'embarquement mérite une trace distincte du billet.

```text
BoardingRecord
├── id
├── company_id
├── trip_id
├── ticket_id
├── boarded_at
├── boarded_by
└── method
```

Cela permet de répondre à :

> Le billet a-t-il réellement été utilisé pour embarquer ?

---

# 27. ⚠️ Entity — Incident

```text
Incident
├── id
├── company_id
├── trip_id
├── vehicle_id
├── driver_id
├── type
├── severity
├── description
├── occurred_at
├── resolved_at
└── created_by
```

Les relations peuvent être optionnelles selon le type d'incident.

---

# 28. 🧾 Entity — Audit Log

```text
AuditLog
├── id
├── company_id
├── user_id
├── action
├── entity_type
├── entity_id
├── old_data
├── new_data
├── ip_address
└── created_at
```

Le contenu exact dépendra de la stratégie de sécurité et de confidentialité.

---

# 29. 🔗 Vue relationnelle simplifiée

```text
COMPANY
 │
 ├──< AGENCY
 │      │
 │      └──< CASH_SESSION
 │
 ├──< USER
 │
 ├──< VEHICLE
 │      ├──< SEAT
 │      ├──< MAINTENANCE
 │      └──< FUEL_RECORD
 │
 ├──< DRIVER
 │
 ├──< ROUTE
 │      │
 │      └──< TRIP
 │              │
 │              ├──< RESERVATION
 │              │
 │              ├──< TICKET
 │              │      │
 │              │      └──< PAYMENT
 │              │
 │              └──< BOARDING_RECORD
 │
 └──< AUDIT_LOG
```

---

# 30. 🧠 Relations cardinales importantes

| Relation | Cardinalité |
|---|---|
| Company → Agency | 1 → N |
| Company → User | 1 → N |
| Company → Vehicle | 1 → N |
| Company → Driver | 1 → N |
| Company → Route | 1 → N |
| Route → Trip | 1 → N |
| Vehicle → Seat | 1 → N |
| Vehicle → Trip | 1 → N |
| Driver → Trip | 1 → N |
| Trip → Ticket | 1 → N |
| Passenger → Ticket | 1 → N |
| Trip → Reservation | 1 → N |
| Ticket → Payment | 1 → N possible |
| Trip → BoardingRecord | 1 → N |
| Vehicle → Maintenance | 1 → N |
| Vehicle → FuelRecord | 1 → N |
| CashSession → CashTransaction | 1 → N |

Les cardinalités définitives devront être validées.

---

# 31. 🧠 Point important : ne pas tout mettre dans `Trip`

Une mauvaise modélisation pourrait donner :

```text
Trip
├── passenger_1
├── passenger_2
├── passenger_3
├── payment_1
├── payment_2
├── fuel
├── maintenance
└── ...
```

❌ Cela deviendrait rapidement impossible à maintenir.

Une meilleure approche :

```text
Trip
 ├── Tickets
 ├── Reservations
 ├── BoardingRecords
 └── ...
```

Chaque domaine conserve sa propre responsabilité.

---

# 32. 🧠 Point important : historique vs état actuel

Certaines données doivent être distinguées entre :

### État actuel

Exemple :

```text
Vehicle.status = AVAILABLE
```

### Historique

Exemple :

```text
VehicleStatusHistory
```

Cette distinction pourra être nécessaire pour :

- affectations ;
- changements de statut ;
- maintenance ;
- remplacements ;
- prix ;
- permissions.

---

# 33. 🕐 Dates et heures

Les événements métier doivent conserver des timestamps fiables.

Exemples :

```text
created_at
updated_at
scheduled_departure
actual_departure
scheduled_arrival
actual_arrival
paid_at
boarded_at
closed_at
```

Il faudra décider une stratégie cohérente de gestion des fuseaux horaires.

Pour un produit utilisé au Cameroun, la référence métier pourra être l'heure locale du Cameroun, tout en conservant une représentation technique cohérente côté serveur.

---

# 34. 💰 Argent et devise

Les montants financiers doivent éviter les calculs approximatifs de type flottant.

Pour le modèle financier, privilégier une représentation adaptée aux montants monétaires.

Exemple :

```text
amount = 25000
currency = XAF
```

La stratégie exacte sera définie lors du schéma PostgreSQL.

---

# 35. 🔐 Multi-tenant dans le modèle

Une question centrale :

> Où placer `company_id` ?

Pour les principales entités métier, une stratégie explicite est recommandée.

Exemple :

```text
vehicles.company_id
drivers.company_id
routes.company_id
trips.company_id
tickets.company_id
payments.company_id
```

Cela facilite :

- RLS ;
- audit ;
- filtrage ;
- isolation ;
- requêtes.

Mais les relations doivent rester cohérentes.

---

# 36. 🛡️ Intégrité référentielle

PostgreSQL doit protéger autant que possible les relations.

Exemples :

```text
ticket.trip_id
      ↓
trip.id
```

Si le voyage n'existe pas :

> ❌ impossible de créer le billet.

Même principe pour :

- véhicule ;
- chauffeur ;
- entreprise ;
- agence ;
- passager ;
- paiement.

---

# 37. 🔒 Contraintes `UNIQUE`

Certaines données doivent être uniques.

Exemples possibles :

```text
company.code
agency.code + company_id
vehicle.registration_number + company_id
driver.license_number + company_id
ticket.ticket_number + company_id
```

Les règles exactes doivent être décidées.

---

# 38. ⚙️ Indexation

Les recherches fréquentes devront être indexées.

Exemples :

```text
trips(company_id, scheduled_departure)

tickets(company_id, trip_id)

tickets(company_id, ticket_number)

vehicles(company_id, status)

drivers(company_id, status)

cash_sessions(company_id, agency_id, status)
```

La stratégie finale dépendra des requêtes réellement observées.

---

# 39. 🧪 Données et tests

La base devra disposer de jeux de données de test.

Exemple :

```text
🏢 Demo Transport
   ├── 🏪 Yaoundé
   ├── 🏪 Douala
   │
   ├── 🚌 BUS-001
   ├── 🚌 BUS-002
   │
   ├── 👨‍✈️ Chauffeur A
   ├── 👨‍✈️ Chauffeur B
   │
   └── 🛣️ Yaoundé → Douala
```

Cela permettra aux développeurs de tester les workflows sans dépendre de données réelles.

---

# 40. 🚨 Questions de modélisation à trancher

Avant de figer le schéma SQL, l'équipe devra décider notamment :

### Voyage

- Un voyage peut-il être reprogrammé ?
- Peut-il changer de ligne ?
- Peut-il changer de véhicule après vente ?
- Peut-il changer de chauffeur après départ ?

### Billetterie

- Une réservation bloque-t-elle définitivement le siège ?
- Combien de temps ?
- Un siège peut-il être changé après paiement ?
- Une personne peut-elle avoir plusieurs billets sur le même voyage ?

### Passager

- Le passager est-il global à l'entreprise ?
- Peut-il être supprimé ?
- Quelles données personnelles sont obligatoires ?

### Finance

- Un billet peut-il avoir plusieurs paiements ?
- Peut-on faire un paiement partiel ?
- Une caisse appartient-elle à un utilisateur ou à une agence ?
- Comment gérer les versements en banque ?

### Véhicules

- Un véhicule peut-il changer d'agence ?
- Les sièges sont-ils toujours fixes ?
- Comment gérer un véhicule avec une configuration différente ?

Ces questions doivent être résolues **avant la création définitive du schéma**.

---

# 41. 🎯 Modèle MVP recommandé

Pour éviter de créer trop d'entités dès le départ, le MVP peut commencer avec :

```text
🏢 Company
🏪 Agency
👤 Profile
🎭 Role
📚 City
🛣️ Route
💰 Fare
🚌 Vehicle
💺 Seat
👨‍✈️ Driver
📅 Trip
👤 Passenger
🟡 Reservation
🎫 Ticket
💰 Payment
🏦 CashSession
💸 CashTransaction
🛂 BoardingRecord
🧾 AuditLog
```

Puis ajouter :

```text
🔧 Maintenance
⛽ FuelRecord
⚠️ Incident
📦 Parcel
```

selon les priorités finales du MVP.

---

# 42. 🧭 Passage vers le schéma SQL

Une fois les entités validées, le processus sera :

```text
🧠 Entités métier
      ↓
🔗 Relations
      ↓
📐 Contraintes
      ↓
🗄️ Tables PostgreSQL
      ↓
🔐 RLS
      ↓
📊 Index
      ↓
🧪 Tests DB
```

Il ne faut pas inverser ce processus.

---

# 43. 🏁 Conclusion

Le modèle de données constitue le squelette du système.

Une bonne base doit refléter le métier sans enfermer prématurément l'application dans des choix techniques.

Le principe central est :

> **Chaque entité doit avoir une responsabilité claire, chaque relation doit avoir une raison métier et chaque contrainte critique doit être protégée au niveau approprié.**

La prochaine étape sera de transformer ce modèle conceptuel en **schéma relationnel PostgreSQL**.

---

## 📌 Prochaine étape

**Document 12 — Schéma PostgreSQL & stratégie RLS**

Nous pourrons alors définir :

- 🗄️ les tables ;
- 🔑 les clés primaires ;
- 🔗 les clés étrangères ;
- 🧱 les contraintes ;
- 📊 les index ;
- 🔐 les politiques RLS ;
- 🧾 les migrations ;
- 🧪 les données de test ;
- 🛡️ les règles d'isolation multi-tenant.

Ce sera le premier document véritablement proche de l'implémentation technique.
