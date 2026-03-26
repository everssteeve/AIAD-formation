# Index des SPECs

> Chaque SPEC est une spécification technique atomique liée à un Intent Statement.
> Format : `SPEC-NNN-[nom-court].md`
> Commande : `/sdd-spec` dans Claude Code

| ID | Titre | Intent parent | SQS | Statut | PR |
|----|-------|---------------|-----|--------|----|
| SPEC-001 | Scaffold projet Astro + structure du parcours | INTENT-001 | 5/5 | done | — |
| SPEC-002 | Contenu pédagogique des 6 étapes du cycle SDD | INTENT-001 | 5/5 | ready | — |
| SPEC-003 | Page d'accueil + progression du parcours | INTENT-001 | 4/5 | ready | — |

## Statuts possibles

- **draft** — SPEC en cours de rédaction
- **review** — En attente de validation SQS (Execution Gate)
- **ready** — SQS >= 4/5, prête pour développement agent
- **in-progress** — Agent en cours de développement
- **validation** — Code produit, en validation QA
- **done** — Code + SPEC synchronisés, PR mergée (Drift Lock)
- **archived** — Déplacée dans `archive/`
