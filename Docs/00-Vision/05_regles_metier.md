# 📐 ERP TRANSPORT
## Document 05 — Règles métier

**Version : 0.1 — Document de travail**  
**Statut : Préparation du projet**  
**Documents liés :**
- `01_vision_cadrage.md`
- `02_utilisateurs_personas.md`
- `03_modules_fonctionnels.md`
- `04_workflows_metier.md`

---

# 1. 🎯 Objectif du document

Ce document formalise les règles qui déterminent **ce que le système autorise, interdit ou doit contrôler**.

Une fonctionnalité décrit ce que le logiciel permet de faire.

Une règle métier décrit **dans quelles conditions cette action est possible et quelles conséquences elle entraîne**.

> 💡 Exemple : « La billetterie permet de vendre un billet » est une fonctionnalité.  
> « Un même siège ne peut être vendu deux fois pour un même voyage » est une règle métier.

Les règles définies ici devront être validées par l'équipe avant d'être considérées comme définitives.

---

# 2. 🧠 Principes généraux

## RM-001 — Une information métier doit avoir une source unique

Une information ne doit pas être saisie plusieurs fois lorsqu'elle peut être dérivée automatiquement.

Exemple :

```text
🎫 Vente billet
      ↓
💰 Recette
      ↓
🏦 Caisse
      ↓
📊 Reporting
```

L'agent ne doit pas saisir manuellement la même vente dans plusieurs modules.

---

## RM-002 — Les données d'une entreprise sont isolées

Dans un environnement SaaS multi-tenant :

> Une entreprise ne doit jamais pouvoir consulter ou modifier les données d'une autre entreprise.

Cette règle concerne notamment :

- utilisateurs ;
- agences ;
- véhicules ;
- chauffeurs ;
- voyages ;
- billets ;
- caisses ;
- recettes ;
- rapports.

---

## RM-003 — Toute action sensible doit être contrôlée

Les actions pouvant avoir un impact financier ou opérationnel important doivent être soumises à des permissions appropriées.

Exemples :

- annulation billet ;
- remboursement ;
- modification tarif ;
- clôture caisse ;
- modification voyage ;
- changement de véhicule ;
- modification de recette.

---

# 3. 🏢 Règles entreprise / agences

## RM-010 — Une entreprise peut posséder plusieurs agences

Une entreprise cliente peut avoir :

```text
Entreprise
   ├── Agence A
   ├── Agence B
   └── Agence C
```

---

## RM-011 — Une agence appartient à une seule entreprise

Une agence ne peut pas appartenir simultanément à plusieurs entreprises.

---

## RM-012 — Un utilisateur appartient au périmètre d'une entreprise

Un utilisateur travaillant pour une entreprise ne doit accéder qu'aux ressources auxquelles son rôle et ses permissions lui donnent accès.

---

## RM-013 — Une entreprise peut avoir plusieurs utilisateurs

Les utilisateurs doivent pouvoir être affectés à des rôles différents.

---

## RM-014 — Une agence peut avoir plusieurs utilisateurs

Plusieurs agents peuvent travailler dans une même agence.

---

# 4. 👥 Règles utilisateurs et permissions

## RM-020 — Les droits dépendent du rôle

Chaque utilisateur possède un ensemble de permissions correspondant à ses responsabilités.

---

## RM-021 — Un utilisateur ne doit pas disposer automatiquement de tous les droits

La création d'un compte ne doit pas donner accès à toutes les fonctions de l'entreprise.

---

## RM-022 — Les actions sensibles doivent être journalisées

Exemples :

```text
👤 Utilisateur
🕐 Date / heure
🎯 Action
📦 Objet concerné
🔄 Ancienne valeur
➡️ Nouvelle valeur
```

---

## RM-023 — La suppression physique doit être limitée

Pour les données ayant une importance historique ou financière, une suppression définitive ne doit pas être privilégiée.

Exemple :

Un billet payé ne doit pas simplement disparaître de la base.

Il doit conserver son historique et passer dans un état approprié.

---

# 5. 📚 Règles référentiels

## RM-030 — Une ligne doit avoir une origine et une destination

Exemple :

