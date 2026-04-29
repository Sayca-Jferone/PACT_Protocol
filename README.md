## Pipeline d'Architecture par Conception Tripartite (PACT)

#### Auteur : Sayca (Jferone)

#### Version : v0.51 (27/04/2026 - Usage perso intensif)

### Utilisation : Construire des logiciels sur des problèmes/sujets en apprentissage ou non-maîtrisés

## Pour qui ?

| | |
|---|---|
| Étudiants 42 | **VOTRE architecture**, pas celle d'un LLM lambda : c'est notre force, pas de vibe code, en vitesse accélérée. La densité des Phases I/II dépend de la complexité de votre sujet. |
| Ingénieurs logiciels | Au-delà de vos maîtrises et expériences, vos projets en solo ou en petite équipe (1 à 5 pax) profitent d'un pipeline qui contracte I/II/III en une **architecture émergente, héritable et lisible**. |
| Conception générale | Ce pipeline est volontairement agnostique au langage et au domaine. Sa logique sert pour de l'ingénierie globale, à expérimenter dans d'autres domaines que le logiciel. |

# Objectif

**Sur quoi ?** -> **Tous vos sujets. Pas d'over-engineering :** Un petit programme demandera quelques minutes de Phases I & II. Pour les plus complexes, on parle de jours, semaines ou mois.

**Pourquoi ?** -> J'estime que ma mémoire de travail limitée m'impose de déconstruire les problèmes et détecter les incohérences des sujets avant de les découvrir le nez dans le code.

1. **Réduire le temps de travail**, la fatigue du développeur par l'entropie `Mental<->Concept<->Produit`.
2. **Renforcer la cohérence** du système avec une remontée des debugs en entonnoir `I<-II<-III`.
3. **Référentiel lisible** pour les changements de features / souhaits-clients en cours de dev.

Le code devient une traduction d'un contrat existant, et non une exploration in-code comme je le faisais autrefois.

| Phase | Produit | Nature | Cadre | Utilité |
|---|---|---|---|---|
| O | Sujet, Marché, Client | Quand on vous fournit un sujet, passez le au crible dans la Phase I afin d'épurer la requête. | Abstrait | Besoin brut de solution logicielle |
| I | `1-SYNTHESIS.md` | La to-do-list, le comportement du produit. Explication corrigée du contexte avec to-do list intégrale | Abstrait | Besoin corrigé, clair, épuré, avant de commencer à produire |
| II | `2-SYSTEM.md` | Architecture intermédiaire entre le mental et le livrable | Hybride | Division du temps de conception du système |
| III | `Code` & `3-DEVNOTES.md` | Système consommable et notes in-dev optionnelles | Concret | Traduction du système en code informatique |

---

## Les 3 phases

### Phase I : Synthesis 📝

Lire le problème, le décomposer, et lister :

- **1. Formats (F-XX)** : langage, normes, contraintes, livrables.
- **2. Mandatory (M-XX)** : ce que le sujet exige explicitement.
- **3. Bonus (B-XX)** : ce qui est optionnel.
- **4. Open-To (O-XX)** : les choix et libertés laissés au développeur.
- **5. Ambiguities (A-XX)** : les zones grises, à résoudre ou assumer.

Sortie : `1-SYNTHESIS.md`. Une to-do list avec cases à cocher, dense, lisible en 60 secondes.

Si le client change souvent d'avis, si la version d'un système par conception doit changer, ça se fait ici. Ensuite, on propage sur Phase II, puis on implémente en Phase III.

### Phase II : System 📐

**Avant d'écrire la première ligne de code**, chaque fichier reçoit un **bloc BIOPGE** (Boundary-Inputs-Outputs-Process-Guarantees-Errors)

```markdown
### `src/file.format`

> Covers: M-02, B-14, O-2   ((OPTIONNEL))

> Author: jferone [resp], patoche [contrib]   ((OPTIONNEL))

| Champ | Contenu |
|---|---|
| **Boundary** | Nom du fichier, auteur, ce que cette unité possède (et ce qu'elle ne possède pas). |
| **Inputs** | Paramètres typés. Pas d'ambiguïté. |
| **Outputs** | Retours typés ou effets de bord. |
| **Process** | Étapes numérotées ou en prose par séquences fléches. |
| **Guarantees** | Invariants vérifiables après exécution. |
| **Errors** | Chaque mode d'échec, son déclencheur, son comportement. |
```

Sortie : `2-SYSTEM.md`. Un tableau de cette forme par bloc-fichier, possible de l'étendre en sous-parties selon votre volonté, si utile.

**Règle : pas de code tant que le bloc BIOPGE concerné n'est pas défini. Même défaillant, on peut sortir du code pour corriger la logique BIOPGE, et revenir.**

#### Exemple tiré d'un projet 42next - A-Maze-ing (2-peers)

