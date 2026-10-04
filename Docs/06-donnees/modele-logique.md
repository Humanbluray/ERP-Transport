# ERP Transport --- Modèle logique

## Statut

À construire après validation des workflows, règles métier,
rôles/permissions et modèle conceptuel final.

## Principes déjà établis

-   PostgreSQL.
-   Architecture multi-tenant.
-   `tenant_id`.
-   RLS.
-   Historisation.
-   Statuts métier explicites.
-   Pas de suppression physique des données opérationnelles importantes.

## Séquence

``` text
Modèle conceptuel final
→ attributs et cardinalités
→ contraintes
→ statuts/transitions
→ historique/audit
→ tables PostgreSQL
→ RLS
```