> Yaoundé → Douala

---

## RM-031 — Une ligne active peut être utilisée pour créer des voyages

Une ligne inactive ne doit pas pouvoir être sélectionnée pour une nouvelle programmation.

---

## RM-032 — Un tarif doit être rattaché à un contexte défini

Le système devra déterminer si le tarif dépend :

- de la ligne ;
- de la classe ;
- du type de voyage ;
- de la période ;
- d'autres paramètres.

Cette règle devra être précisée avant la conception finale de la billetterie.

---

# 6. 🚌 Règles véhicules

## RM-040 — Un véhicule doit avoir un identifiant unique

L'immatriculation ou un identifiant interne doit permettre de distinguer chaque véhicule.

---

## RM-041 — Un véhicule indisponible ne peut pas être affecté à un voyage

Sont notamment concernés :

- véhicule en maintenance ;
- véhicule immobilisé ;
- véhicule hors service.

---

## RM-042 — Un véhicule ne peut pas être affecté à deux voyages incompatibles

Le système doit contrôler les conflits d'horaires.

Exemple :

```text
BUS-023

07h30 → Yaoundé-Douala
      +
08h00 → Yaoundé-Bafoussam

❌ Conflit
```

---

## RM-043 — La capacité du véhicule doit être connue

La capacité sert notamment à déterminer :

- le nombre maximal de sièges ;
- le taux de remplissage ;
- les ventes possibles.

---

## RM-044 — Le véhicule affecté peut être remplacé

Une panne ou un incident peut nécessiter un remplacement.

Le remplacement doit :

- être autorisé ;
- être journalisé ;
- conserver l'historique ;
- vérifier la capacité du nouveau véhicule.

---

## RM-045 — Un véhicule en maintenance ne doit pas être exploitable

Sauf décision explicite permettant un statut particulier.

---

# 7. 👨‍✈️ Règles chauffeurs

## RM-050 — Un chauffeur doit avoir un statut

Exemples :

```text
🟢 Actif
🟡 Suspendu
🔴 Inactif
```

---

## RM-051 — Un chauffeur inactif ne peut pas être affecté

---

## RM-052 — Le système doit contrôler les conflits d'affectation

Un chauffeur ne doit pas être affecté à deux voyages incompatibles.

---

## RM-053 — Le permis doit être compatible avec le véhicule

Cette règle devra tenir compte des catégories de permis pertinentes.

---

## RM-054 — Une affectation de chauffeur doit être historisée

Le système doit conserver :

- voyage ;
- chauffeur ;
- date ;
- heure ;
- éventuellement motif de remplacement.

---

# 8. 📅 Règles planification

## RM-060 — Un voyage doit avoir les informations minimales nécessaires

Avant validation, un voyage doit disposer au minimum des éléments nécessaires à son exploitation.

Exemple :

- ligne ;
- date ;
- heure ;
- véhicule ;
- chauffeur ;
- capacité ;
- tarif.

La liste exacte devra être validée.

---

## RM-061 — Les conflits doivent être détectés avant validation

Le système doit contrôler notamment :

- véhicule ;
- chauffeur ;
- horaire ;
- disponibilité.

---

## RM-062 — Un voyage peut avoir plusieurs états

Exemple :

```text
📝 Brouillon
📅 Planifié
🎫 Ouvert
🛂 Embarquement
🚦 Départ
🛣️ En cours
🏁 Arrivé
✅ Clôturé
❌ Annulé
```

Les statuts définitifs devront être validés.

---

## RM-063 — Les transitions de statut doivent être contrôlées

Un voyage ne doit pas pouvoir passer arbitrairement d'un statut à un autre.

Exemple :

```text
📝 Brouillon
   ↓
📅 Planifié
   ↓
🎫 Ouvert
   ↓
🚦 Départ
```

---

# 9. 🎫 Règles billetterie

## RM-070 — Un billet appartient à un voyage

Un billet ne peut pas exister sans voyage associé.

---

## RM-071 — Un billet doit avoir un identifiant unique

Le numéro de billet doit permettre de retrouver rapidement l'opération.

---

## RM-072 — Un siège ne peut être vendu deux fois pour le même voyage

