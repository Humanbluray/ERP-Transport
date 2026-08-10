# 🗄️ ERP TRANSPORT
## Document 12 — Schéma PostgreSQL & stratégie RLS

**Version : 0.1 — Document de travail**  
**Statut : Pré-conception technique**  
**Documents liés :**
- `08_architecture_technique.md`
- `11_modele_donnees_entites_metier.md`

---

# 1. 🎯 Objectif

Ce document transforme le modèle conceptuel en une première proposition de **schéma relationnel PostgreSQL**.

Il couvre :

- 🗄️ tables principales ;
- 🔑 clés primaires ;
- 🔗 clés étrangères ;
- 🧱 contraintes ;
- 📊 index ;
- 🔐 stratégie RLS ;
- 🏢 isolation multi-tenant ;
- 🧪 migrations et tests.

> ⚠️ Ce schéma est une base de travail. Il doit être revu par l'équipe avant d'être considéré comme définitif.

---

# 2. 🧭 Principes de conception

Le schéma doit respecter :

1. **Une entreprise = un tenant.**
2. Les données métier sont rattachées à `company_id`.
3. Les relations sont protégées par des FK.
4. Les règles critiques sont renforcées par PostgreSQL.
5. Les recherches fréquentes disposent d'index adaptés.
6. RLS constitue une barrière d'isolation.
7. Les données financières doivent être historisées.
8. Les migrations sont versionnées dans Git.

---

# 3. 🏢 Table `companies`

```sql
companies
---------
id                  uuid PK
name                text NOT NULL
legal_name          text
registration_number text
phone               text
email               text
address             text
status              text NOT NULL
created_at          timestamptz
updated_at          timestamptz
```

## Contraintes possibles

```text
status ∈ {active, inactive}
```

Le `registration_number` peut être unique si cette règle correspond au besoin métier.

---

# 4. 🏪 Table `agencies`

```sql
agencies
--------
id          uuid PK
company_id  uuid FK → companies.id
code        text NOT NULL
name        text NOT NULL
address     text
phone       text
status      text NOT NULL
created_at  timestamptz
updated_at  timestamptz
```

## Contrainte

```text
UNIQUE(company_id, code)
```

Deux entreprises peuvent donc avoir chacune une agence `YADE`, mais une même entreprise ne peut pas avoir deux agences avec le même code.

---

# 5. 👤 Table `profiles`

Supabase Auth conserve l'identité.

Notre table `profiles` conserve les informations applicatives.

```sql
profiles
--------
id            uuid PK
auth_user_id  uuid UNIQUE
company_id    uuid FK → companies.id
agency_id     uuid FK → agencies.id
role_id       uuid FK → roles.id
first_name    text
last_name     text
phone         text
status        text
created_at    timestamptz
updated_at    timestamptz
```

## ⚠️ Point important

Le `auth_user_id` doit permettre de relier :

```text
Supabase Auth
      ↓
Profile
      ↓
Company / Agency
```

---

# 6. 🎭 Tables `roles` et `permissions`

## Roles

```sql
roles
-----
id          uuid PK
company_id  uuid NULL
name        text NOT NULL
code        text NOT NULL
created_at  timestamptz
```

Selon la stratégie choisie, certains rôles peuvent être globaux et d'autres spécifiques à une entreprise.

## Permissions

```sql
permissions
-----------
id          uuid PK
code        text UNIQUE
description text
```

## Relation

```sql
role_permissions
----------------
role_id        uuid FK
permission_id  uuid FK
```

---

# 7. 📚 Table `cities`

```sql
cities
------
id          uuid PK
name        text NOT NULL
country     text NOT NULL
region      text
status      text
created_at  timestamptz
```

Selon le modèle SaaS retenu, les villes peuvent être :

### Option A

Globales à la plateforme.

### Option B

Spécifiques à chaque entreprise.

Pour le MVP, une décision explicite est nécessaire.

---

# 8. 🛣️ Table `routes`

```sql
routes
------
id                  uuid PK
company_id          uuid FK
origin_city_id      uuid FK
destination_city_id uuid FK
code                text
default_duration    interval
status              text
created_at          timestamptz
updated_at          timestamptz
```

## Contraintes

```text
origin_city_id ≠ destination_city_id
UNIQUE(company_id, code)
```

---

# 9. 💰 Table `fares`

```sql
fares
-----
id          uuid PK
company_id  uuid FK
route_id    uuid FK
amount      numeric(12,2) NOT NULL
currency    char(3) NOT NULL
valid_from  timestamptz
valid_until timestamptz
status      text
created_at  timestamptz
```

