Runbook — Model bias / high false-positive event

Impact: Modèle montre biais ou taux de faux positifs inacceptable.
Owner: ML Team Lead
Severity: High

Étapes immédiates (0-30min):
1. Isolate model traffic: switch to previous stable model in registry (rollback).
2. Activate shadow traffic to collect inputs producing anomalies.
3. Collect explainability logs (SHAP/LIME) for affected predictions.

Remédiation (30min-72h):
1. Run quick retrain with latest labeled data; run bias & fairness checks.
2. If retrain acceptable, publish new model to staging and run shadow validation.
3. Prepare communication to stakeholders and compliance (if regulatory impact).

Artifacts to collect: prediction samples, model version, dataset fingerprint, explainability reports.

Post-mortem: root cause analysis (data drift, feature change, labeling issue).
