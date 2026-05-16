Lire CLAUDE.md pour connaître les conventions, le stack et les critères de qualité du projet.

Demander le diff git ou le titre de la MR/PR si non fourni dans les arguments.
Coller le diff avec : git diff main...HEAD

Lancer le workflow WF-6 via l'Orchestrateur dans cet ordre strict :
1. Scout       → cartographier les fichiers modifiés et leur rôle dans le projet
2. Réviseur    → revue qualité : conventions CLAUDE.md, DRY, nommage, sécurité, performance
3. Testeur     → vérifier que les tests couvrent les changements introduits
4. Auditeur    → vérification sécurité ciblée sur les endpoints et données modifiés
5. Verdict     → Approuvé ✅ / Approuvé avec réserves ⚠️ / Changements requis ❌ + liste des actions

Format de sortie : rapport de revue structuré avec points positifs, problèmes critiques, améliorations recommandées.