## Point important

Le billet doit conserver son propre montant.

```text
Fare
 ↓
Ticket.amount
```

Ainsi, si le tarif change demain, les anciens billets restent corrects.

---

# 10. 🚌 Table `vehicles`

```sql
vehicles
--------
id                    uuid PK
company_id            uuid FK
registration_number   text NOT NULL
internal_code         text
brand                 text
model                 text
capacity              integer NOT NULL
status                text NOT NULL
current_mileage       numeric
created_at            timestamptz
updated_at            timestamptz
```

## Contraintes

```text
capacity > 0
current_mileage >= 0
UNIQUE(company_id, registration_number)
```

---

# 11. 💺 Table `vehicle_seats`

```sql
vehicle_seats
-------------
id            uuid PK
vehicle_id    uuid FK
seat_number   text NOT NULL
row_number    integer
column_number integer
seat_type     text
status        text
```

## Contrainte

```text
UNIQUE(vehicle_id, seat_number)
```

Un véhicule ne peut pas avoir deux sièges portant le même numéro.

---

# 12. 👨‍✈️ Table `drivers`

```sql
drivers
-------
id               uuid PK
company_id       uuid FK
first_name       text NOT NULL
last_name        text NOT NULL
phone            text
license_number   text
license_category text
license_expiry   date
status           text
created_at       timestamptz
updated_at       timestamptz
```

## Contrainte

```text
UNIQUE(company_id, license_number)
```

si le numéro de permis est obligatoire et fiable.

---

# 13. 🛣️ Table `trips`

```sql
trips
-----
id                    uuid PK
company_id            uuid FK
agency_id             uuid FK
route_id              uuid FK
vehicle_id            uuid FK
driver_id             uuid FK
fare_id               uuid FK
scheduled_departure   timestamptz NOT NULL
actual_departure      timestamptz
scheduled_arrival     timestamptz
actual_arrival        timestamptz
status                text NOT NULL
created_at            timestamptz
updated_at            timestamptz
```

---

# 14. 🔒 Contraintes sur les voyages

Certaines règles doivent être renforcées.

Exemples :

```text
scheduled_arrival > scheduled_departure
```

Le véhicule et le chauffeur doivent appartenir à la même entreprise que le voyage.

Cette règle peut être contrôlée par le service métier et, lorsque pertinent, renforcée par des contraintes ou fonctions PostgreSQL.

---

# 15. ⚠️ Conflit de véhicule / chauffeur

Un véhicule ne doit pas être affecté simultanément à deux voyages incompatibles.

Même principe pour un chauffeur.

Une simple contrainte `UNIQUE(vehicle_id)` ne suffit pas car le véhicule peut effectuer plusieurs voyages.

Il faut vérifier les **intervalles temporels**.

PostgreSQL offre pour cela des mécanismes adaptés, notamment les contraintes d'exclusion (`EXCLUDE`) avec des plages temporelles.

Exemple conceptuel :

```text
Vehicle BUS-001

08:00 ───────── 12:00  Voyage A
             ❌
10:00 ───────── 14:00  Voyage B
```

Le système doit refuser ce conflit.

> 🧠 Cette partie mérite un prototype SQL avant d'être figée.

---

# 16. 👤 Table `passengers`

```sql
passengers
---------
id          uuid PK
company_id  uuid FK
first_name  text NOT NULL
last_name   text NOT NULL
phone       text
id_type     text
id_number   text
created_at  timestamptz
updated_at  timestamptz
```

Les données personnelles doivent être limitées au strict nécessaire pour le métier.

---

# 17. 🟡 Table `reservations`

```sql
reservations
------------
id            uuid PK
company_id    uuid FK
trip_id       uuid FK
passenger_id  uuid FK
seat_number   text NOT NULL
status        text NOT NULL
expires_at    timestamptz
created_by    uuid FK → profiles.id
created_at    timestamptz
updated_at    timestamptz
```

## États possibles

```text
active
confirmed
expired
cancelled
```

---

# 18. 🎫 Table `tickets`

```sql
tickets
-------
id             uuid PK
company_id     uuid FK
trip_id        uuid FK
passenger_id   uuid FK
reservation_id uuid FK NULL
seat_number    text NOT NULL
ticket_number  text NOT NULL
amount         numeric(12,2) NOT NULL
currency       char(3) NOT NULL
status         text NOT NULL
issued_at      timestamptz
cancelled_at   timestamptz
created_by     uuid FK → profiles.id
created_at     timestamptz
```

## Contraintes

```text
UNIQUE(company_id, ticket_number)
amount >= 0
```

