# [NOM DU PROJET] — [STACK PRINCIPAL]

<!--
  INSTRUCTIONS : Ce fichier est lu par tous les agents Claude au démarrage.
  Remplacer chaque [PLACEHOLDER] par les informations de votre projet.
  Supprimer les sections marquées "si absent" qui ne s'appliquent pas.
  Plus ce fichier est précis, plus les agents seront pertinents.

  LÉGENDE DES PLACEHOLDERS :
  [REQUIS] → À remplir obligatoirement — les agents ne peuvent pas fonctionner correctement sans
  [OPTIONNEL] → Remplir si applicable au projet, supprimer sinon
-->

## Stack technique
- **Backend / Serveur**: [REQUIS — ex: NestJS/Node.js, Django/Python, Spring Boot/Java, Laravel/PHP]
- **Frontend**: [OPTIONNEL — ex: Angular 17+, React 18, Vue 3, Next.js — supprimer si API seule]
- **Base de données**: [REQUIS — ex: MySQL 8, PostgreSQL 15, MongoDB 7, SQLite]
- **Auth**: [REQUIS — ex: JWT access+refresh, OAuth2, sessions httpOnly, Keycloak]
- **Conteneurs**: [OPTIONNEL — ex: Docker + Docker Compose, ou "aucun en développement"]
- **CI/CD**: [OPTIONNEL — ex: GitLab CI, GitHub Actions, Jenkins, CircleCI]

## Structure du projet
```
[nom-projet]/
├── [backend-dir]/          # [REQUIS — description du backend]
├── [frontend-dir]/         # [OPTIONNEL — description — supprimer si absent]
├── [infra-dir]/            # [OPTIONNEL — Dockerfiles, manifests K8s, configs Nginx]
└── docs/                   # [OPTIONNEL — ADR, architecture, guides]
```

## Commandes essentielles

### [Backend / API]  ← [REQUIS — adapter au framework]
```bash
[commande démarrage dev]         # dev avec hot-reload
[commande build]                 # build production
[commande test unitaires]        # tests unitaires
[commande test e2e]              # tests end-to-end
[commande migration:run]         # appliquer migrations (si ORM)
[commande migration:revert]      # annuler dernière migration
[commande migration:show]        # état des migrations
```

### [Frontend — supprimer si absent]  ← [OPTIONNEL]
```bash
[commande démarrage]             # dev server
[commande build]                 # build production
[commande test]                  # tests
[commande e2e]                   # tests E2E (Cypress, Playwright, etc.)
```

### Docker / Infra  ← [OPTIONNEL]
```bash
# Adapter selon votre infrastructure
[docker build / compose up / kubectl apply]
```

## Architecture Backend

### Modules / Domaines principaux  ← [REQUIS]
<!-- Lister les modules ou domaines métier du projet -->
- **[Auth]** — [REQUIS — ex: JWT, refresh tokens, guards/middleware]
- **[Utilisateurs]** — [REQUIS — ex: CRUD, profils, rôles]
- **[Domaine métier 1]** — [REQUIS — description]
- **[Domaine métier 2]** — [OPTIONNEL — description]

### Conventions de nommage  ← [REQUIS]
- Fichiers: [REQUIS — ex: `kebab-case.suffix.ts`, `snake_case.py`]
- Classes: [REQUIS — ex: PascalCase]
- Variables/méthodes: [REQUIS — ex: camelCase, snake_case]
- Constantes: [REQUIS — ex: UPPER_SNAKE_CASE]
- Tables DB: [REQUIS — ex: snake_case pluriel — `user_profiles`]

### Pattern d'accès aux données  ← [REQUIS]
<!-- Décrire l'approche choisie — l'agent DBA et le Codeur s'appuient sur cette info -->
[REQUIS — ex: Repository pattern via TypeORM. Jamais de SQL brut sauf migrations complexes.]
[ex: Active Record via Eloquent. Scopes pour les requêtes réutilisables.]
[ex: DAO pattern. QueryBuilder pour les requêtes complexes.]

### Validation des inputs  ← [REQUIS]
<!-- Décrire le mécanisme de validation — utilisé par le Codeur et le Réviseur -->
[REQUIS — ex: class-validator + class-transformer. Un DTO par opération.]
[ex: Zod schemas côté API. Validation au niveau du contrôleur.]
[ex: Pydantic models. Validation automatique par FastAPI.]

