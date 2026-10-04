# Hiérarchie de la documentation du projet — ERP Transport

## 1. Pourquoi cette hiérarchie ?

La documentation du projet doit permettre à chaque membre de comprendre le produit, ses règles et ses choix, même s’il rejoint l’équipe en cours de développement.

Elle doit aussi empêcher qu’une règle métier soit inventée au moment du développement.

Notre principe est donc :

> **Une règle métier doit être décidée, documentée, puis implémentée.**

Si une information nécessaire au développement n’est pas définie, elle devient un **point à décider**, et non une permission d’improviser.

---

# 2. Les quatre niveaux

La documentation est organisée selon cette hiérarchie :

```text
NIVEAU 1 — VISION
        ↓
NIVEAU 2 — ORGANISATION & MÉTIER
        ↓
NIVEAU 3 — RÈGLES & WORKFLOWS
        ↓
NIVEAU 4 — TECHNIQUE
        ↓
CODE
```

Chaque niveau répond à une question différente.

---

## Niveau 1 — Vision

### Question

> **Pourquoi construisons-nous cet ERP ?**

Ce niveau décrit la raison d’être du projet :

- problème à résoudre ;
- utilisateurs et clients visés ;
- vision du produit ;
- objectifs ;
- périmètre ;
- MVP ;
- éléments explicitement hors MVP.

La Vision donne la direction générale du projet.

Elle permet notamment de décider si une fonctionnalité est cohérente avec le produit.

Elle ne décrit pas comment programmer cette fonctionnalité.

---

## Niveau 2 — Organisation et métier

### Question

> **Quelles sont les entités et comment fonctionne l’entreprise de transport dans notre ERP ?**

Ce niveau décrit le monde métier que le logiciel doit représenter.

On y trouve notamment :

- l’entreprise ;
- les agences ;
- les utilisateurs ;
- les bus ;
- les catégories de bus ;
- les trajets ;
- les tarifs ;
- les voyages ;
- les billets ;
- les passagers ;
- les colis ;
- les autres entités qui seront validées.

On y décrit également les relations, la possession, les responsabilités et la place de chaque entité.

Exemple issu de l’architecture validée lors de la réunion du 5 septembre : l’entreprise est l’entité supérieure et possède notamment les agences, utilisateurs, bus, catégories de bus, trajets et tarifs, tandis que l’agence gère l’activité opérationnelle des voyages.

### Pourquoi ce niveau est important

Avant de définir ce que les utilisateurs peuvent faire, nous devons savoir **sur quelles entités ils travaillent**.

Ce niveau répond donc à des questions comme :

- Qu’est-ce qu’une agence ?
- Qu’est-ce qu’un trajet ?
- Qu’est-ce qu’un voyage ?
- À qui appartient un bus ?
- Qui possède les tarifs ?
- Quelle entité dépend de quelle autre ?
- Quelle est la responsabilité de chaque entité ?

---

## Niveau 3 — Règles et workflows

### Question

> **Que peut-on faire, qui peut le faire, quand et sous quelles conditions ?**

C’est ici que notre compréhension du métier devient un comportement précis du système.

### Les règles métier

Une règle métier définit une contrainte ou une condition que le système doit respecter.

Exemple :

> **Un bus ne peut être planifié que s’il est disponible.**

Cette phrase doit ensuite être précisée.

Que signifie exactement « disponible » ?

Quelles conditions doivent être réunies ?

Qui peut planifier le bus ?

Que se passe-t-il si le bus n’est pas disponible ?

Ces éléments doivent être définis et validés avant leur implémentation.

### Les workflows

Un workflow décrit le déroulement d’une opération :

```text
Déclencheur
    ↓
Action
    ↓
Vérifications
    ↓
Conditions
    ↓
Validation
    ↓
État suivant
```

Il répond notamment à :

- Qui démarre l’opération ?
- Quelle action est effectuée ?
- Quelles vérifications sont nécessaires ?
- Quelles conditions doivent être satisfaites ?
- Qui peut valider ?
- Quel est l’état suivant ?
- Que se passe-t-il en cas d’échec, d’annulation ou d’imprévu ?

Le niveau 3 constitue donc le **contrat fonctionnel et métier** que le logiciel devra respecter.

---

## Niveau 4 — Technique

### Question

> **Comment implémentons-nous ce comportement ?**

Une fois la vision, le métier, les règles et les workflows définis, l’équipe technique détermine comment les réaliser.

Ce niveau concerne notamment :

- architecture logicielle ;
- structure du projet ;
- technologies ;
- base de données ;
- API ;
- services ;
- repositories ;
- sécurité ;
- RLS ;
- tests ;
- déploiement ;
- conventions de code.

