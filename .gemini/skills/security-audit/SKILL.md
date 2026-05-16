---
name: security-audit
description: >
  Orchestre un audit de sécurité complet OWASP A01-A10 suivi de tests d'intrusion.
  Déclencher avant une release, après l'ajout d'un endpoint sensible, ou à la demande
  d'un audit de sécurité, de vérification des vulnérabilités ou des dépendances.
---

Lire GEMINI.md pour connaître le stack, les mécanismes de sécurité et les dépendances du projet.

Lancer le workflow WF-3 via l'Orchestrateur dans cet ordre strict :
1. Auditeur Sécurité → audit statique OWASP A01–A10 sur le code source
2. Auditeur Sécurité → audit des dépendances directes ET transitives
   → pour chaque CVE sans fix : documenter la décision (fix / override / wontfix)
3. Pentester         → tests d'intrusion : auth bypass, IDOR, injection SQL/NoSQL
4. Pentester         → tests rate limiting (vérifier que les 429 se déclenchent effectivement)
5. Réviseur          → priorisation des vulnérabilités par criticité
6. Rapport sécurité  → document complet avec corrections recommandées

Environnement cible : développement et staging uniquement. Jamais sur la production.