---

# 19. 💺 Gestion de la disponibilité des sièges

La disponibilité d'un siège est une donnée dérivée du contexte :

```text
Vehicle
   ↓
Seat
   ↓
Trip
   ↓
Reservation / Ticket
```

Il est donc déconseillé de stocker naïvement :

```text
vehicle_seats.status = SOLD
```

car un même siège peut être :

```text
BUS-001
Seat 12

Voyage A → vendu
Voyage B → disponible
Voyage C → réservé
```

Le statut doit être déterminé dans le contexte du voyage.

---

# 20. 🔒 Double vente : stratégie

La protection doit être pensée au niveau PostgreSQL.

Une approche consiste à réserver une ressource de siège dans une table de disponibilité par voyage.

Exemple :

```sql
trip_seats
----------
id
trip_id
vehicle_seat_id
status
```

Puis :

```text
UNIQUE(trip_id, vehicle_seat_id)
```

La vente ou réservation modifie l'état de cette ligne dans une transaction.

Cette approche peut simplifier fortement la gestion des sièges.

---

# 21. 🎯 Proposition `trip_seats`

```sql
trip_seats
----------
id
trip_id
vehicle_seat_id
status
ticket_id
reservation_id
created_at
updated_at
```

États possibles :

```text
AVAILABLE
HELD
SOLD
BOARDED
BLOCKED
```

## Avantage

La disponibilité devient directement interrogeable :

```text
Trip
 ↓
TripSeats
 ↓
AVAILABLE
```

> 💡 Cette table est une proposition importante à discuter avec l'équipe. Elle peut rendre la billetterie plus robuste et plus simple à raisonner.

---

# 22. 💰 Table `payments`

```sql
payments
--------
id
company_id
ticket_id
amount
currency
method
status
reference
paid_at
created_by
created_at
```

## Modes

```text
CASH
MOBILE_MONEY
CARD
OTHER
```

Le système pourra commencer avec les méthodes réellement nécessaires au pilote.

---

# 23. 🏦 Table `cash_sessions`

```sql
cash_sessions
-------------
id
company_id
agency_id
user_id
opening_amount
expected_amount
closing_amount
variance
status
opened_at
closed_at
```

## États

```text
OPEN
ACTIVE
CLOSED
```

Une contrainte métier peut empêcher un utilisateur d'avoir plusieurs sessions actives simultanément, selon le modèle de caisse retenu.

---

# 24. 💸 Table `cash_transactions`

```sql
cash_transactions
-----------------
id
cash_session_id
type
amount
reference_type
reference_id
created_by
created_at
```

Exemples :

```text
SALE
REFUND
DEPOSIT
WITHDRAWAL
ADJUSTMENT
```

---

# 25. 🛂 Table `boarding_records`

```sql
boarding_records
----------------
id
company_id
trip_id
ticket_id
boarded_at
boarded_by
method
```

## Contrainte

Un billet ne doit pas être embarqué deux fois.

```text
UNIQUE(ticket_id)
```

---

# 26. 🔧 Table `maintenance`

```sql
maintenance
-----------
id
company_id
vehicle_id
type
description
mileage
cost
started_at
completed_at
status
created_at
```

---

# 27. ⛽ Table `fuel_records`

```sql
fuel_records
------------
id
company_id
vehicle_id
trip_id
quantity
unit_price
total_amount
mileage
station
fueled_at
created_by
```

Les données pourront ensuite alimenter des indicateurs de consommation.

---

# 28. ⚠️ Table `incidents`

```sql
incidents
---------
id
company_id
trip_id
vehicle_id
driver_id
type
severity
description
occurred_at
resolved_at
created_by
```

Toutes les FK liées au contexte de l'incident peuvent être nullable selon la nature de l'événement.

---

# 29. 🧾 Table `audit_logs`

```sql
audit_logs
----------
id
company_id
user_id
action
entity_type
entity_id
old_data
new_data
created_at
```

Pour des raisons de sécurité et de confidentialité, les données sensibles ne doivent pas être journalisées sans nécessité.

---

# 30. 🏢 Multi-tenant

Le principe central :

```text
company_id
    ↓
Tenant boundary
```

Exemple :

```text
Company A
 ├── Vehicles A
 ├── Trips A
 └── Tickets A

Company B
 ├── Vehicles B
 ├── Trips B
 └── Tickets B
```

Un utilisateur de A ne doit jamais pouvoir accéder aux données de B.

---

# 31. 🔐 RLS — Principe général

Avec Supabase, les tables exposées à l'application doivent avoir une politique RLS adaptée.