### Gestion des erreurs  ← [REQUIS]
<!-- Décrire la stratégie d'erreurs HTTP -->
[REQUIS — ex: Exceptions framework standard (NotFoundException, ForbiddenException).]
[ex: Middleware d'erreur Express centralisé. Codes HTTP standardisés.]

## Architecture Frontend  ← [OPTIONNEL — supprimer entièrement si pas de frontend SPA]

### Structure des modules
```
[OPTIONNEL — décrire la structure des dossiers frontend]
[ex: src/app/core/, shared/, features/, layout/]
```

### State management
[OPTIONNEL — ex: NgRx pour l'état global. Signals pour l'état local.]
[ex: Redux Toolkit + RTK Query. Context pour l'état local.]
[ex: Pinia. Composables pour l'état partagé entre composants.]

### Conventions
[OPTIONNEL — ex: Standalone components. OnPush partout. Lazy loading par feature.]
[ex: Composants fonctionnels. Hooks personnalisés. React.memo sur les listes.]

## Base de données

### Variables d'environnement ([répertoire]/.env)  ← [REQUIS]
```
DB_HOST=[REQUIS]
DB_PORT=[REQUIS]
DB_USERNAME=[REQUIS]
DB_PASSWORD=[REQUIS]
DB_NAME=[REQUIS]
[AUTH_SECRET ou JWT_SECRET]=[REQUIS — secret fort 32+ chars]
[AUTRES_SECRETS]=[OPTIONNEL]
```

### Schéma principal  ← [REQUIS]
<!-- Lister les entités / tables principales — utilisé par le DBA et le Codeur -->
- `[entité_1]` — [REQUIS — id, colonnes principales, created_at]
- `[entité_2]` — [REQUIS — id, colonnes principales, created_at]
- `[table_jonction]` — [OPTIONNEL — clés étrangères, rôle]

## Cache  ← [OPTIONNEL — supprimer si pas de cache applicatif]
- **Solution** : [OPTIONNEL — ex: Redis 7, Valkey, Memcached, in-process (node-cache / lru-cache)]
- **Intégration** : [OPTIONNEL — ex: @nestjs/cache-manager + cache-manager-redis-yet, django-redis, Spring Cache + @Cacheable, Laravel Cache facade]
- **TTL par défaut** : [OPTIONNEL — ex: 300s pour les entités, 60s pour les listes]
- **Variables d'environnement** :
  ```
  REDIS_HOST=[OPTIONNEL]
  REDIS_PORT=[OPTIONNEL — défaut: 6379]
  REDIS_PASSWORD=[OPTIONNEL]
  ```

## Sécurité (OWASP Top 10)  ← [REQUIS]
<!-- Décrire les mesures de sécurité en place — utilisé par l'Auditeur et le Réviseur -->
- Validation stricte de tous les inputs ([REQUIS — mécanisme])
- Pas d'interpolation SQL directe — [REQUIS — ORM / requêtes paramétrées]
- Rate limiting sur les endpoints sensibles ([REQUIS — mécanisme + seuil])
- CORS configuré explicitement ([REQUIS — pas de `*`])
- [REQUIS — Headers HTTP de sécurité — Helmet.js, middleware custom, etc.]
- Mots de passe hashés [REQUIS — algorithme + rounds, ex: bcrypt rounds 12]
- Tokens stockés de manière sécurisée [REQUIS — ex: httpOnly cookies]

## Tests  ← [REQUIS]
- **Unitaires**: [REQUIS — framework, ex: Jest, Pytest, JUnit, PHPUnit]
- **Intégration / E2E backend**: [REQUIS — ex: Supertest, pytest, RestAssured]
- **E2E frontend**: [OPTIONNEL — ex: Cypress, Playwright — supprimer si absent]
- **Couverture minimale**: [REQUIS — ex: 80]% sur les services / logique métier
- [OPTIONNEL — Outils de fixtures/factories : ex: @faker-js/faker, factory_boy, Faker]

## Qualité de code  ← [REQUIS]
- [REQUIS — Linter + Formatter, ex: ESLint + Prettier, Pylint + Black, Checkstyle + Spotless]
- [OPTIONNEL — Pre-commit hooks : ex: Husky, pre-commit, lefthook]
- [OPTIONNEL — Convention de commits : ex: Conventional Commits, Gitmoji]
- [OPTIONNEL — Analyse statique : ex: SonarQube, CodeClimate, Semgrep]

---

## Agents disponibles (`.claude/agents/`)

Les agents sont des sous-agents spécialisés invocables via le tool `Agent`. Chaque fichier `.md` dans `.claude/agents/` définit un rôle, ses responsabilités et ses règles de travail. **Tous les agents lisent `CLAUDE.md` au démarrage** pour connaître le stack et les conventions du projet courant.

### Quand et comment les utiliser

Passer le contexte nécessaire dans le prompt de l'agent (fichiers concernés, objectif, contraintes). Les agents ne lisent pas la conversation en cours — il faut les briefer explicitement.

### Catalogue des agents

#### Planification & Architecture
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **Orchestrateur** | `orchestrateur.md` | Dirige les autres agents en séquence pour livrer une fonctionnalité complète | On veut déléguer un workflow entier sans interventions manuelles |
| **Agenda** | `agenda.md` | Élabore un plan d'exécution détaillé et séquentiel avant toute implémentation majeure | Avant de commencer une nouvelle feature complexe |
| **Scout** | `scout.md` | Cartographie le code existant sans le modifier — localise patterns, fichiers, dette technique | En début de tâche pour explorer avant d'implémenter |

#### Développement
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **Codeur** | `codeur.md` | Implémente les fonctionnalités en respectant les conventions CLAUDE.md | Implémentation d'une feature après planification |
| **Spécialiste Backend** | `specialiste-backend.md` | Expert API backend — modules, DTOs/schemas, services, repositories | Travaux purement backend (API, entités, guards) |
| **Spécialiste Frontend** | `specialiste-frontend.md` | Expert frontend — composants, store, routing, UI | Travaux purement frontend (composants, store, UI) |
| **Fullstack + Perf** | `fullstack-et-perf.md` | Implémente de bout en bout (DB→API→Frontend→Tests) et optimise les performances | Feature complète cross-couches ou goulots d'étranglement |

#### Qualité & Tests
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **Testeur** | `testeur.md` | Écrit et exécute les tests ; bloque si couverture insuffisante | Écriture de tests, vérification de couverture |
| **Réviseur** | `reviseur.md` | Code review rigoureux (qualité, sécurité, perf) avant merge | Avant toute PR — vérification finale |
| **Débogueur** | `debogueur.md` | Analyse les erreurs par RCA (root cause analysis) sans fix hâtif | Investigation de bug |

#### Sécurité
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **Auditeur Sécurité** | `auditeur-securite.md` | Audit OWASP Top 10 — validation des inputs, chiffrement, exposition, dépendances | Avant une release ou après ajout d'un endpoint sensible |
| **Pentester** | `pentester.md` | Simule des attaques réelles (auth bypass, IDOR, injection, rate limiting, fuzzing) | Test d'intrusion avant mise en production |

#### Infrastructure & Données
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **DevOps** | `devops.md` | Gère Docker, CI/CD, reverse proxy, variables d'environnement | Changements infra, pipeline, déploiement |
| **DBA** | `dba.md` | Conception de schémas, migrations réversibles, data migrations, optimisation DB | Modifications de schéma, index, requêtes lentes |

#### Design & Documentation
| Agent | Fichier | Rôle | Déclencher quand… |
|-------|---------|------|-------------------|
| **Concepteur UI** | `concepteur-ui.md` | Définit UX, cohérence visuelle, interactions | Specs d'interface, design system, accessibilité |
| **Documentation** | `documentation.md` | Maintient Swagger/OpenAPI, README, CONTRIBUTING, CHANGELOG, ADR | Avant/après chaque release, ajout d'API |

### Workflows types (via Orchestrateur)

```
Nouvelle feature complète :
  Agenda → Scout → DBA → Spécialiste Backend → Spécialiste Frontend → Testeur → Réviseur

Bug critique :
  Débogueur → Codeur → Testeur → Réviseur

Préparation release :
  Testeur → Auditeur Sécurité → DBA (vérif rollback) → Réviseur → Documentation → DevOps

Refactoring DB :
  Scout → DBA → Spécialiste Backend → Testeur
```

### Exemple d'invocation

```
Agent({
  prompt: "Ajoute un endpoint PATCH /api/[ressource]/:id/[action].
           Fichiers concernés : [chemin/vers/service.ts], [chemin/vers/controller.ts].
           Respecter les conventions de CLAUDE.md."
})
```

> **Note** : Les agents lisent `CLAUDE.md` au démarrage pour connaître le stack et les conventions. Toujours fournir les chemins de fichiers et le contexte métier dans le prompt — ils ne voient pas la conversation précédente.
