# AGENT-GUIDE — SDD Training

> Ce fichier est le **contexte permanent** de l'agent IA.
> Il est injecté dans CHAQUE session de développement.
> Le maintenir à jour est une responsabilité de l'Agents Engineer (AE).
> Framework : AIAD SDD Mode v1.3

---

## IDENTITÉ DU PROJET

**Nom** : SDD Training
**Description** : Site web interactif de formation au cycle SDD Mode pour Product Engineers. Parcours guidé avec exercices pratiques.
**Domaine métier** : Formation / Méthodologie de développement assisté par IA
**Mission** : Permettre à un Product Engineer de compléter un cycle SDD complet en autonomie et d'être opérationnel sur un vrai projet.

---

## DOCUMENTATION DE RÉFÉRENCE

| Document | Chemin | Mode d'injection |
|----------|--------|-----------------|
| PRD | @.aiad/PRD.md | Cadrage uniquement |
| Architecture | @.aiad/ARCHITECTURE.md | Condensé permanent |
| SPEC active | @.aiad/specs/[SPEC-XXX].md | Par tâche uniquement |
| Index SPECs | @.aiad/specs/_index.md | Planification |
| Gouvernance | @.aiad/gouvernance/ | Permanent (Tier 1, veto) |

---

## STACK TECHNIQUE (Référence Rapide)

- **Framework** : Astro 5.x (SSG) + MDX pour le contenu pédagogique
- **Interactivité** : React 19 en îlots Astro (`client:visible`)
- **Language** : TypeScript strict
- **Styling** : CSS natif avec custom properties (`--sdd-*`)
- **Tests** : Vitest (unit) + Playwright (E2E / accessibilité)
- **CI/CD** : GitHub Actions → GitHub Pages
- **Stockage client** : localStorage (progression)
- **Pas de** : backend, base de données, authentification, framework CSS

---

## RÈGLES ABSOLUES

### TOUJOURS
- Utiliser du HTML sémantique (`<main>`, `<article>`, `<nav>`, `<section>`)
- Tester l'accessibilité avec Lighthouse (score 100 requis)
- Utiliser `client:visible` pour les îlots React (pas `client:load`)
- Synchroniser SPEC + code dans la même PR (Drift Lock)
- Ajouter un test pour chaque bug fix
- Valider les entrées utilisateur côté client dans les exercices

### JAMAIS
- Installer un framework CSS (Tailwind, Bootstrap, etc.) — CSS natif uniquement
- Utiliser `client:load` sauf nécessité technique documentée
- Déposer de cookies sans consentement explicite
- Charger des ressources externes sans justification (fonts Google, CDN tiers)
- Committer sans lint passing
- Livrer sans mettre à jour la SPEC correspondante

---

## CONVENTIONS DE CODE

### Nommage
- **Composants React** : PascalCase → `ExerciseIntent.tsx`
- **Composants Astro** : PascalCase → `ProgressBar.astro`
- **Fichiers contenu MDX** : kebab-case numéroté → `01-intent-statement.mdx`
- **Variables CSS** : `--sdd-[catégorie]-[propriété]` → `--sdd-color-primary`
- **Scripts** : camelCase → `progress.ts`
- **Types** : PascalCase, suffixe si ambigu → `StepProgress`, `ExerciseResult`

### Structure d'un composant React (exercice)
```tsx
import { useState } from 'react';
import type { ExerciseResult } from '../../types';

interface Props {
  stepId: string;
}

export default function ExerciseName({ stepId }: Props) {
  const [result, setResult] = useState<ExerciseResult | null>(null);

  return (
    <div role="region" aria-label="Exercice : [nom]">
      {/* Contenu de l'exercice */}
    </div>
  );
}
```

### Structure d'une page MDX (étape du parcours)
```mdx
---
title: "Étape 1 — L'Intent Statement"
description: "Apprendre à capturer l'intention humaine"
order: 1
---

import ExerciseIntent from '../../components/exercises/ExerciseIntent';

## Concept

[Explication de l'étape]

## Exemple

[Exemple concret d'artefact]

## Exercice

<ExerciseIntent client:visible stepId="01-intent" />
```

### Gestion des erreurs
```typescript
// Toujours graceful degradation — jamais de crash visible
try {
  const data = JSON.parse(localStorage.getItem(key) ?? '{}');
} catch {
  return defaultValue;
}
```

---

## VOCABULAIRE MÉTIER

| Terme métier | Définition | Terme à éviter |
|--------------|------------|----------------|
| **Intent Statement** | Déclaration formelle du POURQUOI d'une fonctionnalité | "ticket", "tâche" |
| **SPEC** | Spécification technique atomique liée à un Intent | "user story" (trop vague) |
| **Execution Gate** | Point de contrôle qualité avant exécution agent (SQS ≥ 4/5) | "review" |
| **SQS** | Spec Quality Score — note sur 5 à la Gate | "score" (ambigu) |
| **Drift Lock** | Synchronisation SPEC + code dans la même PR | "mise à jour doc" |
| **Drift Check** | Vérification que artefacts et code sont synchronisés | "audit" |
| **Îlot** | Composant React hydraté côté client dans une page Astro statique | "widget" |
| **Product Engineer** | Rôle AIAD : gardien de l'intention, orchestrateur d'agents IA | "développeur" |
| **Parcours** | Séquence d'étapes de formation couvrant le cycle SDD complet | "cours", "tutoriel" |

---

## PATTERNS DE DÉVELOPPEMENT

### Pattern 1 — Contenu MDX + Îlot interactif
Chaque étape du parcours est un fichier MDX qui combine explication statique (HTML pur, SEO-friendly, accessible) et exercice interactif (composant React hydraté à la visibilité).

### Pattern 2 — Progression en localStorage
La progression est stockée côté client. Chaque exercice complété appelle `markStepComplete(stepId)`. La progression est lue au chargement pour afficher l'état dans la barre de progression.

---

## ANTI-PATTERNS

| Anti-pattern | Pourquoi éviter | Alternative |
|--------------|-----------------|-------------|
| Framework CSS | Poids inutile, dépendance, contraire RGESN | CSS natif + custom properties |
| `client:load` systématique | Charge le JS immédiatement, dégrade la performance | `client:visible` (lazy) |
| Fonts externes (Google Fonts) | Requête tierce, privacy, latence | System font stack |
| localStorage sans try/catch | Peut crasher (mode privé Safari, quota) | Toujours wrapper dans try/catch |
| Contenu pédagogique dans les composants React | Non indexable, non accessible sans JS | Contenu dans MDX, interactivité dans les îlots |

---

## LESSONS LEARNED

> Section mise à jour à chaque fin d'itération (commande `/aiad-retro`).
> Documentez ici les erreurs récurrentes de l'agent ET les corrections appliquées.

| Date | Erreur agent | Correction | Impact |
|------|-------------|------------|--------|
| | | | |

---

## HUMAN LEARNINGS

> Section v1.1 — Documentez ici les écarts entre l'intention humaine et la livraison.
> Ces learnings ne sont PAS des erreurs de l'agent — ce sont des défaillances de l'expression humaine.

| Date | Intention exprimée | Résultat obtenu | Apprentissage |
|------|--------------------|-----------------|---------------|
| | | | |
