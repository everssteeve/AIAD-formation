# SPEC-003-accueil-progression

**Intent parent** : INTENT-001
**Auteur** : Steeve Evers
**Date** : 2026-03-26
**Statut** : ready
**SQS** : 4/5 (Gate ouverte avec réserve 2026-03-26 — préciser le rendu visuel du schéma du cycle)

---

## 1. Contexte

Le PE qui arrive sur le site doit comprendre en quelques secondes ce qu'est le cycle SDD, visualiser les 6 étapes, et savoir où il en est dans le parcours. Cette SPEC couvre la page d'accueil et le mécanisme de suivi de progression.

## 2. Comportement Attendu

### Input
- Liste des étapes du parcours (content collection `parcours`)
- État de progression stocké dans `localStorage` (clé `sdd-training-progress`)

### Processing

**Page d'accueil :**
1. Affiche un titre, une description courte du parcours, et un schéma visuel du cycle SDD (6 étapes reliées en séquence)
2. Chaque étape du schéma est un lien cliquable vers la page MDX correspondante
3. Si une progression existe, les étapes consultées sont visuellement marquées

**Progression :**
1. Quand un PE consulte une page d'étape, elle est marquée comme "consultée" dans localStorage
2. Le format de stockage est : `{ [stepSlug]: { visited: true, visitedAt: timestamp } }`
3. La barre de progression dans le layout affiche le ratio étapes consultées / total

### Output
- Page d'accueil avec vue d'ensemble du cycle SDD
- Barre de progression visible dans le layout (toutes les pages)
- Marquage automatique des étapes consultées

### Cas limites
- localStorage indisponible (mode privé Safari) → la progression ne s'affiche pas, le site fonctionne sans
- Le PE visite une étape pour la première fois → marquée comme consultée au chargement de la page
- Le PE efface son localStorage → la progression repart à zéro, pas de message d'erreur
- Toutes les étapes consultées → la barre affiche 100% avec un message de félicitations

## 3. Critères d'Acceptation

- [ ] La page d'accueil affiche les 6 étapes du cycle SDD avec titre et description courte
- [ ] Chaque étape est un lien cliquable vers sa page de contenu
- [ ] Le schéma du cycle montre visuellement la séquence des étapes (Intent → ... → Drift Lock)
- [ ] Une barre de progression est visible dans le layout sur toutes les pages du parcours
- [ ] Visiter une page d'étape la marque comme "consultée" automatiquement
- [ ] La progression persiste entre les sessions (localStorage)
- [ ] Le site fonctionne normalement si localStorage est indisponible
- [ ] Lighthouse Accessibility = 100
- [ ] La page d'accueil charge en < 1.5s (FCP)

## 4. Interface / API

### Module de progression (`src/scripts/progress.ts`)
```typescript
const STORAGE_KEY = 'sdd-training-progress';

interface StepProgress {
  visited: boolean;
  visitedAt: number;
}

type Progress = Record<string, StepProgress>;

export function getProgress(): Progress;
export function markStepVisited(stepSlug: string): void;
export function getCompletionRatio(totalSteps: number): number;
export function resetProgress(): void;
```

### Composant barre de progression (`src/components/ui/ProgressBar.astro`)
```typescript
interface Props {
  currentStep: number;
  totalSteps: number;
}
```
Note : le ratio affiché est mis à jour côté client via un script inline qui lit localStorage.

### Page d'accueil (`src/pages/index.astro`)
- Récupère les étapes via `getCollection('parcours')`
- Les trie par `order`
- Affiche le schéma du cycle + liens

## 5. Dépendances

- **SPEC-001** : Le scaffold Astro, le layout et la content collection doivent exister
- **SPEC-002** : Le contenu des étapes doit exister pour que les liens fonctionnent (mais la page d'accueil peut être développée avant avec des placeholders)

## 6. Estimation Context Budget

**Contexte à injecter pour cette tâche :**
- AGENT-GUIDE (condensé) : ~500 tokens
- Cette SPEC : ~500 tokens
- `progress.ts` : ~200 tokens
- `TrainingLayout.astro`, `index.astro` : ~400 tokens
- **Total estimé** : ~1 600 tokens

## 7. Definition of Output Done (DoOD)

- [ ] Code implémenté et lint passing
- [ ] `npm run build` sans erreur
- [ ] Progression testée manuellement : visiter une étape → barre mise à jour → persiste après rechargement
- [ ] Graceful degradation testée : localStorage désactivé → pas de crash
- [ ] Lighthouse Accessibility = 100
- [ ] SPEC mise à jour si écart (Drift Lock)
- [ ] Gouvernance vérifiée : RGAA (barre de progression accessible, `aria-valuenow`/`aria-valuemax`), RGESN (pas de JS inutile, script inline minimal), RGPD (aucune donnée personnelle dans localStorage)