Conceptuellement :

```text
auth.uid()
    ↓
profiles
    ↓
company_id
    ↓
row.company_id
```

La règle fondamentale est :

> L'utilisateur ne peut accéder qu'aux lignes appartenant à son périmètre autorisé.

---

# 32. 🛡️ Fonction SQL de récupération du tenant

Une fonction sécurisée peut centraliser la récupération de l'entreprise courante.

Conceptuellement :

```sql
get_user_company_id()
```

Elle retourne le `company_id` associé à `auth.uid()`.

Cela évite de recopier une logique complexe dans toutes les policies.

> ⚠️ La fonction exacte devra être conçue et testée avec les mécanismes de sécurité PostgreSQL/Supabase appropriés.

---

# 33. 🔐 Exemple de policy RLS

Conceptuellement :

```sql
create policy "company isolation"
on vehicles
for select
using (
    company_id = get_user_company_id()
);
```

Même principe pour :

- trips ;
- tickets ;
- passengers ;
- payments ;
- vehicles ;
- drivers ;
- maintenance.

---

# 34. 🏪 Isolation par agence

Certaines données peuvent nécessiter un contrôle supplémentaire.

Exemple :

```text
User
 ↓
Company
 ↓
Agency
 ↓
Resource
```

Un responsable d'agence peut être limité à :

```text
agency_id = current_user_agency_id
```

Alors qu'un administrateur entreprise peut accéder à toutes les agences.

---

# 35. 🎭 RLS + RBAC

Il faut distinguer :

### RBAC

> Que peut faire l'utilisateur ?

### RLS

> Quelles lignes peut-il voir ou modifier ?

Exemple :

```text
Agent
  ↓
permission: ticket.sell
  +
company_id = A
  +
agency_id = YDE
```

Cela permet un contrôle beaucoup plus précis.

---

# 36. 🔒 INSERT / UPDATE / DELETE

Une policy RLS doit être pensée pour chaque opération :

```text
SELECT
INSERT
UPDATE
DELETE
```

Il ne suffit pas de sécuriser uniquement `SELECT`.

Exemple :

Un utilisateur ne doit pas pouvoir créer :

```text
ticket.company_id = Company B
```

même s'il appartient à Company A.

---

# 37. 🧠 Ne jamais faire confiance au `company_id` envoyé par l'UI

Une interface peut envoyer :

```json
{
  "company_id": "..."
}
```

Mais ce champ ne doit pas être considéré comme une preuve d'autorisation.

La sécurité doit dériver le tenant depuis :

```text
auth.uid()
     ↓
profile
     ↓
company_id
```

---

# 38. 🔐 Règles de sécurité sensibles

Les opérations suivantes doivent être particulièrement protégées :

- changement de rôle ;
- remboursement ;
- clôture caisse ;
- modification d'un billet ;
- annulation voyage ;
- modification tarif ;
- suppression de données ;
- accès aux rapports financiers.

Certaines opérations peuvent être réservées à des rôles spécifiques.

---

# 39. 🧱 Suppression des données

Éviter les suppressions physiques sur les objets métier importants.

Exemple :

```text
Ticket
❌ DELETE
```

Préférer :

```text
status = CANCELLED
```

Cela permet de conserver l'historique.

Même principe pour :

- voyages ;
- paiements ;
- caisses ;
- remboursements ;
- embarquements.

---

# 40. 🧾 Migrations

Toutes les modifications du schéma doivent être versionnées.

Exemple :

```text
migrations/
├── 001_initial_schema.sql
├── 002_add_roles.sql
├── 003_add_trips.sql
├── 004_add_tickets.sql
├── 005_add_cash.sql
└── ...
```

Règle :

> ❌ Ne pas modifier manuellement la production sans migration versionnée.

---

# 41. 🧪 Tests RLS

Les policies doivent avoir leurs propres tests.

Exemple :

```text
Utilisateur Company A
        ↓
SELECT vehicles
        ↓
Résultat = véhicules A
```

Puis :

```text
Utilisateur Company A
        ↓
SELECT vehicles WHERE company_id = B
        ↓
Résultat = aucun accès
```

Même chose pour les écritures.

---

# 42. 🧪 Tests d'intégrité

Tests indispensables :

### Test 1

Créer un billet avec un voyage inexistant.

```text
→ ❌
```

### Test 2

Créer deux sièges identiques dans le même véhicule.

```text
→ ❌
```

### Test 3

Créer deux tickets pour le même siège et voyage.

```text
→ ❌
```

### Test 4

Utilisateur A lit données B.

```text
→ ❌
```

### Test 5

