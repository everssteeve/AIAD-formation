# Changelog des Artefacts AIAD

> Ce fichier trace les mises à jour significatives des artefacts SDD Mode.
> Il permet de vérifier la synchronisation artefacts/code lors du Drift Check.

## Format

```
## [Date] — [Artefact] — [Type de changement]

**Auteur** : [Qui]
**Raison** : [Pourquoi cette mise à jour]
**Impact** : [SPECs ou code affectés]
```

---

<!-- Ajoutez vos entrées ci-dessous, les plus récentes en haut -->

## 2026-03-26 — SPEC-001 — Drift Lock validé

**Auteur** : Product Engineer (agent)
**Raison** : Scaffold Astro implémenté et validé. Ajout devDependencies (@astrojs/check, typescript) documenté dans la SPEC. Aucun drift fonctionnel.
**Impact** : SPEC-001 → done. INTENT-001 → en-cours.

## 2026-03-26 — PRD.md — Création initiale

**Auteur** : Product Engineer (cadrage /sdd-init)
**Raison** : Cadrage initial du projet SDD Training — site interactif de formation au cycle SDD Mode
**Impact** : Aucun code existant — fondation pour les futures SPECs

## 2026-03-26 — ARCHITECTURE.md — Création initiale

**Auteur** : Product Engineer (cadrage /sdd-init)
**Raison** : Définition de la stack technique (Astro 5 + MDX + React 19 îlots, CSS natif, GitHub Pages)
**Impact** : Cadre technique pour toutes les SPECs à venir

## 2026-03-26 — AGENT-GUIDE.md — Création initiale

**Auteur** : Product Engineer (cadrage /sdd-init)
**Raison** : Contexte permanent agent — conventions, vocabulaire métier, patterns, anti-patterns
**Impact** : Injecté dans chaque session de développement