Exemple avec le bus :

```text
NIVEAU 1 — VISION
Pourquoi gérer les bus ?
        ↓
NIVEAU 2 — MÉTIER
Qu’est-ce qu’un bus ?
À qui appartient-il ?
Quelle relation avec l’agence et le voyage ?
        ↓
NIVEAU 3 — RÈGLE / WORKFLOW
Dans quelles conditions est-il disponible ?
Qui peut l’affecter ?
Que se passe-t-il après départ et arrivée ?
        ↓
NIVEAU 4 — TECHNIQUE
Comment vérifier la disponibilité ?
Quelle structure de données ?
Quel service ?
Quelle transaction ?
Quelles contraintes PostgreSQL ?
Quelles RLS ?
```

**Le niveau technique n’invente donc pas le comportement métier.**

Il implémente un comportement défini aux niveaux précédents.

---

# 3. La logique générale

On peut retenir :

```text
VISION
  ↓
POURQUOI

ORGANISATION & MÉTIER
  ↓
QUOI

RÈGLES & WORKFLOWS
  ↓
QUI / QUAND / SOUS QUELLES CONDITIONS

TECHNIQUE
  ↓
COMMENT

CODE
```

Cette séparation permet d’éviter de mélanger les décisions produit, métier et techniques.

---

# 4. Le principe de non-invention

Le processus de référence est :

```text
DISCUSSION
    ↓
PROPOSITION
    ↓
VALIDATION DE L’ÉQUIPE
    ↓
RÈGLE DOCUMENTÉE
    ↓
WORKFLOW DOCUMENTÉ
    ↓
CHOIX TECHNIQUE
    ↓
IMPLÉMENTATION
```

Une idée discutée n’est donc pas automatiquement une règle.

Une proposition d’un développeur n’est pas automatiquement une règle.

Une solution technique n’est pas automatiquement une règle métier.

Si une règle n’a pas été validée, elle doit rester identifiée comme **à décider**.

---

# 5. Le Journal des décisions

Les décisions importantes sont enregistrées dans un journal central.

Chaque décision reçoit un identifiant unique :

```text
DEC-001
DEC-002
DEC-003
...
```

Une décision doit notamment indiquer :

- identifiant ;
- date ;
- sujet ;
- décision ;
- justification lorsque nécessaire ;
- statut ;
- documents impactés.

Exemple :

```text
DEC-XXX

Sujet :
Propriété des bus

Décision :
Les bus appartiennent à l’entreprise et non aux agences.

Statut :
VALIDÉ

Impacts :
- organisation métier
- voyages
- disponibilité des bus
- permissions
- modèle de données
```

Le journal conserve également l’historique des évolutions.

Une nouvelle décision ne doit pas effacer silencieusement une ancienne décision.

---

# 6. Comment un nouveau développeur doit découvrir le projet

Un développeur qui rejoint le projet doit pouvoir suivre ce parcours :

```text
1. Vision
      ↓
2. Organisation métier
      ↓
3. Entités et responsabilités
      ↓
4. Règles métier
      ↓
5. Workflows
      ↓
6. Architecture technique
      ↓
7. Conventions de développement
      ↓
8. Journal des décisions
      ↓
9. Code
```

Il doit pouvoir comprendre :

> **Pourquoi le projet existe → ce qu’il représente → comment il fonctionne → quelles règles il doit respecter → comment nous avons choisi de le construire.**

Il ne doit pas être obligé de retrouver ces informations dans les anciennes conversations WhatsApp, réunions ou discussions entre développeurs.

---

# 7. Principe directeur

La documentation doit permettre de remonter du code vers la décision métier :

```text
CODE
 ↑
ARCHITECTURE TECHNIQUE
 ↑
WORKFLOW
 ↑
RÈGLE MÉTIER
 ↑
ORGANISATION MÉTIER
 ↑
VISION
```

Ainsi, lorsqu’un développeur rencontre une fonction, une contrainte ou un comportement dans le code, il doit être possible de comprendre **ce qu’il implémente et pourquoi**.

---

# 8. Conclusion

Cette hiérarchie n’est pas simplement une organisation de fichiers Markdown.

C’est une méthode de travail collective.

Elle sépare clairement :

- **ce que nous voulons construire** ;
- **le métier que nous voulons représenter** ;
- **les règles que nous avons décidé d’appliquer** ;
- **la manière dont nous choisissons de les implémenter**.

Notre principe de référence est donc :

> **On ne code pas d’abord pour découvrir ensuite la règle métier.**
>
> **On définit la vision, on comprend le métier, on valide les règles et les workflows, puis on choisit la solution technique et on code.**