Utilisateur sans permission tente un remboursement.

```text
→ ❌
```

---

# 43. 📊 Index initiaux

Les index ne doivent pas être ajoutés au hasard.

Quelques candidats :

```text
profiles(auth_user_id)

agencies(company_id)

vehicles(company_id, status)

drivers(company_id, status)

routes(company_id, status)

trips(company_id, scheduled_departure)

trips(company_id, status)

tickets(company_id, trip_id)

tickets(company_id, ticket_number)

passengers(company_id, phone)

cash_sessions(company_id, agency_id, status)

audit_logs(company_id, created_at)
```

La stratégie devra être ajustée à partir des requêtes réelles.

---

# 44. 💡 Attention à la sur-indexation

Un index accélère certaines lectures mais augmente :

- l'espace disque ;
- le coût des écritures ;
- le temps des opérations de maintenance.

Donc :

> **Indexer les requêtes importantes, pas toutes les colonnes.**

---

# 45. 💰 Montants financiers

Pour les montants, PostgreSQL doit utiliser un type adapté aux valeurs monétaires.

Une option :

```text
numeric(12,2)
```

ou une représentation entière dans l'unité monétaire minimale selon les choix du système.

Pour le FCFA, il faudra définir clairement la convention.

---

# 46. 🌍 Devise

Le MVP peut initialement utiliser :

```text
XAF
```

mais conserver une colonne :

```text
currency
```

permettra une évolution future.

Exemple :

```text
amount = 25000
currency = XAF
```

---

# 47. 🕐 Dates

Les dates métier doivent utiliser des timestamps cohérents.

Préférer une stratégie uniforme avec `timestamptz` côté PostgreSQL.

Exemples :

```text
created_at
scheduled_departure
actual_departure
paid_at
boarded_at
closed_at
```

L'affichage sera ensuite converti dans le fuseau local de l'utilisateur.

---

# 48. 🧠 Schéma MVP simplifié

```text
COMPANIES
   │
   ├── AGENCIES
   ├── PROFILES
   ├── VEHICLES
   │      └── VEHICLE_SEATS
   ├── DRIVERS
   ├── ROUTES
   │      └── TRIPS
   │             ├── TRIP_SEATS
   │             ├── RESERVATIONS
   │             ├── TICKETS
   │             │      └── PAYMENTS
   │             └── BOARDING_RECORDS
   │
   └── CASH_SESSIONS
          └── CASH_TRANSACTIONS
```

Autour de ce noyau :

```text
MAINTENANCE
FUEL_RECORDS
INCIDENTS
AUDIT_LOGS
```

---

# 49. 🚨 Points à prototyper avant validation

Trois sujets méritent un prototype technique avant de figer l'architecture :

## 1️⃣ Double vente concurrente

Tester deux transactions simultanées sur :

```text
Trip X
Seat 12
```

## 2️⃣ RLS multi-tenant

Tester :

```text
Company A
vs
Company B
```

## 3️⃣ Conflits de planning

Tester deux voyages chevauchants pour :

```text
Vehicle X
Driver Y
```

Si ces trois mécanismes sont solides, une grande partie du risque technique du cœur du MVP est réduite.

---

# 50. 🏁 Conclusion

Le schéma proposé donne une première base relationnelle cohérente pour le MVP.

Les principes essentiels sont :

```text
🏢 Company
    ↓
🔐 Tenant isolation
    ↓
📦 Domain data
    ↓
🔗 Foreign keys
    ↓
🧱 Database constraints
    ↓
🛡️ RLS
```

Et pour les opérations critiques :

```text
Application
     ↓
Service
     ↓
Transaction
     ↓
PostgreSQL constraint
     ↓
Data integrity
```

Le système ne doit pas dépendre uniquement de Python pour garantir l'intégrité.

---

## 📌 Prochaine étape

**Document 13 — Workflows techniques de bout en bout**

Nous allons prendre les processus les plus importants du MVP et les décrire techniquement :

- 🎫 vendre un billet ;
- 💰 encaisser ;
- 🟡 réserver ;
- 🛂 embarquer ;
- 🛣️ démarrer un voyage ;
- 🏁 clôturer un voyage ;
- 💸 rembourser ;
- 🏦 clôturer une caisse.

Pour chaque workflow, nous détaillerons :

```text
Utilisateur
 ↓
UI
 ↓
Service
 ↓
Validation
 ↓
Repository
 ↓
Transaction DB
 ↓
RLS / contraintes
 ↓
Résultat
```

Ce sera la passerelle entre **la base de données** et **le code que les développeurs vont réellement écrire**.
