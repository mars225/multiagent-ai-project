---
name: new-module
description: >
  Orchestre la création complète d'un nouveau module applicatif (scaffold).
  Déclencher quand l'utilisateur veut créer un nouveau module, un nouveau domaine
  métier, une nouvelle ressource API, ou scaffolder une nouvelle section du projet.
---

Lire GEMINI.md pour connaître le stack, la structure du projet et les conventions de nommage.

Demander le nom du module si non fourni.

Lancer le workflow WF-4 via l'Orchestrateur dans cet ordre strict :
1. Agenda            → architecture du module : entités, endpoints, composants, dépendances
2. DBA               → schéma DB + migration avec down() et stratégie de backfill si données existantes
3. Codeur            → scaffold backend : module, controller, service, entité, DTOs/schemas
4. Spécialiste Frontend → scaffold frontend : store, service HTTP, composants, routes (si applicable)
5. Testeur           → fixtures et factories de test pour le nouveau module
6. Documentation     → API docs (Swagger) + README du module
7. DevOps            → vérification que le module est déployable

Appliquer les règles de fail-fast : stopper si la migration n'a pas de down() fonctionnel.
