# ERP Transport --- Workflows métier

> Référence consolidée des workflows principaux.
> Administration/gouvernance traitée séparément.

## Voyage

``` text
VOYAGE
├── NORMAL
│   ├── PROGRAMMÉ
│   └── NON PROGRAMMÉ
└── NAVETTE
    └── COLIS UNIQUEMENT
```

Cycle normal :

``` text
PLANIFIÉ → VENTE OUVERTE → PLEIN → EN ROUTE → ARRIVÉ
```

-   PLANIFIÉ : réservations possibles, pas de vente directe.
-   Affectation du bus : ouverture automatique de la vente.
-   VENTE OUVERTE : ventes et confirmations de réservations.
-   PLEIN : automatique lorsque billets VALIDÉ = capacité.
-   EN ROUTE : départ déclaré par un utilisateur autorisé.
-   ARRIVÉ : arrivée déclarée par un utilisateur autorisé à destination.

## Réservation

``` text
EN_COURS ──→ ANNULÉE
    │
    └──→ CONFIRMÉE → BILLET
```

Uniquement sur les voyages PLANIFIÉ. Pas d'expiration automatique
imposée par le système.

## Billetterie

Statuts MVP :

``` text
VALIDÉ → ANNULÉ
VALIDÉ → REMBOURSÉ
```

Un motif est requis. Aucun billet n'est supprimé physiquement.

## Capacité

``` text
Places disponibles = capacité - billets VALIDÉ - réservations EN_COURS
```

Les réservations ne rendent jamais le voyage PLEIN.

## Colis

``` text
EN_ATTENTE
   ↓ affectation
EN_COURS
   ↓ voyage EN_ROUTE
EN_ROUTE
   ↓ voyage ARRIVÉ
ARRIVÉ — CHECKING EN ATTENTE
   ↓ confirmation physique
ARRIVÉ
```

Les transitions liées au départ et à l'arrivée du voyage sont
automatiques pour les colis.
