# SPEC-001-scaffold-astro

**Intent parent** : INTENT-001
**Auteur** : Steeve Evers
**Date** : 2026-03-26
**Statut** : done
**SQS** : 5/5 (Gate ouverte 2026-03-26)

---

## 1. Contexte

Le projet SDD Training nécessite un site statique de consultation guidée pour former les Product Engineers au cycle SDD. Cette SPEC couvre l'initialisation du projet Astro, la structure des contenus MDX, le layout de formation et la navigation entre les étapes du parcours.

## 2. Comportement Attendu

### Input
- Aucune donnée utilisateur en entrée
- Fichiers MDX dans `src/content/parcours/` définissant les étapes du cycle SDD

### Processing
1. Astro génère des pages statiques depuis les fichiers MDX via une content collection `parcours`
2. Chaque fichier MDX a un frontmatter : `title`, `description`, `order` (numéro d'étape)
3. Le layout `TrainingLayout.astro` encadre chaque page avec : header (titre du parcours), navigation latérale (liste des étapes), zone de contenu, navigation précédent/suivant
4. La navigation latérale affiche les étapes triées par `order` avec l'étape courante mise en évidence

### Output
- Site statique HTML avec pages générées pour chaque étape MDX
- Navigation fonctionnelle entre les étapes (latérale + précédent/suivant)

### Cas limites
- Un fichier MDX sans champ `order` dans le frontmatter → exclu du parcours, warning au build
- Un seul fichier MDX présent → la navigation précédent/suivant n'affiche pas de liens
- Aucun fichier MDX → la page d'index du parcours affiche un message "Parcours en cours de rédaction"

## 3. Critères d'Acceptation

- [ ] `npm run dev` lance le site sans erreur
- [ ] `npm run build` produit un site statique sans erreur
- [ ] Une page MDX dans `src/content/parcours/` génère une page accessible à `/parcours/[slug]`
- [ ] La navigation latérale liste toutes les étapes triées par `order`
- [ ] L'étape courante est visuellement identifiée dans la navigation latérale
- [ ] Les liens précédent/suivant permettent de parcourir les étapes séquentiellement
- [ ] Le HTML généré utilise des balises sémantiques (`<nav>`, `<main>`, `<article>`, `<aside>`)
- [ ] Le site est responsive (mobile-first)
- [ ] Lighthouse Accessibility = 100

## 4. Interface / API

### Content Collection Schema (`src/content.config.ts`)
```typescript
import { defineCollection, z } from 'astro:content';

const parcours = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    order: z.number(),
  }),
});

export const collections = { parcours };
```

### Layout Props (`src/layouts/TrainingLayout.astro`)
```typescript
interface Props {
  title: string;
  description: string;
  currentOrder: number;
  steps: Array<{ title: string; slug: string; order: number }>;
}
```

### Structure de fichiers créée
```
├── astro.config.mjs
├── tsconfig.json
├── package.json
├── src/
│   ├── content.config.ts
│   ├── content/
│   │   └── parcours/
│   │       └── 01-intent-statement.mdx   ← fichier placeholder
│   ├── layouts/
│   │   └── TrainingLayout.astro
│   ├── pages/
│   │   ├── index.astro                   ← redirect vers parcours
│   │   └── parcours/
│   │       └── [...slug].astro           ← pages dynamiques MDX
│   └── styles/
│       └── global.css                    ← reset, custom properties, layout
└── public/
    └── favicon.svg
```

## 5. Dépendances

- Aucune SPEC parente (c'est la fondation)
- Dépendances npm : `astro`, `@astrojs/mdx`
- Dépendances dev : `@astrojs/check`, `typescript` (type-check)
- Pas de React nécessaire pour cette SPEC (consultation uniquement, pas d'interactivité)

## 6. Estimation Context Budget

**Contexte à injecter pour cette tâche :**
- AGENT-GUIDE (condensé) : ~500 tokens
- Cette SPEC : ~600 tokens
- Fichiers source pertinents : `astro.config.mjs`, `src/content.config.ts`, `TrainingLayout.astro`, `global.css`
- **Total estimé** : ~3 000 tokens

## 7. Definition of Output Done (DoOD)

- [ ] Code implémenté et lint passing
- [ ] `npm run build` sans erreur ni warning
- [ ] Lighthouse Accessibility = 100 vérifié manuellement
- [ ] SPEC mise à jour si écart (Drift Lock)
- [ ] Gouvernance vérifiée : RGAA (sémantique HTML, navigation clavier), RGESN (CSS natif, pas de dépendance superflue)
