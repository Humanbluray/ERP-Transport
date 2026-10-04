# ERP Transport --- Multi-tenant et RLS

## Architecture

``` text
Entreprise A ─┐
Entreprise B ─┼→ même application
Entreprise C ─┘
              ↓
       PostgreSQL partagé
              ↓
       tenant_id + RLS
```

Chaque entreprise utilise la même application et la même infrastructure
tout en restant isolée logiquement.

## RLS

PostgreSQL Row Level Security doit empêcher qu'un utilisateur d'un
tenant consulte ou modifie les lignes d'un autre tenant.

Le filtrage de sécurité ne doit pas dépendre uniquement du frontend ou
du code applicatif.

## Objectifs

-   séparation stricte des données ;
-   réduction du risque de fuite inter-tenant ;
-   protection contre les requêtes mal filtrées ;
-   mutualisation SaaS ;
-   maîtrise des coûts.

Les politiques RLS détaillées seront définies avec le modèle de données
final.
