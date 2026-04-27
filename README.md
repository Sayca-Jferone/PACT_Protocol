<h1 align="center" class="heading-element" dir="auto">Sayca Labs | PACT v0.50</h1>

*Protocol for sys Architectures by Contracts and Typing*

### PACT - Protocole d'Architecture système par Contrats Typés

#### Auteur : Sayca J.Ferone

#### Version : v0.50 - 27/04/2026

**Objet :** Séparation du développement logiciel en 3 plans épistémiques.

# Objectif

1. **Réduire le temps de travail** et la fatigue du développeur, en réduisant l'entropie entre Mental->Concept->Produit.
2. **Renforcer la cohérence** du système avec une priorisation de "debug" en entonnoir : I->II->III
3. **Référentiel lisible permanent** pour les changements de features / souhaits-clients
- **Utile pour tout le Monde:** Développeurs Humains, ou artificiels. Seuls, ou en équipe. Junior, ou Senior.


Le code devient une traduction d'un contrat existant, pas une exploration in-code.

| Phase | Produit | Nature | Cadre | Utilité |
|---|---|---|---|---|
| I | `1-SYNTHESIS.md` | Explication corrigée du contexte avec to-do list intégrale | Abstrait `φ` | Référence de confiance selon versioning |
| II | `2-SYSTEM.md` | Architecture intermédiaire entre le mental et le livrable | Formel `σ` | Division du temps de conception du système |
| III | `Code` & `3-DEVNOTES.md` | Système consommable et notes in-dev optionnelles | Concret `γ` | Traduction du système en code informatique |


---

### OPTIONS AVANCÉES

<details>
<summary>📈 Unités du Système Épistémique PACT</summary>

**Ne concerne pas le développement logiciel classique.**

Sert aux *vecteurs de perfectionnement cognitif* des développeurs (humains et artificiels) :

| Unité | Symbole | Domaine mathématique | Forme | Nature |
|---|---|---|---|---|
| Précision conceptuelle | φ | [0, 1] | Scalaire continu | Mesure l'avancement Phase I -> de l'idée au **concept** à faible granularité |
| Cohérence architecturale | σ | [0, 1] | Scalaire continu | Mesure l'avancement Phase II -> du **concept** au **contrat** BIOPGE complet |
| Complétude d'implémentation | γ | [0, 1] | Scalaire continu | Mesure l'avancement Phase III -> du **contrat** au **produit** fonctionnel testé |
| État épistémique du projet | Ψ | [0,1]³ | Vecteur (φ, σ, γ) | Représentation interne honnête -> position dans l'espace PACT |
| Variation épistémique | ΔΨ | ℝ³ | Vecteur différentiel | Gain de précision entre deux états Ψ(t₁) → Ψ(t₂) |
| Profil de domaine | 𝒫 | ℕ³ sous contrainte | Triplet de poids (Wφ, Wσ, Wγ) avec Wφ+Wσ+Wγ = 100 | Paramètre domaine-dépendant -> encode où se concentre le risque |
| Indice de Granularité | IG | [0, 100] | Scalaire -> projection de Ψ pondérée par 𝒫 | Interface publique lisible -> `(φ·Wφ + σ·Wσ + γ·Wγ)` |

**Contrainte d'ordre partiel sur Ψ** (invariant PACT) :

```
 φ > 0  ->  lancement  (I : on synthétise le contexte  ->  donne le Concept)
 σ > 0  ->  φ ≥ θ₁     (II : on ne contracte pas sans concept  ->  donne le Contrat)
 γ > 0  ->  σ ≥ θ₂     (III : on n'implémente pas sans contrat  ->  donne le Livrable)
```

Références génériques de PACT v0.5, à varier possiblement selon les résultats du domaine d'ingénierie concerné.

> θ₁ = 0.7 comme seuil Phase I -> II : concept suffisant, rétroactivité II -> I tolérée et bon marché

> θ₂ = 0.9 comme seuil Phase II -> III : contrat quasi-parfait exigé, rétroactivité III -> II coûteuse

> Validé (27/04/2026) : Conclusion d'audit par Sayca-JFerone + Claude Sonnet 4.6 sur tous les programmes de Sayca (plus solo/2-peers subjects done from 42next)

Extrait du Corpus SaycaLabs `BIGDATA_TOOLS -> Inventaire des unités de mesure épistémique`

</details>

---

## Les 3 phases

### Phase I : Synthesis 📝

Lire le problème. Le décomposer. Lister :

- **1. Formats (F-XX)** : langage, normes, contraintes, livrables.
- **2. Mandatory (M-XX)** : ce que le sujet exige explicitement.
- **3. Bonus (B-XX)** : ce qui est optionnel.
- **4. Open-To (O-XX)** : les choix et libertés laissés au développeur.
- **5. Ambiguities (A-XX)** : les zones grises, à résoudre ou assumer.

Sortie : `1-SYNTHESIS.md`. Une to-do list avec cases à cocher, dense, lisible en 60 secondes.

Si le client change souvent d'avis, si la version d'un système par conception doit changer, ça se fait ici. Ensuite, on propage sur Phase II, puis on implémente en Phase III.

