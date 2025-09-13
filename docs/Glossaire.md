Glossaire (sélection critique pour le blueprint)

Concierge : Service broker qui délivre accès temporaires aux ressources critiques (DB, API externes). Applique quotas, token short-lived, et log d'audit.

DB Manager : Point d'entrée unique pour les accès DB (pooling, quotas, replica routing, migrations dry-run).

Outbox : Pattern transactionnel où les événements sortants sont persistés dans la même transaction que l'écriture métier pour garantir livraison fiable.

DLQ (Dead Letter Queue) : File pour messages/événements qui n'ont pas pu être traités pour reprocessing manuel/automatique.

Ledger : Stock immuable d'événements/transactions utilisé pour audit et reconciliation (durée longue).

SBOM : Software Bill of Materials — inventaire des composants logiciels d'une image/artifact.

Model Registry : Gestion versionnée des artefacts modèle (hash, dataset fingerprint, metrics, signature).

SLO / KPI : Service Level Objective / Key Performance Indicator — métriques opérationnelles et business surveillées lors des Gates.
