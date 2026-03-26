# INTENT-001-parcours-formation-sdd

**Auteur** : Steeve Evers
**Date** : 2026-03-26
**Statut** : en-cours

---

## POURQUOI MAINTENANT
Besoin de scaler la formation des Product Engineers. L'accompagnement individuel ne passe pas à l'échelle face au nombre de PEs à former.

## POUR QUI
Un développeur qui veut devenir Product Engineer dans un contexte SDD Mode.

## OBJECTIF
Un PE peut parcourir le cycle SDD complet (Intent → SPEC → Gate → Exec → Validate → Drift Lock) de manière autonome via un site web interactif de consultation guidée. Métrique : le PE identifie correctement l'enchaînement des 6 étapes et le rôle de chaque artefact après consultation du parcours.

## CONTRAINTES
- Conformité gouvernance Tier 1 (RGPD, AI Act, RGAA, RGESN) telle que définie dans `.aiad/gouvernance/`
- Consultation guidée uniquement — pas d'exercices interactifs en v1
- Budget technique minimal (site statique, pas de backend)

## CRITÈRE DE DRIFT
Si un PE constate des incohérences entre le contenu de la formation et la documentation officielle du framework SDD (artefacts, cycle, commandes, vocabulaire), l'implémentation a dérivé de l'intention.

---

## SPECs liées
- [ ] SPEC-001 — Scaffold projet Astro + structure du parcours
- [ ] SPEC-002 — Contenu pédagogique des 6 étapes du cycle SDD
- [ ] SPEC-003 — Page d'accueil + progression du parcours
