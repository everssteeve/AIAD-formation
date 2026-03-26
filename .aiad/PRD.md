# PRD : SDD Mode Training — Site Interactif de Formation

> Ce fichier est la source de vérité produit. Il est injecté en contexte agent lors des phases de cadrage uniquement.
> Mainteneur : Product Manager

## 1. Contexte et Problème

**Situation actuelle** : Le framework AIAD et son SDD Mode proposent un cycle de développement structuré (Intent → SPEC → Gate → Exec → Validate → Drift Lock), mais il n'existe aucun support de formation interactif pour les Product Engineers qui découvrent cette méthodologie. L'apprentissage se fait uniquement par la lecture de documentation statique et l'expérimentation non guidée.

**Qui ressent le problème** : Les Product Engineers qui doivent adopter SDD Mode dans leur pratique quotidienne et manquent d'un parcours d'apprentissage structuré et pratique.

**Impact business** : Sans formation guidée, l'adoption de SDD Mode est lente, les erreurs de cycle sont fréquentes (SPECs manquantes, Gates sautées, Drift non vérifié), et la valeur du framework n'est pas pleinement exploitée.

## 2. North Star / Product Goal

Un Product Engineer complète un cycle SDD complet de bout en bout sur le site de formation, en autonomie, et se sent capable de l'appliquer sur un vrai projet.

## 3. Personas et Use Cases

| Persona | Besoin | Résultat attendu |
|---------|--------|------------------|
| **Product Engineer débutant SDD** | Comprendre le cycle SDD et ses artefacts | A complété un cycle complet guidé et sait quand utiliser chaque commande |
| **Formateur / Lead** | Disposer d'un support de formation pour onboarder des PEs | Peut orienter les nouveaux PEs vers le site comme ressource d'apprentissage |

## 4. Outcome Criteria (Mesurables)

| Critère | Baseline | Cible | Méthode |
|---------|----------|-------|---------|
| Taux de complétion du parcours | 0% (n'existe pas) | ≥ 70% des utilisateurs qui démarrent terminent le cycle | Analytics / tracking de progression |
| Autonomie post-formation | Non mesurable | Le PE produit son premier Intent + SPEC sans assistance | Feedback qualitatif |
| Temps de parcours complet | N/A | ≤ 2 heures | Mesure sur le site |

## 5. Périmètre

### In Scope
- Site web interactif présentant le cycle SDD Mode étape par étape
- Exercices pratiques pour chaque phase du cycle (Intent, SPEC, Gate, Exec, Validate, Drift Lock)
- Exemples concrets d'artefacts bien rédigés vs mal rédigés
- Progression visible (étapes complétées)
- Conformité RGPD, AI Act, RGAA, RGESN

### Out of Scope
- Intégration avec un vrai repo Git (le PE s'entraîne dans le navigateur)
- Système d'authentification / comptes utilisateurs
- Backend / base de données (site statique)
- Certification ou scoring formel
- Contenu multilingue (français uniquement en v1)

## 6. User Stories (Prioritaires)

```
US-001 | MUST   | PE peut parcourir le cycle SDD étape par étape → Outcome : comprend l'enchaînement Intent → SPEC → Gate → Exec → Validate → Drift Lock
US-002 | MUST   | PE peut lire des exemples concrets d'artefacts (Intent, SPEC) → Outcome : distingue un bon artefact d'un mauvais
US-003 | MUST   | PE peut réaliser des exercices interactifs à chaque étape → Outcome : a pratiqué chaque phase du cycle
US-004 | SHOULD | PE voit sa progression dans le parcours → Outcome : sait où il en est et ce qu'il reste
US-005 | COULD  | PE peut consulter un glossaire du vocabulaire SDD → Outcome : maîtrise le vocabulaire métier
```

## 7. Trade-offs et Décisions Clés

| Décision | Raison | Coût / Bénéfice |
|----------|--------|-----------------|
| Site statique (pas de backend) | Budget minimal, hébergement gratuit (GitHub Pages) | Pas de persistance serveur — progression stockée côté client |
| Français uniquement en v1 | Public cible francophone, réduit le scope | Limite l'audience internationale |
| Exercices dans le navigateur (pas de vrai repo) | Réduit la complexité, accessible sans setup | Moins réaliste qu'un vrai projet |

## 8. Dépendances et Risques

**Dépendances externes** : Documentation AIAD / SDD Mode à jour (source de vérité pour le contenu)

**Risques** :
- Contenu qui diverge du framework réel — Mitigation : lier au repo AIAD officiel
- Exercices trop simples / pas assez engageants — Mitigation : itérer sur le feedback des premiers utilisateurs

## 9. Évolution Prévue (v2)

- Support anglais
- Exercices avancés avec simulation d'agent IA
- Mode "sandbox" avec vrai repo Git temporaire
- Tableau de bord formateur (suivi de progression des apprenants)
