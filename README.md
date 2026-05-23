# Redface 2 F-Droid repository

Dépôt F-Droid privé qui distribue les APKs signés de [Redface 2](https://github.com/ForumHFR/redface2),
le client Android moderne du forum Hardware.fr.

## Pour les utilisateurs

Ajoutez ce dépôt à votre client F-Droid (F-Droid, Neo Store, Foxy Droid, etc.) :

```
URL    : https://forumhfr.github.io/redface2-fdroid/
Fingerprint : (rempli après le premier build)
```

Voir la [documentation d'installation côté `redface2`](https://github.com/ForumHFR/redface2/blob/main/docs/guides/installation.md)
pour le walk-through complet.

> **Note** : si vous avez déjà installé Redface 2 via Google Play Store, vous devez
> d'abord la désinstaller avant de pouvoir installer via F-Droid. Les deux canaux
> utilisent des clés de signature différentes (la signature Play est générée par
> Google, la signature F-Droid est celle de notre clé d'upload originale).

## Pour les contributeurs

Ce repo contient :
- `main` : ce README + les workflows GitHub Actions.
- `gh-pages` (orphan) : l'index F-Droid (`index-v2.json`) + les APKs hébergés statiquement.

Le workflow `publish.yml` est déclenché par un `repository_dispatch` event de type
`rf2_release` émis depuis le workflow de release de `redface2` chaque fois qu'un
nouveau tag `app-v*` est publié. Le workflow télécharge alors l'APK correspondant
depuis la GitHub Release, le passe à `fdroid update` qui re-signe l'index avec la
clé du repo F-Droid, et push le résultat sur `gh-pages`.

L'identité d'écriture cross-repo est portée par une GitHub App
(`redface2-fdroid-publisher`) installée sur ce repo. Le workflow de `redface2`
échange l'App ID + la clé privée stockés en secret contre un token éphémère via
`actions/create-github-app-token@v1`, puis appelle l'API `repository_dispatch`
de ce repo.

## License

Le contenu de ce dépôt (workflows, métadonnées F-Droid) est sous MIT. Les APKs
distribués restent sous la licence de Redface 2 (GPL-3.0-only).
