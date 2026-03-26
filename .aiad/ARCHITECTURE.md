# ARCHITECTURE

> Ce fichier est le contexte technique permanent. Un résumé condensé (max 500 tokens) est injecté dans chaque session agent.
> Mainteneur : Tech Lead

## 1. Principes Architecturaux

1. **Sobriété maximale** : Zéro backend, zéro dépendance inutile. Site statique pur.
2. **Accessibilité native** : HTML sémantique, RGAA 4.1 / WCAG 2.1 AA dès la conception.
3. **Privacy by Design** : Aucun cookie, aucun tracking sans consentement explicite. Pas de données personnelles collectées.
4. **Progressive Enhancement** : Le contenu est accessible sans JavaScript. Les interactions enrichissent l'expérience sans la conditionner.

## 2. Vue d'Ensemble

```
Navigateur (client uniquement)
  │
  ├── Astro (SSG) → HTML statique pré-rendu
  │     ├── Pages MDX (contenu pédagogique)
  │     └── Composants interactifs (îlots React)
  │
  ├── localStorage → Progression utilisateur
  │
  └── GitHub Pages → Hébergement statique
```

## 3. Stack Technique

| Composant | Technologie | Justification |
|-----------|-------------|---------------|
| **Runtime** | Node.js 20 LTS | Standard, support long terme |
| **Framework** | Astro 5.x | SSG performant, îlots d'interactivité, idéal pour contenu |
| **Contenu** | MDX | Markdown enrichi avec composants interactifs inline |
| **Interactivité** | React 19 (îlots Astro) | Composants interactifs pour les exercices uniquement |
| **Styling** | CSS natif (custom properties) | Zéro dépendance, léger, conforme RGESN |
| **Language** | TypeScript strict | Typage fort, maintenabilité |
| **Tests** | Vitest + Playwright | Unitaires + E2E accessibilité |
| **Hébergement** | GitHub Pages | Gratuit, CI/CD intégré via GitHub Actions |
| **CI/CD** | GitHub Actions | Build + déploiement automatique sur push main |

## 4. Structure du Projet

```
.
├── .aiad/                  ← Artefacts SDD Mode
├── src/
│   ├── content/
│   │   └── parcours/       ← Contenu pédagogique MDX (1 fichier par étape)
│   ├── components/
│   │   ├── exercises/      ← Composants React interactifs (exercices)
│   │   ├── ui/             ← Composants UI réutilisables (Progress, Badge, etc.)
│   │   └── layout/         ← Composants de mise en page
│   ├── layouts/
│   │   └── TrainingLayout.astro
│   ├── pages/
│   │   ├── index.astro     ← Page d'accueil
│   │   └── parcours/       ← Pages générées depuis le contenu MDX
│   ├── scripts/
│   │   └── progress.ts     ← Gestion progression localStorage
│   └── styles/
│       └── global.css      ← Styles globaux, custom properties, tokens
├── public/
│   └── assets/             ← Images, exemples d'artefacts
├── tests/
│   ├── unit/               ← Tests Vitest
│   └── e2e/                ← Tests Playwright (accessibilité)
├── astro.config.mjs
├── tsconfig.json
├── package.json
└── CLAUDE.md
```

## 5. Conventions de Code

### Nommage
- **Composants** : PascalCase (`ExerciseIntent.tsx`, `ProgressBar.tsx`)
- **Fichiers contenu** : kebab-case numéroté (`01-intent-statement.mdx`, `02-spec.mdx`)
- **Variables CSS** : `--sdd-[catégorie]-[propriété]` (`--sdd-color-primary`, `--sdd-space-md`)
- **Scripts utilitaires** : camelCase (`progress.ts`, `consentManager.ts`)

### Formatting
- **Indentation** : 2 espaces
- **Line length** : max 100 chars
- **Quotes** : simples en JS/TS, doubles en HTML/JSX

### Imports
```typescript
// 1. Modules Node/npm
import { useState } from 'react';

// 2. Composants locaux
import { ProgressBar } from '../ui/ProgressBar';

// 3. Utilitaires locaux
import { getProgress } from '../../scripts/progress';

// 4. Types
import type { ExerciseResult } from '../../types';
```

## 6. Patterns Utilisés

### Îlot interactif (Astro Island)
```astro
---
// Dans un fichier .astro ou .mdx
import ExerciseIntent from '../components/exercises/ExerciseIntent';
---

<ExerciseIntent client:visible />
```
Le composant React est hydraté uniquement quand il entre dans le viewport. Le contenu statique autour reste du HTML pur.

### Gestion de progression (localStorage)
```typescript
const STORAGE_KEY = 'sdd-training-progress';

export function markStepComplete(stepId: string): void {
  const progress = getProgress();
  progress[stepId] = { completed: true, completedAt: Date.now() };
  localStorage.setItem(STORAGE_KEY, JSON.stringify(progress));
}
```

## 7. Gestion des Erreurs

```typescript
// Pattern : graceful degradation — jamais de crash visible
function getProgress(): Record<string, StepProgress> {
  try {
    return JSON.parse(localStorage.getItem(STORAGE_KEY) ?? '{}');
  } catch {
    return {};
  }
}
```

## 8. Sécurité

- **Authentification** : Aucune (site public)
- **Autorisation** : Aucune (pas de rôles)
- **Secrets** : Aucun secret côté client. Container ID GTM en variable d'environnement si analytics ajouté.
- **Input validation** : Pas d'input utilisateur persisté côté serveur. Validation côté client pour les exercices uniquement.
- **CSP** : Content Security Policy stricte via headers GitHub Pages

## 9. Performance (Budgets)

| Métrique | Budget | Monitoring |
|----------|--------|-----------|
| **Lighthouse Performance** | ≥ 95 | GitHub Actions (CI) |
| **Lighthouse Accessibility** | 100 | GitHub Actions (CI) |
| **First Contentful Paint** | < 1.5s | Lighthouse |
| **Total page weight** | < 200 KB (gzipped) | Build report |
| **JavaScript client** | < 50 KB (gzipped) | Build report |

## 10. ADRs (Architecture Decision Records)

> Les ADRs sont stockés dans `.aiad/adrs/` au format :
> `ADR-NNN-[titre].md`

[Aucun ADR pour l'instant — documentez chaque décision technique significative]

---

## Résumé Condensé (Context Budget — max 500 tokens)

Site statique de formation SDD Mode. Stack : Astro 5 SSG + MDX (contenu) + React 19 îlots (exercices interactifs). TypeScript strict, CSS natif. Hébergé sur GitHub Pages. Pas de backend, pas d'auth, pas de DB. Progression stockée en localStorage. Conformité : RGPD (aucune donnée perso), RGAA (WCAG 2.1 AA), RGESN (CSS natif, pas de framework CSS, JS minimal), AI Act (pas de composant IA). Structure : `src/content/parcours/` (MDX pédagogique), `src/components/exercises/` (React), `src/scripts/progress.ts` (progression). Conventions : PascalCase composants, kebab-case contenu, 2 espaces, 100 chars max. Tests : Vitest + Playwright. CI : GitHub Actions. Budgets : Lighthouse perf ≥95, a11y 100, FCP <1.5s, poids <200KB gzip.
