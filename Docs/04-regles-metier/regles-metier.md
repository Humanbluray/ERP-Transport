# ERP Transport --- Règles métier

> Première consolidation. À compléter et auditer avant le modèle
> logique.

## Voyage

**RM-001** --- Un voyage est une occurrence concrète d'un trajet.

**RM-002** --- Un trajet est orienté départ → arrivée.

**RM-003** --- Un voyage est NORMAL ou NAVETTE.

**RM-004** --- Un voyage NORMAL est PROGRAMMÉ ou NON_PROGRAMMÉ.

**RM-005** --- Une NAVETTE transporte uniquement des colis.

**RM-006** --- Une réservation peut être créée sur un voyage PLANIFIÉ.

**RM-007** --- La vente directe n'est pas autorisée sur un voyage
PLANIFIÉ.

**RM-008** --- L'affectation du bus ouvre automatiquement la vente.

**RM-009** --- PLEIN est automatique lorsque billets VALIDÉ = capacité
du bus.

**RM-010** --- Les réservations EN_COURS ne comptent pas pour PLEIN.

**RM-011** --- Une annulation/remboursement libérant une place déclenche
un recalcul.

**RM-012** --- Le départ est déclaré par un utilisateur disposant de la
permission appropriée.

**RM-013** --- L'arrivée est déclarée par un utilisateur autorisé à
l'agence d'arrivée.

## Réservation et billet

**RM-014** --- Une réservation n'est pas un billet.

**RM-015** --- Une réservation confirmée crée un billet.

**RM-016** --- Une réservation annulée reste dans l'historique.

**RM-017** --- Les billets MVP ont uniquement VALIDÉ, ANNULÉ et
REMBOURSÉ.

**RM-018** --- Un motif est obligatoire lors de
l'annulation/remboursement.

**RM-019** --- Les billets ne sont jamais supprimés physiquement.

## Colis

**RM-020** --- Tout colis est rattaché à un voyage.

**RM-021** --- Un colis peut être transporté par un voyage NORMAL ou une
NAVETTE.

**RM-022** --- EN_ATTENTE signifie enregistré mais non affecté.

**RM-023** --- EN_COURS signifie affecté à un voyage qui n'est pas
encore EN_ROUTE.

**RM-024** --- Voyage EN_ROUTE ⇒ colis concernés EN_ROUTE
automatiquement.

**RM-025** --- Voyage ARRIVÉ ⇒ colis concernés en attente de checking
automatiquement.

**RM-026** --- Le colis devient ARRIVÉ après confirmation physique de
réception.

## Principes transversaux

**RM-027** --- Les données opérationnelles historiques ne sont pas
supprimées physiquement.

**RM-028** --- Le système doit refléter la réalité opérationnelle et
éviter les contraintes irréalistes.

**RM-029** --- Les changements sensibles de statut sont contrôlés par
permissions.
