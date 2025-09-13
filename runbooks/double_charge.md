Runbook — Double-charge / Duplicate-payment

Impact: Client débité deux fois pour la même opération.
Owner: Payments Team Lead
Severity: High

Prérequis: accès logs, ledger, outbox status, transaction id, user id.

Étapes immédiates (0-15min):
1. Isoler l'utilisateur (si nécessaire) : throttle ou mettre en maintenance la route payment.
2. Chercher la transaction id dans le ledger et vérifier l'idempotency key.
3. Si ledger montre deux charges, marquer le second comme suspicious/fraud en attente.
4. Communiquer à support / product (template message pré-rédigé).

Remédiation (15-120min):
1. Si duplicata confirmé : initier remboursement via saga compensating transaction et noter dans ledger.
2. Publier expliqueration dans audit log (qui, quand, pourquoi).
3. Rejouer/rechercher outbox publish pour l'événement manquant.

Artifacts à collecter: traces, logs, ledger entries, SBOM (si suspect code issue), commit id.

Post-mortem: créer ticket, ajouter test idempotency dans suite PR.