C'est une règle critique.

```text
Voyage X + Siège 12

Premier achat → ✅
Deuxième achat → ❌
```

Le contrôle doit être garanti même si deux agents tentent la vente simultanément.

---

## RM-073 — Un siège réservé n'est pas nécessairement un siège payé

Le système doit distinguer :

```text
🟡 Réservé
🟢 Payé
```

---

## RM-074 — Une réservation peut expirer

Une réservation non payée peut avoir une durée de validité.

Exemple :

> 30 minutes.

La durée exacte reste à définir.

---

## RM-075 — Un billet payé ne doit pas être supprimé

Il doit être :

- annulé ;
- remboursé si nécessaire ;
- conservé dans l'historique.

---

## RM-076 — Une vente doit générer une opération financière

Lorsqu'un billet est payé :

```text
🎫 Billet
   ↓
💰 Paiement
   ↓
🏦 Caisse
```

Les détails dépendront du mode de paiement.

---

## RM-077 — Le prix réellement payé doit être historisé

Si un tarif change plus tard, les anciens billets doivent conserver le montant effectivement payé.

---

# 10. ❌ Règles annulation / remboursement

## RM-080 — Un billet ne peut être annulé que par un utilisateur autorisé

---

## RM-081 — L'annulation doit conserver l'historique

Le système doit connaître :

- qui a annulé ;
- quand ;
- pourquoi ;
- quel billet ;
- quel montant.

---

## RM-082 — Le remboursement doit être lié au paiement initial

Il ne doit pas être enregistré comme une opération indépendante sans référence au billet concerné.

---

## RM-083 — Les conditions de remboursement doivent être configurables

Exemples possibles :

- remboursement intégral ;
- remboursement partiel ;
- aucun remboursement ;
- remboursement avant une certaine échéance.

Les règles commerciales devront être définies par l'entreprise.

---

# 11. 💺 Règles sièges

## RM-090 — Chaque siège doit être identifiable

Exemple :

```text
01
02
03
...
35
```

---

## RM-091 — La disponibilité du siège dépend du voyage

Un même siège peut être disponible sur un autre voyage utilisant le même véhicule.

---

## RM-092 — Le plan de sièges doit dépendre de la configuration du véhicule

Le système ne doit pas supposer que tous les bus ont le même nombre ou la même disposition de sièges.

---

# 12. 🛂 Règles embarquement

## RM-100 — Seul un billet valide permet l'embarquement

---

## RM-101 — Un billet annulé ne permet pas l'embarquement

---

## RM-102 — Un billet déjà embarqué ne doit pas être embarqué une deuxième fois

---

## RM-103 — L'embarquement doit être enregistré

Le système doit pouvoir savoir :

- quand ;
- par qui ;
- pour quel voyage ;
- pour quel billet.

---

# 13. 🚦 Règles départ / arrivée

## RM-110 — Un voyage ne peut être marqué comme parti sans validation préalable

Les contrôles exacts devront être définis.

---

## RM-111 — L'heure réelle de départ doit être distincte de l'heure programmée

Exemple :

```text
Heure prévue : 07h30
Heure réelle : 07h48
```

Cette distinction permettra de calculer les retards.

---

## RM-112 — L'arrivée doit enregistrer les données réelles

Notamment :

- heure ;
- kilométrage ;
- incidents éventuels.

---

# 14. 💰 Règles finance / caisses

## RM-120 — Une caisse doit avoir un cycle de vie

```text
🔓 Ouverte
   ↓
💰 En activité
   ↓
🔒 Clôturée
```

---

## RM-121 — Une caisse clôturée ne doit pas être modifiable librement

Toute correction doit être contrôlée et historisée.

---

## RM-122 — Le système doit distinguer solde théorique et solde réel

```text
Solde théorique
      VS
Solde réel
      ↓
Écart
```

---

## RM-123 — Les écarts de caisse doivent être conservés

Un écart ne doit pas être simplement écrasé.

---

## RM-124 — Les recettes doivent pouvoir être rattachées à leur origine

Exemple :

```text
💰 Recette
   ├── Billet
   ├── Colis
   └── Autre
```

---

