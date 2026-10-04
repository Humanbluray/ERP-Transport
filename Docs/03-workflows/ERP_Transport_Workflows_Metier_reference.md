# ERP Transport --- Synthèse des workflows métier principaux

> **Statut du document :** Référence de travail validée\
> **Périmètre :** Workflows opérationnels principaux\
> **Hors périmètre :** Administration / gouvernance de l'entreprise

------------------------------------------------------------------------

## 1. Vision générale

L'ERP Transport suit l'activité autour d'une unité centrale : le
**voyage**.

Un voyage représente une occurrence réelle d'un trajet et constitue le
support des opérations de transport :

-   transport de passagers ;
-   réservations ;
-   billetterie ;
-   transport de colis.

Un colis est toujours rattaché à un voyage.

Deux types de voyages sont retenus :

1.  **Voyage normal** --- transport de passagers et éventuellement de
    colis.
    -   voyage programmé ;
    -   voyage non programmé.
2.  **Voyage navette** --- transport de colis uniquement, sans
    passagers.
    -   créé par le responsable des colis.

À ce stade, les navettes restent modélisées comme un **type particulier
de voyage**. Une structure séparée ne sera envisagée que si les règles
métier futures le justifient.

------------------------------------------------------------------------

# 2. Workflow Voyage

## 2.1 Types de voyages

### Voyage normal

Un voyage normal peut être :

-   **programmé** : généré à partir d'une planification ;
-   **non programmé** : créé au besoin par l'exploitation.

Il peut transporter des passagers et des colis.

### Voyage navette

La navette :

-   ne transporte aucun passager ;
-   transporte uniquement des colis ;
-   est créée par le responsable des colis.

------------------------------------------------------------------------

## 2.2 Cycle de vie

``` text
PLANIFIÉ
    │
    │ Affectation du bus
    ▼
VENTE OUVERTE
    │
    │ Tous les billets sont validés
    ▼
PLEIN
    │
    │ Déclaration du départ
    ▼
EN ROUTE
    │
    │ Déclaration de l'arrivée
    ▼
ARRIVÉ
```

### PLANIFIÉ

Responsabilité principale : agence de départ / planificateur.

-   Le voyage existe mais aucun bus n'est encore affecté.
-   Les réservations peuvent être enregistrées.
-   La vente directe de billets n'est pas autorisée.

### Affectation du bus

L'affectation du bus déclenche automatiquement l'ouverture de la vente.

``` text
PLANIFIÉ + BUS AFFECTÉ
        ↓
VENTE OUVERTE
```

### VENTE OUVERTE

Le guichetier peut :

-   vendre les places disponibles ;
-   confirmer les réservations ;
-   créer les billets correspondants.

### PLEIN

Le système recalcule la capacité après chaque vente.

Le voyage devient **PLEIN uniquement lorsque :**

``` text
nombre de billets VALIDÉ = capacité du bus
```

Les réservations EN_COURS ne rendent jamais le voyage PLEIN.

Lorsque le voyage devient PLEIN, le système notifie notamment :

-   le planificateur ;
-   l'agence responsable.

Si un billet valide est annulé ou remboursé et que le nombre de billets
valides repasse sous la capacité, le voyage revient automatiquement à
**VENTE OUVERTE**.

### EN ROUTE

Le passage à EN ROUTE est une action opérationnelle manuelle.

Un utilisateur disposant de la permission appropriée peut déclarer le
départ après les vérifications terrain.

Le système enregistre notamment :

-   la date/heure réelle de départ ;
-   l'utilisateur ayant déclaré le départ ;
-   l'agence concernée.

Le système ne bloque pas le départ parce qu'un passager ayant payé est
absent.

### ARRIVÉ

L'agence d'arrivée prend la responsabilité opérationnelle du voyage
lorsqu'il est EN ROUTE.

À l'arrivée, un utilisateur autorisé déclare l'arrivée effective.

Le passage à ARRIVÉ est manuel.

------------------------------------------------------------------------

# 3. Workflow Réservation

## 3.1 Principe

Une réservation représente une **intention de voyager** et non une
vente.

Elle peut être créée uniquement lorsque le voyage est encore :

