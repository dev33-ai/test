Register / Secure OAuth App — ORG_TOKEN

But : procédure pas-à-pas pour sécuriser l'OAuth App `ORG_TOKEN`, générer/rotater un client secret, et stocker le secret de façon sûre (secrets manager / GitHub org secrets). Ne pas committer les secrets dans Git.

Important : tu as actuellement un seul client secret. GitHub empêche la suppression du seul secret, il faut d'abord générer un nouveau secret, le déployer (mettre à jour les consommateurs), puis supprimer l'ancien.

1) Checklist rapide (avant de commencer)
- Owner désigné (ex: dev33-ai) — qui valide la rotation.
- Accès admin à GitHub Org → Developer settings → OAuth Apps.
- Accès au secrets manager organisationnel (Vault, AWS Secrets Manager, or GitHub org secrets via `gh`).
- Plan de rollback (qui contacter, notifications internes) et fenêtre de maintenance si nécessaire.

2) Générer un nouveau client secret (via l'UI)
- Dans l'org : Settings → Developer settings → OAuth Apps → ORG_TOKEN → Generate a new client secret.
- Copie ce nouveau client_secret immédiatement (tu ne pourras pas le revoir après création).

3) Stocker le secret en sécurité
- Recommande : stocker le secret dans le secrets manager de l'organisation (ex: Vault, AWS Secrets Manager, Google Secret Manager). Ensuite, propager vers les consumers via mécanismes automatiques.
- Si tu veux stocker dans GitHub org secrets (pour workflows/actions), tu peux utiliser `gh` :

```bash
# Installe/connexion gh CLI puis :
# Exemple : stocker le secret sous le nom ORG_TOKEN_CLIENT_SECRET visible à tous les repos de l'org
gh auth login
printf '%s' "<NOUVEAU_CLIENT_SECRET>" | gh secret set ORG_TOKEN_CLIENT_SECRET --org Fintech-Blueprint --visibility all
```

Remplace `<NOUVEAU_CLIENT_SECRET>` par la valeur que tu as copiée.

4) Mettre à jour les consommateurs (apps / CI / integrations)
- Met à jour toutes les apps / scripts / services qui utilisent l'ancien client_secret pour utiliser le nouveau secret (puis vérifier l'authentification via un test de smoke).
- Si tu as des tokens d'accès déjà émis à partir de l'ancien secret, considère leur invalidation selon le besoin de sécurité.

5) Supprimer l'ancien client secret
- Une fois le nouveau secret déployé et validé, supprime l'ancien via l'UI Developer settings (ou via l'API si tu as un script). GitHub demande au moins un secret présent pour l'app, donc la suppression n'est possible qu'après création du nouveau.

6) Documentation et audit
- Enregistre dans `docs/Org_Settings.md` ou dans ton registre d'audit : qui a roté le secret, date, raison, et liste des systèmes touchés.
- Ajouter une entrée ADR si la rotation est liée à une politique (ex: rotation 90 jours).

7) Paramètres à vérifier pour l'app ORG_TOKEN
- Application name: ORG_TOKEN — OK.
- Homepage URL: https://vongstaad.com — OK.
- Authorization callback URL: https://vongstaad.com/oauth/callback — OK (doit être exact).
- Enable Device Flow: activer seulement si tu veux autoriser flows CLI/device.
- Scopes: limiter au minimum requis (principle of least privilege).

8) Recommandations supplémentaires
- Considérer GitHub App si tu as besoin de permissions fines, webhooks, ou installations par repo/org.
- Plan de rotation automatique : implémente process ordonné (ex: alert 7 jours avant expiration, rotation via pipeline automatisée).
- Restreindre qui peut créer ou administrer OAuth Apps dans l'organisation (policies, owners only).
- Si tu exposes l'app (marketplace), configure logo et page de listing en respectant guidelines GitHub.

9) Procédure rapide pour rotation via API (avancé)
- Tu peux automatiser via l'API GitHub (requiert token admin). Exemple conceptuel (ne pas exposer secrets) : appelle l'endpoint pour créer un nouveau client secret puis stocke la valeur dans ton secrets manager. Voir la doc GitHub Developer API.

10) Post-rotation checks
- Test de connexion avec le nouveau secret depuis un consumer (smoke test).
- Vérifier logs d'audit GitHub (qui a créé/roté secret).
- Vérifier workflows/actions qui utilisent le secret avec une exécution de test.

---

Si tu veux, je peux :
- générer automatiquement un petit script `scripts/rotate_oauth_secret.sh` (template) qui :
  - appelle l'UI/API (conceptuel),
  - stocke dans `gh secret set` (ou Vault) et
  - notifie les owners (ex: via Slack webhook) — nécessite tokens/service account pour l'org.
- ou produire une version imprimable de cet audit et l'ajouter à `docs/`.

Dis-moi si tu veux que je génère le script template `rotate_oauth_secret.sh` maintenant (je le mets dans `scripts/` sans y inclure le secret explicite).