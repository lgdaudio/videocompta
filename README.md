# VideoCompta CRM — espace privé

Application de gestion (CRM / comptabilité vidéaste) hébergée en site statique,
protégée par un **chiffrement de bout en bout côté navigateur**.

## Fonctionnement de la sécurité

- `app.enc` contient l'application **chiffrée en AES-256-GCM**. La clé est
  dérivée du mot de passe (PBKDF2-SHA256, 310 000 itérations).
- `index.html` est une page de connexion : elle demande l'e-mail autorisé et le
  mot de passe, puis **déchiffre l'application dans le navigateur**.
- **Sans le bon mot de passe, `app.enc` est indéchiffrable** — même pour
  quelqu'un qui télécharge tous les fichiers du site.
- Le mot de passe n'apparaît **en clair dans aucun fichier** servi.
- Une fois connectée, l'utilisatrice arrive directement dans l'application,
  **déjà identifiée** (compte pré-provisionné dans le stockage local).

Seul l'e-mail autorisé peut se connecter ; les données du CRM restent dans le
navigateur de l'utilisatrice (`localStorage`).

## Hébergement

Déployé via **GitHub Pages**. Le dépôt est **privé**.

> Note : sur les comptes GitHub gratuits, l'URL Pages reste techniquement
> publique même depuis un dépôt privé. C'est le **chiffrement** ci-dessus qui
> garantit la confidentialité réelle du contenu, pas l'URL.

## Re-chiffrer (changer le mot de passe)

Les fichiers source et l'outil de chiffrement sont conservés dans le dossier
parent (`VideoCompta.html` + `build/`). Pour régénérer `app.enc` :

```bash
APP_PASSWORD='nouveau-mot-de-passe' node build/encrypt.mjs
```

Puis mettre à jour l'e-mail autorisé dans `index.html` (`ALLOWED_EMAIL`) si besoin.