``` text
PLANIFIÉ
```

Aucun paiement n'est effectué au moment de la réservation.

------------------------------------------------------------------------

## 3.2 Cycle de vie

``` text
EN_COURS
   │
   ├──────────────► ANNULÉE
   │
   │ confirmation + paiement
   ▼
CONFIRMÉE
   │
   ▼
CRÉATION DU BILLET
```

### EN_COURS

-   La place est réservée.
-   Elle n'est pas encore comptabilisée comme billet validé.
-   Elle ne contribue donc pas au statut PLEIN.

### CONFIRMÉE

Le passager se présente, confirme son voyage et paie.

La réservation devient CONFIRMÉE et un billet est créé.

### ANNULÉE

La réservation est conservée dans l'historique avec le statut ANNULÉE.

La place est libérée.

### Règle importante

Il n'y a **pas d'expiration automatique imposée par le système**.

Le moment où une réservation non confirmée peut être libérée relève de
la décision opérationnelle du guichetier / de l'agence.

------------------------------------------------------------------------

# 4. Workflow Billetterie

## 4.1 Création d'un billet

Un billet peut être créé de deux façons :

1.  vente directe pendant un voyage en **VENTE OUVERTE** ;
2.  confirmation d'une réservation EN_COURS.

------------------------------------------------------------------------

## 4.2 Statuts retenus

Seuls trois statuts sont nécessaires dans le MVP :

``` text
VALIDÉ
ANNULÉ
REMBOURSÉ
```

Aucun statut supplémentaire n'est retenu à ce stade.

------------------------------------------------------------------------

## 4.3 Cycle de vie

``` text
                 ┌──► ANNULÉ
VALIDÉ ──────────┤
                 └──► REMBOURSÉ
```

Un billet VALIDÉ peut être annulé ou remboursé par un utilisateur
autorisé.

Le changement de statut :

-   nécessite un motif ;
-   est conservé dans l'historique ;
-   libère la place ;
-   déclenche le recalcul du statut du voyage.

Il n'y a aucune suppression physique du billet.

------------------------------------------------------------------------

# 5. Gestion de la capacité

La gestion des places distingue clairement :

-   les billets validés ;
-   les réservations EN_COURS.

## Places disponibles à la vente

``` text
Places disponibles =
capacité du bus
- billets VALIDÉ
- réservations EN_COURS
```

## Voyage PLEIN

``` text
billets VALIDÉ = capacité du bus
```

### Exemple

Bus de 70 places :

``` text
65 billets VALIDÉ
+ 5 réservations EN_COURS
= 0 place disponible à la vente
```

Mais le voyage reste :

``` text
VENTE OUVERTE
```

Il ne devient PLEIN que lorsque les 70 places correspondent à des
billets VALIDÉ.

------------------------------------------------------------------------

# 6. Workflow Colis

## 6.1 Principe fondamental

Un colis est toujours transporté par un voyage.

Il peut être affecté :

-   à un voyage normal ;
-   à une voyage navette.

------------------------------------------------------------------------

## 6.2 Cycle de vie du colis

Les noms définitifs des statuts restent à améliorer, mais la logique
validée est :

``` text
EN_ATTENTE
    │
    │ Affectation à un voyage
    ▼
EN_COURS
    │
    │ Voyage EN ROUTE
    ▼
EN_ROUTE
    │
    │ Voyage ARRIVÉ
    ▼
ARRIVÉ — CHECKING EN ATTENTE
    │
    │ Réception physique confirmée
    ▼
ARRIVÉ
```

### EN_ATTENTE

Le colis est enregistré mais n'est pas encore affecté à un voyage.

### EN_COURS

Le colis est affecté à un voyage, mais celui-ci n'est pas encore parti.

### EN_ROUTE

Lorsque le voyage passe automatiquement à EN_ROUTE, les colis affectés à
ce voyage passent automatiquement à EN_ROUTE.

Aucune modification manuelle colis par colis n'est nécessaire.

### ARRIVÉ --- checking en attente

Lorsque le voyage passe à ARRIVÉ, les colis concernés passent
automatiquement dans un état indiquant :

> voyage arrivé, réception physique du colis encore à vérifier.