# 15. ⛽ Règles carburant

## RM-130 — Un plein doit être rattaché à un véhicule

---

## RM-131 — Le kilométrage doit être cohérent

Le système doit pouvoir détecter un kilométrage inférieur au précédent relevé, sauf correction autorisée.

---

## RM-132 — Une consommation doit pouvoir être analysée

Le système doit permettre de calculer des indicateurs comme :

- litres/100 km ;
- coût/km ;
- coût par voyage.

---

# 16. 🔧 Règles maintenance

## RM-140 — Une maintenance doit être historisée

Elle doit conserver notamment :

- véhicule ;
- date ;
- kilométrage ;
- type ;
- description ;
- coût ;
- intervenant éventuel.

---

## RM-141 — Une maintenance peut rendre un véhicule indisponible

Selon le type d'intervention.

---

## RM-142 — La fin de maintenance doit permettre de rendre le véhicule disponible

Après validation.

---

## RM-143 — Les alertes doivent être déclenchées selon des critères définis

Exemples :

- kilométrage ;
- date ;
- échéance document.

---

# 17. 📦 Règles colis

## RM-150 — Un colis doit avoir un identifiant unique

---

## RM-151 — Un colis doit avoir une origine et une destination

---

## RM-152 — Un colis en transit doit être associé à un voyage

---

## RM-153 — Un colis livré doit conserver son historique

Exemple :

```text
📦 Déposé
   ↓
🛣️ En transit
   ↓
🏁 Arrivé
   ↓
👤 Retiré
```

---

# 18. 🔄 Règles remplacement

## RM-160 — Le remplacement d'un véhicule doit être historisé

Le système doit conserver :

```text
Ancien véhicule
      ↓
Motif
      ↓
Nouveau véhicule
      ↓
Date / heure
      ↓
Utilisateur
```

---

## RM-161 — Le nouveau véhicule doit être disponible

---

## RM-162 — Le nouveau véhicule doit être compatible avec les contraintes du voyage

Notamment :

- capacité ;
- statut ;
- disponibilité.

---

## RM-163 — Le remplacement ne doit pas supprimer les billets existants

Les réservations et billets doivent rester liés au voyage.

---

# 19. ⚠️ Règles incidents

## RM-170 — Un incident doit être associé à un contexte

Par exemple :

- véhicule ;
- voyage ;
- chauffeur ;
- passager.

---

## RM-171 — Un incident important doit être historisé

Informations possibles :

- date ;
- heure ;
- lieu ;
- description ;
- responsable ;
- statut ;
- résolution.

---

# 20. 📊 Règles reporting

## RM-180 — Les indicateurs doivent être calculés à partir des données opérationnelles

Les statistiques ne doivent pas être saisies manuellement.

---

## RM-181 — Les indicateurs doivent respecter une définition stable

Exemple :

> « Taux de remplissage »

doit avoir une définition précise et identique dans tous les rapports.

---

## RM-182 — Les données historiques doivent rester cohérentes

Une modification d'un tarif actuel ne doit pas modifier artificiellement les revenus historiques.

---

# 21. 🧾 Règles audit / traçabilité

Les actions suivantes devraient être journalisées :

- connexion ;
- modification d'un tarif ;
- annulation billet ;
- remboursement ;
- modification d'un voyage ;
- changement de véhicule ;
- changement de chauffeur ;
- clôture caisse ;
- modification financière ;
- modification des permissions.

Le journal doit idéalement conserver :

```text
👤 Qui ?
🕐 Quand ?
🎯 Quelle action ?
📦 Sur quel objet ?
🔄 Avant ?
➡️ Après ?
```

---

# 22. 🔄 Matrice simplifiée des règles critiques

