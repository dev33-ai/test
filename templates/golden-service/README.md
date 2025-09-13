Golden service template — usage rapide

But : fournir un point de départ standardisé pour tout nouveau service cloné depuis le blueprint.

Contenu recommandé du template :
- README.md (ce fichier + quickstart)
- Dockerfile + image build pipeline
- .github/workflows/ci.yml (lint, unit, contract, sbom, image build)
- /src (squelette code)
- /tests (unit + contract)
- /docs (service-specific ADR, runbooks, api.md)
- /infra (k8s manifests/helm chart)

Utilisation rapide :
1. Cloner ce dossier dans un nouveau repo dans l'organisation.
2. Adapter README (owner, description) et CI (secrets namespaces).
3. Valider Gate0 checklist (infra & secrets accessibles) avant premier push.

Notes :
- Keep API contracts in `api/` and version them.
- Expose minimal observability (OpenTelemetry metrics + traces) by default.
