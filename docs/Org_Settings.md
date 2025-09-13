Organisation — Fintech-Blueprint — Paramètres importants

Ce document rassemble les paramètres organisationnels à conserver dans la documentation interne pour la création et l'administration des repositories et des applications OAuth.

1) Domaines vérifiés
- vongstaad.com — Verified

2) Section : OAuth App (token usage)

But : fournir un token d'application contrôlé par l'organisation pour usages internes (ex: accès GitHub Copilot, automation), documenter les valeurs à renseigner lors de l'enregistrement.

Register a new OAuth app — valeurs proposées
- Application name : ORG_TOKEN
  - Nom lisible par les utilisateurs et administrateurs.
- Homepage URL : https://vongstaad.com
  - URL complète de la page d'accueil de l'application.
- Application description : Token for GitHub Copilot access for VonGstaad org
  - Description affichée aux utilisateurs.
- Authorization callback URL : <À DÉFINIR>
  - URL de callback pour OAuth (si usage web). Si l'application n'a pas de composant web, indiquer https://vongstaad.com/oauth/callback comme placeholder ou laisser vide temporairement.
- Enable Device Flow : recommandé si l'application doit autoriser des dispositifs sans navigateur (activer si prévu).

Remarques de sécurité & process
- Scope minimal : limiter les permissions du token à l'usage nécessaire.
- Rotation : planifier rotation régulière (ex: 90 jours) et stocker le secret dans le secrets manager de l'organisation.
- Audit : enregistrer quel admin a créé l'app, la date, et l'usage prévu dans l'ADR ou registre d'audit.

3) Astuce callback URL
- Si tu n'es pas sûr du callback URL, utiliser un placeholder non-public, ex : https://vongstaad.com/oauth/callback, puis mettre à jour une fois l'URL finale connue.
- Pour usage Device Flow, callback URL n'est pas strictement nécessaire.

4) Prochaine étape
- Décider qui est l'owner de l'application (nom + GitHub handle).
- Créer l'OAuth App dans les Settings → Developer settings → OAuth Apps.
- Stocker client_id & client_secret dans le secrets manager et documenter l'usage.

Si tu veux, je peux :
- générer un fichier `scripts/register_oauth.md` avec la checklist pas-à-pas pour créer l'OAuth App dans l'org et enregistrer les secrets ;
- ou produire directement le `INSTRUCTIONS.md` et `push_to_org.sh` pour pousser les skeleton repos une fois que tu confirmes les noms des repos.
