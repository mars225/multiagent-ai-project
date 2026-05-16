Lire CLAUDE.md pour connaître le stack, les commandes de test et le seuil de couverture du projet.

Demander le numéro de version si non fourni dans les arguments (format semver recommandé : MAJOR.MINOR.PATCH).

Lancer le workflow WF-5 via l'Orchestrateur dans cet ordre strict :
1. Testeur           → lancer tous les tests, vérifier couverture ≥ seuil CLAUDE.md
2. Auditeur Sécurité → audit des dépendances directes et transitives
3. DBA               → vérifier que toutes les migrations ont un down() fonctionnel
   ⚠️ Point de non-retour : confirmer que le rollback DB est possible avant de continuer
4. Fullstack + Perf  → vérification performances (requêtes N+1, bundle size, temps de réponse)
5. Réviseur          → revue de tous les changements depuis la dernière release
6. Documentation     → finaliser CHANGELOG : [Unreleased] → [VERSION]
7. DevOps            → vérifier que le pipeline CI est vert
8. DevOps            → préparer le runbook de rollback (étapes de retour arrière si échec prod)
9. Documentation     → commandes de tag et release git

Appliquer les règles de fail-fast à chaque étape.