```markdown
### /renderer.py
|  |  |
|---|---|
| Boundary | `renderer.py` - jferone [resp], elocufie [contrib] - Live UX & raw maze translator to ASCII/ANSI.
| Inputs | `grid: list[list[int]]` ; `path: list[tuple[int,int]] \| None` ; `entry: tuple[int,int]` ; `exit_: tuple[int,int]` ; `config: dict[str, Any]`
| Outputs | stdout ANSI terminal rendering
| Process | iterate cells -> draw North+West walls per cell via ANSI codes ; mark entry cell and exit cell with distinct symbols ; if `path` not None: highlight path cells ; numbered menu: 1 re-gen ; 2 show/hide path ; 3 rotate wall colors ; 4 quit
| Guarantees | display consistent with grid ; shared walls drawn exactly once ; entry and exit always visually distinct ; all 4 menu interactions functional ; `path=None` renders maze without path overlay, no crash
| Errors | terminal too small -> warning message, no crash
```

**ATTENTION:** Si un bug conceptuel est relevé dans l'un des blocs, on retourne en Phase II. Pire s'il s'agit de la forme du programme entier, on court-circuite en Phase I, puis Phase II, puis Phase III.

---

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

Phase O  (Problème/Demande) <--- clarifier le sujet ---+
   |                                                   |
   v                                                   |
Phase I  (Synthesis)        <--- clarifier le sujet ---+
   |                                                   |
   v                                                   |
Phase II (System)           <----- erreur logique -----+
   |                                                   |
   v                                                   |
Phase III (Code & Debug) --------- erreur logique -----+
```

Retourner en arrière n'est pas un échec. C'est le protocole qui filtre une mauvaise compréhension ou une mauvaise architecture au stade le moins coûteux.

---

## Comment l'utiliser

1. Copier les templates de `1-SYNTHESIS.md` et `2-SYSTEM.md` à la racine du projet (ou dans `docs/`).
2. Faire la Phase I jusqu'à ce qu'aucune question ouverte ne puisse invalider l'architecture.
3. Remplir chaque bloc BIOPGE en Phase II. Valider avant tout code.
4. Coder en Phase III. Ne retourner en Phase II que sur erreur logique.

Les templates exacts et les règles de chaque phase sont dans les Claude AI Skills `pact-phase1`, `pact-phase2`, `pact-phase3` (utilisables avec un LLM compagnon, applicable à la lettre par un humain) ou directement dans les sections du présent dépôt.

---

### Ancêtres et cousins

PACT converge avec trois standards/méthodes déjà établies, dont j'ai découvert la proximité pendant l'audit technique du protocole :

| Référence | Domaine | Ressemblance avec PACT | Différence clé |
|---|---|---|---|
| **Design by Contract** (Meyer, 1986) | Génie logiciel formel | **Bloc BIOPGE = contrat au sens Meyer** : pre/post/invariant/error mapping direct | DbC opère au niveau fonction/classe ; PACT systématise au niveau fichier/module + impose une phase conceptuelle en amont (Phase I) |
| **arc42** (Starke & Hruschka, 2005) | Documentation d'architecture | **Format markdown structuré** ; compartiments optionnels ; pragmatisme "tell, don't show" | arc42 couvre 12 aspects d'architecture (contexte, décisions, qualité, risques) ; PACT restreint à contrats de fichier → code et synthèse conceptuelle |
| **Spec-Driven Development** (GitHub Spec-Kit, 2024-2025) | LLM-assisted coding | **Séparation spec → plan → code** en markdown ; source de vérité = spécification ; phase de synthèse en amont | SDD cible agents LLM + workflow itératif ; PACT est agnostique agent et impose contrainte d'ordre strict (I → II → III) |

La convergence est réelle. Les racines : DbC (contrats), arc42 (format pratique), SDD (phase conceptuelle → exécution).


---

### OPTIONS AVANCÉES

<details>
<summary>Référence: Unités du Système épistémique PACT</summary>

**Ne concerne pas le développement logiciel classique.** -> Destiné uniquement aux modèles de performances cognitives et de meta-engineering sur systèmes en développement et/ou existants.

| Unité | Indice | Domaine mathématique | Forme | Nature |
|---|---|---|---|---|
| Précision de concept | φ | [0, 1] | Scalaire continu | Mesure l'avancement Phase I -> de l'idée au **concept** à faible granularité |
| Cohérence d'architecture | σ | [0, 1] | Scalaire continu | Mesure l'avancement Phase II -> du **concept** au **contrat** BIOPGE complet |
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

> Validé (27/04/2026) : Conclusion d'audit par Sayca + Claude Sonnet 4.6 & Opus 4.7 sur tous les programmes de Sayca (plus solo/2-peers subjects done from 42next)
</details>

---

*Protocole de terrain, pas de théorie académique. Testé sur projets réels, raffiné sous contraintes réelles.*
