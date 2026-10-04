# ERP Transport --- Architecture SaaS

## Principe

Une seule application sert plusieurs entreprises clientes.

``` text
ENTREPRISES
    ↓
APPLICATION SaaS
    ↓
AUTHENTIFICATION
    ↓
POSTGRESQL
    ↓
tenant_id + RLS
```

## Principes

-   une base de code commune ;
-   infrastructure mutualisée ;
-   déploiement centralisé ;
-   isolation logique des tenants ;
-   sécurité indépendante du frontend.

La sécurité combine authentification, contexte tenant, permissions
métier et RLS.