| Domaine | Règle critique | Priorité |
|---|---|---|
| 🔐 Sécurité | Isolation des entreprises | 🔴 Critique |
| 🎫 Billetterie | Un siège ne peut être vendu deux fois | 🔴 Critique |
| 💰 Finance | Une vente doit être traçable financièrement | 🔴 Critique |
| 🚌 Flotte | Un véhicule indisponible ne peut être affecté | 🔴 Critique |
| 👨‍✈️ Chauffeurs | Pas de conflit d'affectation | 🔴 Critique |
| 🛣️ Voyages | Transitions de statut contrôlées | 🔴 Critique |
| 🏦 Caisse | Une caisse clôturée est protégée | 🔴 Critique |
| ❌ Annulation | Historique obligatoire | 🟠 Haute |
| 🔧 Maintenance | Historique véhicule | 🟠 Haute |
| 📦 Colis | Traçabilité du colis | 🟠 Haute |
| 📊 Reporting | Définitions stables | 🟠 Haute |
| 🧾 Audit | Actions sensibles journalisées | 🟠 Haute |

---

# 23. 🧪 Règles et tests

Chaque règle métier importante devra être transformée en scénario de test.

### Exemple — double vente

**Étant donné :**

> Le siège 12 est déjà vendu pour le voyage V001.

**Quand :**

> Un agent tente de vendre le siège 12 pour V001.

**Alors :**

> La vente doit être refusée.

---

### Exemple — véhicule indisponible

**Étant donné :**

> BUS-023 est en maintenance.

**Quand :**

> Un responsable tente de l'affecter au voyage V001.

**Alors :**

> L'affectation doit être refusée.

---

### Exemple — billet annulé

**Étant donné :**

> Le billet B001 est annulé.

**Quand :**

> Un contrôleur tente de le valider à l'embarquement.

**Alors :**

> L'embarquement doit être refusé.

---

# 24. ❓ Règles restant à décider

Certaines règles ne doivent pas être inventées par les développeurs.

Elles devront être décidées collectivement sur la base du fonctionnement réel des entreprises ciblées.

### Billetterie

- durée d'une réservation ;
- politique de remboursement ;
- frais d'annulation ;
- changement de voyage ;
- changement de siège ;
- vente sans identité complète ;
- règles pour les enfants ou tarifs spéciaux.

### Exploitation

- délai minimal avant départ ;
- conditions d'ouverture des ventes ;
- conditions d'annulation d'un voyage ;
- gestion des retards ;
- remplacement de véhicule.

### Finance

- gestion des commissions ;
- modes de paiement ;
- règles de caisse ;
- fréquence des versements ;
- gestion des écarts.

### Colis

- tarification ;
- poids maximum ;
- objets interdits ;
- responsabilité ;
- réclamations ;
- délais de retrait.

Ces décisions seront prises après validation auprès d'utilisateurs réels du secteur.

---

# 25. 🧠 Principe important : ne pas inventer le métier

Les règles métier doivent autant que possible être basées sur :

1. 📋 les pratiques réelles des entreprises ciblées ;
2. 👥 les entretiens avec des professionnels ;
3. 🧪 les tests du prototype ;
4. 📚 les contraintes réglementaires applicables ;
5. 🎯 les objectifs commerciaux du produit.

Le développeur ne doit pas décider seul d'une règle qui a un impact métier ou financier.

> **Le code implémente la règle ; il ne doit pas inventer la règle.**

---

# 26. 🎯 Conclusion

Les règles métier constituent le contrat fonctionnel entre le métier et le logiciel.

Elles permettront de répondre à une question essentielle :

> **« Que doit exactement faire le système dans chaque situation ? »**

Elles serviront directement à :

- concevoir la base de données ;
- concevoir les services ;
- définir les permissions ;
- écrire les tests ;
- développer les interfaces ;
- gérer les exceptions ;
- contrôler la cohérence des données.

---

## 📌 Prochaine étape

**Document 06 — Définition du MVP**

Nous allons maintenant réduire toute cette vision à un premier produit réellement développable et commercialisable.

Nous définirons :

- 🟢 ce qui est indispensable ;
- 🟡 ce qui est important mais peut attendre ;
- 🔵 ce qui appartient à la V2 ;
- 🔴 ce qui est explicitement hors périmètre ;
- 👥 les fonctionnalités par rôle ;
- 🗓️ les étapes de développement ;
- 🎯 les critères permettant de considérer le MVP comme terminé.

L'objectif sera d'éviter le piège classique :

> **vouloir construire tout l'ERP avant d'avoir validé le premier produit.**