### ARRIVÉ

Après vérification physique et confirmation de la réception par
l'opérateur, le colis passe à ARRIVÉ.

La confirmation peut éventuellement déclencher l'envoi d'un SMS au
destinataire lorsque cette fonctionnalité sera activée.

------------------------------------------------------------------------

# 7. Responsabilités autour des colis

### Guichetier / opérateur

-   enregistrer les colis ;
-   consulter les colis ;
-   affecter les colis à un voyage ;
-   suivre leur état.

### Responsable colis

-   créer les voyages navettes ;
-   organiser les affectations de colis ;
-   superviser le suivi des colis ;
-   participer au contrôle à l'arrivée.

### Agence d'arrivée / opérateur autorisé

-   prendre en charge les opérations liées à l'arrivée ;
-   vérifier physiquement les colis ;
-   confirmer leur réception.

------------------------------------------------------------------------

# 8. Automatisations métier validées

L'ERP doit automatiser les conséquences évidentes des événements
opérationnels.

## Départ du voyage

``` text
Voyage → EN_ROUTE
        ↓
Colis affectés → EN_ROUTE
```

## Arrivée du voyage

``` text
Voyage → ARRIVÉ
        ↓
Colis affectés → ARRIVÉ / CHECKING
```

## Voyage plein

``` text
Dernier billet VALIDÉ
        ↓
Recalcul capacité
        ↓
Billets VALIDÉ = capacité
        ↓
Voyage → PLEIN
        ↓
Notifications
```

## Annulation / remboursement d'un billet

``` text
Billet VALIDÉ
      ↓
ANNULÉ / REMBOURSÉ
      ↓
Place libérée
      ↓
Recalcul du voyage
      ↓
Si billets VALIDÉ < capacité
      ↓
VENTE OUVERTE
```

------------------------------------------------------------------------

# 9. Principes métier transversaux

Les workflows validés reposent sur quelques principes structurants :

1.  **L'historique opérationnel est conservé.** Les réservations et
    billets ne sont pas supprimés physiquement.

2.  **Le système enregistre la réalité opérationnelle.** Il ne doit pas
    imposer des contraintes irréalistes au terrain.

3.  **Les événements du voyage entraînent automatiquement les
    conséquences évidentes sur les colis.**

4.  **Une réservation n'est pas un billet.** Elle bloque une place mais
    ne compte pas dans le calcul du plein.

5.  **Le plein est déterminé par les billets VALIDÉ.**

6.  **Les changements sensibles de statut sont contrôlés par des
    permissions.** Le système ne doit pas dépendre exclusivement d'un
    intitulé de rôle.

7.  **Le voyage constitue l'unité opérationnelle centrale.** Billets,
    réservations et colis sont rattachés à celui-ci.

8.  **Les statuts représentent l'état métier et non toutes les
    situations physiques possibles.** On évite de multiplier les statuts
    lorsque cela rendrait l'exploitation inutilement complexe.

------------------------------------------------------------------------

# 10. Ce qui reste volontairement à définir

Les workflows principaux sont désormais établis. Les points suivants
pourront être précisés lors de la consolidation des règles métier :

-   noms définitifs des statuts colis ;
-   règles détaillées en cas d'annulation d'un voyage ;
-   règles de réaffectation d'un colis ;
-   gestion d'un colis non récupéré ;
-   gestion des colis perdus ou incidents ;
-   éventuelles règles particulières liées aux navettes ;
-   détail des notifications SMS ;
-   traçabilité/audit détaillée des changements de statut.

Ces points ne remettent pas en cause le workflow principal validé.

------------------------------------------------------------------------

# 11. Prochaine étape

Les workflows principaux étant maintenant définis, la prochaine étape
logique est la :

## Consolidation des règles métier

Objectif :

``` text
Workflows validés
      ↓
Règles métier numérotées
      ↓
Matrice rôles / permissions
      ↓
Modèle conceptuel final
      ↓
Modèle logique
      ↓
Tables + contraintes + historiques
      ↓
SQL PostgreSQL / Supabase
```

Ce document constitue donc la **base de référence fonctionnelle des
workflows opérationnels du MVP**.
