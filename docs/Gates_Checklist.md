Checklist synthétique par Gate (version courte)

Gate0 — Platform Ready
- Cluster K8s reachable
- GitOps connected
- Secrets manager reachable
- Image registry accessible & signing policy active
- Golden template deployé en mock

Gate1 — PR CI
- Lint OK
- Unit tests pass
- Contract tests pass
- Image build & SBOM generated
- Quick SAST/SCA (blocker vuln => fail)
- Docs: README + ADR link

Gate2 — Merge → Staging
- GitOps apply OK
- Smoke / E2E happy path
- DB migration dry-run OK
- Observability sanity (logs/metrics/traces emitted)
- ML: model artifact + validation report (if applicable)

Gate3 — Canary
- Canary plan (traffic %, duration) défini
- Infra SLOs (p95, error-rate) OK
- Business KPIs OK
- Cost guard checked
- ML: shadow testing metrics OK

Gate4 — Full Rollout
- Audit entry créé (who/what/when)
- Explainability logs (ML) stored
- Sustained SLO/KPI window OK
- Outbox & DLQ health OK

Gate5 — Operational Maturity
- Chaos/game day schedule
- Monthly audits (SBOM, secrets rotation)
- Drift detection active (ML)
- FinOps budget checks
