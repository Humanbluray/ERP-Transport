# ERP Transport --- Utilisateurs, rôles et permissions

## Principe

Les permissions sont **définies par le produit et immuables**.
L'entreprise configure ses rôles en regroupant ces permissions.

## Gouvernance

### OWNER

Gouvernance complète de l'entreprise, administration globale et
validation des actions nécessitant son niveau d'autorité. Consultation
des opérations.

### ADMIN

Administration déléguée. Consultation des opérations mais pas
d'exécution des opérations métier quotidiennes.

## Actions ADMIN déjà validées

-   Ajouter une agence.
-   Ajouter un trajet.
-   Ajouter un utilisateur.
-   Désactiver un utilisateur.
-   Affecter une agence à un utilisateur.
-   Affecter un rôle à un utilisateur.
-   Créer un bus.

La création d'une agence et la création d'un bus par ADMIN nécessitent
validation OWNER.

## Principe opérationnel

Les droits sont liés aux permissions, pas uniquement au nom du rôle.

Exemple :

``` text
VOYAGE_DECLARER_DEPART
        ↓
tout rôle auquel cette permission est attribuée
```

La matrice complète sera construite après consolidation des règles
métier.