### Phase II : System 📐

**Avant d'écrire la première ligne de code**, chaque fichier *(ou unité logique)* reçoit un **bloc BIOPGE** :

```markdown
### `src/file.format`

> Covers: M-02, B-14, O-2	# (Option: Si `2-SYSTEM.md` réfère directement les points de `1-SYNTHESIS.md`. Demande plus d'efforts.)

> Author: jferone [resp], patoche [contrib], ...	# (Option: S'il s'agit d'un projet multi-devs)

| Champ | Contenu |
|---|---|
| **Boundary** | Nom du fichier, auteur, ce que cette unité possède (et ce qu'elle ne possède pas). |
| **Inputs** | Paramètres typés. Pas d'ambiguïté. |
| **Outputs** | Retours typés ou effets de bord. |
| **Process** | Étapes numérotées. Pas de prose. |
| **Guarantees** | Invariants vérifiables après exécution. |
| **Errors** | Chaque mode d'échec, son déclencheur, son comportement. |
```

Sortie : `2-SYSTEM.md`. Un tableau par bloc fichier/logique, **zéro prose entre les blocs**.

**Règle dure : zéro code écrit sur un fichier dont le bloc n'est pas validé.**

### Phase III : Implementation 🏗️

Le contrat existe. Il suffit de le traduire en code, dans votre syntaxe et dans vos normes.

Pendant l'écriture, deux types d'erreurs possibles :

1. **Erreur syntaxique** *(typo, import, off-by-one, non-native in language)* : on corrige sur place.
2. **Erreur logique** *(flux incorrect, garantie impossible, cas oublié)* : on retourne en Phase II, on amende le bloc, on re-valide.

Cette distinction est la règle critique de Phase III. La confondre, c'est accumuler de la dette invisible.

Sortie : un produit quelconque, du code auditable, où chaque fonction trace jusqu'à un bloc BIOPGE Phase II, la synthèse Phase I est cochée en intégralité.

`3-DEVNOTES.md` est le journal optionnel du développeur, mis à jour au fil de l'eau : décisions non triviales, ressources consultées, retroactivités. Il complète les commits git, il ne les remplace pas.

---

## Le triplet livré

À la fin d'un projet construit/modifié sous PACT, le dépôt contient par exemple :

```
[livrables]
src/
	[code source]
doc/
	1-SYNTHESIS.md			# le problème lisible en 60 secondes
	2-SYSTEM.md				# le contrat d'architecture lisible par tout auditeur
	3-DEVNOTES.md			# le journal du développeur, feedbacks optionnels de code/debug
	[ex: schéma Phase II, ...]
```

Ce dépôt de base suffit à lui-seul pour la rédaction d'un `README.md` final cohérent (utile pour les rendus inspectés ou tout dépôt public).

---

## Rétroactivité

Chaque phase peut renvoyer à la précédente :

```
Phase I  (Synthesis)     <--- clarifier le sujet ---+
   |                                                |
   v                                                |
Phase II (System)        <----- erreur logique -----+
   |                                                |
   v                                                |
Phase III (Code & Debug) ------ erreur logique -----+
```

Retourner en arrière n'est pas un échec. C'est le protocole qui filtre une mauvaise compréhension ou une mauvaise architecture au stade le moins coûteux.

---

## Pour qui ?

- **Étudiants 42** : pour structurer un projet avant de coder, pour défendre proprement en soutenance, pour produire un rendu lisible.
- **Ingénieurs logiciels** : pour les projets en solo ou en petite équipe (1 à 5 personnes), où l'architecture émergente devient vite illisible.
- **Tout projet de conception** : le protocole est volontairement agnostique au langage et au domaine. Sa logique (problème, système, produit) tient pour de l'ingénierie globale autant que logicielle.

---

## Comment l'utiliser

1. Copier les templates de `1-SYNTHESIS.md` et `2-SYSTEM.md` à la racine du projet (ou dans `doc/`).
2. Faire la Phase I jusqu'à ce qu'aucune question ouverte ne puisse invalider l'architecture.
3. Remplir chaque bloc BIOPGE en Phase II. Valider avant tout code.
4. Coder en Phase III. Ne retourner en Phase II que sur erreur logique.

Les templates exacts et les règles de chaque phase sont dans les Claude AI Skills `pact-phase1`, `pact-phase2`, `pact-phase3` (utilisables avec un LLM compagnon) ou directement dans les sections du présent dépôt.

---

## Philosophie

> Que le code reflète le système. Pas l'inverse.

Un système qui ne peut pas être décrit en blocs BIOPGE avant son implémentation est un système qui n'est pas encore compris.

La compréhension est le prérequis. Le reste, c'est de la frappe.

---

## Roadmap

- [ ] Fork équipe 1 à 5 personnes
- [ ] Fork équipe 5 à 50 personnes
- [ ] Présentation amphi 42 Nice
- [ ] Fork ingénierie globale (résolution de problèmes hors domaine logiciel)

---

*Protocole de terrain, pas théorie académique. Testé sur projets réels, raffiné sous contraintes réelles.*
