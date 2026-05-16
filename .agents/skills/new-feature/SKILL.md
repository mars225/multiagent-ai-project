---
name: new-feature
description: >
  Orchestre la création d'une nouvelle fonctionnalité complète de bout en bout.
  Déclencher quand l'utilisateur veut ajouter une feature, implémenter une nouvelle
  capacité, créer un nouvel écran ou endpoint dans le projet.
---

Lire AGENTS.md pour connaître le stack, les conventions et les commandes du projet.

Demander le nom de la fonctionnalité si non fourni.

Lancer le workflow WF-1 via l'Orchestrateur dans cet ordre strict :
1. Agenda      → plan détaillé et décomposition en tâches atomiques
2. Scout       → exploration du code existant lié à la feature
3. DBA         → migration DB + entité si modification de schéma nécessaire
4. Codeur      → implémentation backend (service, controller, DTO/schema)
5. Codeur      → implémentation frontend (store, composants, routes) si applicable
6. Testeur     → tests unitaires + E2E, vérification du seuil de couverture AGENTS.md
7. Réviseur    → code review qualité, sécurité, performance
8. Documentation → mise à jour Swagger/OpenAPI + entrée CHANGELOG

Appliquer les règles de fail-fast : stopper le workflow si une étape échoue.
Afficher le rapport final structuré à la fin du workflow.
