# SPEC-002-contenu-pedagogique

**Intent parent** : INTENT-001
**Auteur** : Steeve Evers
**Date** : 2026-03-26
**Statut** : ready
**SQS** : 5/5 (Gate ouverte 2026-03-26 — 2e passage après remédiation)

---

## 1. Contexte

Le parcours de formation SDD Mode doit couvrir les 6 étapes du cycle. Chaque étape fait l'objet d'une page MDX de consultation guidée expliquant le quoi, le pourquoi, les artefacts produits, et un exemple concret. Pas d'exercices interactifs en v1.

## 2. Comportement Attendu

### Input
- Documentation officielle du framework SDD Mode (CLAUDE.md, AGENT-GUIDE template, SPEC template)
- Vocabulaire métier défini dans l'AGENT-GUIDE

### Processing
Chaque page MDX suit une structure pédagogique uniforme :
1. **En bref** — Résumé de l'étape en 2-3 phrases
2. **Pourquoi cette étape ?** — Justification dans le cycle (qu'est-ce qui casse si on la saute ?)
3. **Ce que tu produis** — Artefact(s) résultant (avec lien vers le template)
4. **Exemple concret** — Un exemple réaliste et commenté d'artefact bien rédigé (projet fil rouge)
5. **Erreurs fréquentes** — 2-3 anti-patterns courants
6. **Checklist** — Points à vérifier avant de passer à l'étape suivante

### Projet fil rouge pour les exemples
Tous les exemples concrets utilisent le même projet fictif : **RecettesApp** — une application web de gestion de recettes de cuisine. Ce domaine est choisi pour sa simplicité et son universalité.

Éléments récurrents dans les exemples :
- **Intent exemple** : "Permettre à un cuisinier amateur de sauvegarder ses recettes préférées"
- **SPEC exemple** : "SPEC-001 — Formulaire de création de recette" (champs : titre, ingrédients, étapes, temps de préparation)
- **Persona** : Marie, développeuse front-end qui découvre SDD Mode sur son premier projet en tant que PE
- **Stack fictive** : Next.js + PostgreSQL (suffisamment classique pour être compris par tout PE)

Chaque page MDX illustre une étape du cycle SDD appliquée à RecettesApp, de l'Intent initial jusqu'au Drift Lock final.

### Output
6 fichiers MDX dans `src/content/parcours/` :

| Fichier | Étape | Artefact produit |
|---------|-------|-----------------|
| `01-intent-statement.mdx` | Intent Statement | `INTENT-NNN.md` |
| `02-spec.mdx` | Rédaction de SPEC | `SPEC-NNN.md` |
| `03-execution-gate.mdx` | Execution Gate | Score SQS ≥ 4/5 |
| `04-execution-agent.mdx` | Exécution Agent | Code implémenté |
| `05-validation.mdx` | Validation | Code review + tests |
| `06-drift-lock.mdx` | Drift Lock | SPEC synchronisée + PR mergée |

### Cas limites
- Le contenu du framework SDD évolue → le critère de drift de l'INTENT-001 s'applique : le contenu du site doit rester cohérent avec le framework officiel
- Un terme métier n'a pas d'équivalent simple → utiliser le glossaire de l'AGENT-GUIDE et ajouter une note explicative inline
- L'exemple concret est trop long (>50 lignes) → le tronquer avec un commentaire "extrait" et lier le template complet

## 3. Critères d'Acceptation

- [ ] 6 fichiers MDX présents dans `src/content/parcours/`
- [ ] Chaque fichier suit la structure pédagogique à 6 sections (En bref, Pourquoi, Ce que tu produis, Exemple, Erreurs fréquentes, Checklist)
- [ ] Chaque fichier a un frontmatter valide (`title`, `description`, `order`)
- [ ] Aucun terme de la colonne "Terme à éviter" du glossaire AGENT-GUIDE n'apparaît dans les fichiers MDX (vérifiable par grep)
- [ ] Les exemples concrets utilisent tous le projet fil rouge RecettesApp et sont cohérents entre eux
- [ ] Aucun contenu ne contredit la hiérarchie documentaire AIAD
- [ ] `npm run build` sans erreur

## 4. Interface / API

### Frontmatter MDX (exemple)
```yaml
---
title: "Étape 1 — L'Intent Statement"
description: "Capturer le POURQUOI avant de spécifier le COMMENT"
order: 1
---
```

### Structure d'une page type
```mdx
---
title: "Étape N — [Nom]"
description: "[Description courte]"
order: N
---

## En bref
[2-3 phrases]

## Pourquoi cette étape ?
[Justification — qu'est-ce qui casse si on la saute ?]

## Ce que tu produis
[Artefact + lien template]

## Exemple concret
[Exemple réaliste commenté — projet fil rouge]

## Erreurs fréquentes
- **[Erreur 1]** : [Explication + correction]
- **[Erreur 2]** : [Explication + correction]

## Checklist avant de passer à la suite
- [ ] [Point de vérification 1]
- [ ] [Point de vérification 2]
```

## 5. Dépendances

- **SPEC-001** : Le scaffold Astro et la content collection `parcours` doivent exister
- Documentation officielle SDD Mode : source de vérité pour le contenu

## 6. Estimation Context Budget

**Contexte à injecter pour cette tâche :**
- AGENT-GUIDE (condensé) : ~500 tokens
- Cette SPEC : ~500 tokens
- CLAUDE.md (cycle SDD, commandes) : ~1 000 tokens
- Templates d'artefacts (Intent, SPEC) : ~500 tokens
- **Total estimé** : ~2 500 tokens

## 7. Definition of Output Done (DoOD)

- [ ] 6 fichiers MDX rédigés et build passing
- [ ] Relecture humaine : conformité du contenu avec la documentation officielle SDD Mode (CLAUDE.md, templates)
- [ ] SPEC mise à jour si écart (Drift Lock)
- [ ] Gouvernance vérifiée : RGAA (contenu structuré avec headings hiérarchiques, texte alternatif si images), RGESN (pas de médias lourds)
