Runbook — DB primary failure & failover

Impact: Base primaire indisponible.
Owner: SRE
Severity: Critical

Pré-conditions: accès au DB Manager, playbooks failover, replica health.

Étapes immédiates (0-10min):
1. Vérifier health des replicas et du primary via DB Manager telemetry.
2. Si primary down, activer route de lecture depuis replicas (si safe).
3. Initiate failover via DB Manager (suivre procédure automatisée).
4. Mettre en place feature flag pour réduire charge write-heavy sur le service.

Remédiation (10-60min):
1. Monitor replication lag until stable.
2. Reconfigure apps to point to new primary via DB Manager.
3. Run integrity checks on recent transactions (compare ledger vs DB state).

Artifacts à collecter: telemetry graphs, replication lag, DB Manager audit log, migration scripts invoked.

Post-mortem: document cause, adjust autoscaling / healthcheck thresholds.
