# ERP Transport --- Modèle métier conceptuel

``` text
ENTREPRISE
 ├── AGENCES
 │    └── TRAJETS
 │         └── VOYAGES
 │              ├── RÉSERVATIONS
 │              ├── BILLETS
 │              └── COLIS
 ├── BUS
 ├── UTILISATEURS
 └── RÔLES / PERMISSIONS
```

## Entités principales

-   **Entreprise** : tenant du SaaS.
-   **Agence** : point opérationnel de l'entreprise.
-   **Trajet** : liaison orientée entre une agence de départ et une
    agence d'arrivée.
-   **Voyage** : occurrence concrète d'un trajet ; unité opérationnelle
    centrale.
-   **Bus** : ressource affectée à un voyage.
-   **Réservation** : intention de voyage, distincte d'un billet.
-   **Billet** : titre de transport issu d'une vente ou d'une
    réservation confirmée.
-   **Colis** : objet transporté, toujours rattaché à un voyage.
-   **Utilisateur** : personne utilisant l'ERP.
-   **Rôle / Permission** : mécanisme de contrôle des accès.

Un trajet est orienté : Yaoundé → Douala est différent de Douala →
Yaoundé.
