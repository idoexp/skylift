<!--
SEO/AEO notes :
- Mots-clés français visés : "extension VS Code SFTP", "déployer FTP VS Code",
  "envoyer code FTP", "synchroniser SFTP Visual Studio Code", "upload SFTP".
- FAQ en Q/R parce que les moteurs de recherche IA (Perplexity, ChatGPT,
  Le Chat Mistral) extraient ces blocs prioritairement.

[🇬🇧 English](README.md)  ·  🇫🇷 Français
-->

# Skylift — Le SFTP le plus simple pour VS Code

> **Configuré en 60 secondes.** Aucun JSON à écrire. Aucune config à déboguer. Tu cliques, c'est tout.
> Push en un clic, diff visuel, uploads atomiques, détection d'anomalies.

[![Installer depuis le marketplace VS Code](https://img.shields.io/badge/VS%20Code%20Marketplace-Installer-blue?logo=visualstudiocode)](https://marketplace.visualstudio.com/items?itemName=IdoExperiences.skylift)
[![Dernière version](https://img.shields.io/github/v/release/idoexp/skylift)](https://github.com/idoexp/skylift/releases)
[![Licence : MIT](https://img.shields.io/badge/Licence-MIT-green.svg)](LICENSE)

![Sidebar Skylift avec une cible configurée et les quatre cards d'action](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/sidebar.png)

## La simplicité, en un paragraphe

La plupart des extensions SFTP pour VS Code partent du principe que tu sais écrire du JSON, que tu te souviens lequel est `host` et lequel est `username`, et que tu vas déboguer des séquences d'échappement dans des chemins shell à 23h. **Skylift fait l'inverse** : un **assistant point-and-click en 4 étapes** te pose les questions évidentes (hôte ? utilisateur ? mot de passe ou clé SSH ?), teste la connexion, te laisse **parcourir l'arborescence du serveur visuellement** pour choisir le bon dossier, et écrit la config pour toi. Ensuite, **Envoyer le code** c'est un clic dans la sidebar — ou un raccourci clavier. Aucun `sftp.json`. Aucun YAML. Aucune variable de template. Aucun safari de documentation.

Skylift est une **extension VS Code SFTP** conçue pour les développeurs qui déploient leur code par SFTP et qui ne veulent ni écrire du JSON, ni se battre avec des configs périmées, ni deviner ce qui se trouve réellement sur le serveur. C'est une alternative moderne à *liximomo/vscode-sftp* et *Natizyskunk/vscode-sftp* avec un setup en cliquant, un diff fiable, et un focus sur la sécurité (uploads atomiques, alertes sur fichiers sensibles, détection des modifications côté serveur).

---

## Sommaire

- [Pourquoi Skylift](#pourquoi-skylift)
- [Installation](#installation)
- [Démarrage rapide (60 secondes)](#démarrage-rapide-60-secondes)
- [Fonctionnalités](#fonctionnalités)
- [Travail en équipe](#travail-en-équipe)
- [Comparaison avec les autres extensions VS Code SFTP](#comparaison-avec-les-autres-extensions-vs-code-sftp)
- [FAQ](#faq)
- [Dépannage](#dépannage)
- [Roadmap](#roadmap)
- [Support](#support)
- [Licence](#licence)

---

## Pourquoi Skylift

Les extensions VS Code SFTP existantes sont puissantes mais lourdes : des dizaines de réglages, configuration JSON-only, pas de diff avant push, aucun signal si quelque chose cloche sur le serveur. Skylift part dans la direction opposée. Tout est conçu autour de trois principes :

### 1. Simple par défaut

- **Setup en cliquant** : un assistant en 4 étapes, jamais de JSON à éditer.
- **Push en un clic** : un bouton dans la sidebar (et un raccourci clavier). C'est toute l'interface pour le cas commun.
- **Auto-détection** : les clés SSH dans `~/.ssh/` sont listées automatiquement. Les patterns courants (`.git/`, `node_modules/`, `.env`, `.DS_Store`) sont pré-configurés. Les clés PuTTY `.ppk` sont converties à la volée. Rien à lire avant.
- **Navigateur distant visuel** : tu choisis le dossier de destination en cliquant dans l'arborescence du serveur. Tu ne tapes jamais un chemin.
- **Defaults raisonnables partout** : chaque préférence a une valeur par défaut qui correspond à ce que 95 % des devs veulent. Tu ouvres le panneau, tu cliques Push, tu déploies.

### 2. Précis

- **Smart Push** : walk les deux côtés (local et distant) à chaque push, avec un cache de dossier agressif. Toujours juste, même après un `git pull` ou dans un setup multi-dev.
- **Diff visuel avant envoi** : colonnes émetteur/récepteur avec flèches directionnelles (vert = upload, rouge = download), actions cascadantes au niveau dossier, filtres par statut.
- **Diagnose des anomalies** : liste les fichiers qui existent sur le serveur mais pas en local. Décide par fichier : supprimer, télécharger, sauvegarder, ou toujours ignorer.

### 3. Sécurisé

- **Uploads atomiques** : écriture dans un fichier temp puis renommage. Un upload tué ne corrompt jamais le fichier en place.
- **Préservation de la mtime** : sémantique rsync `-t` — la mtime serveur reste alignée avec la locale après chaque upload, plus de fausse alerte « le distant est plus récent ».
- **Patterns d'ignore conscients de la sécurité** : `.env`, `.git/`, `.env.*` sont marqués comme alertants (déclenchent une anomalie si trouvés sur le serveur) — les autres sont silencieux.
- **Les secrets restent hors du repo** : mots de passe et passphrases dans le keychain de l'OS via VS Code `SecretStorage`. Jamais écrits dans le JSON.
- **Aucune télémétrie**.

---

## Installation

### Depuis le Marketplace VS Code (recommandé)

1. Ouvre VS Code.
2. `Ctrl+Shift+X` (Extensions).
3. Cherche **Skylift**.
4. Clique **Installer**.

Ou en ligne de commande :

```bash
code --install-extension IdoExperiences.skylift
```

### Depuis un fichier `.vsix` (offline / pré-release)

Récupère le dernier `.vsix` depuis la page [Releases](https://github.com/idoexp/skylift/releases) et installe :

```bash
code --install-extension skylift-X.Y.Z.vsix
```

---

## Démarrage rapide (60 secondes)

1. Clique l'icône **Skylift** dans la barre d'activité (rail de gauche).
2. Clique **Configurer une cible**. L'assistant te guide :
   - **Connexion** SFTP (hôte, port, utilisateur)

     ![Étape 1 de l'assistant — formulaire de connexion (hôte, port, utilisateur)](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/wizard-connection.png)

   - **Authentification** (mot de passe ou clé SSH — les clés dans `~/.ssh/` sont auto-détectées, format `.ppk` PuTTY supporté)
   - **Test de connexion**
   - Choix du **dossier distant** en parcourant le serveur de manière interactive (sans taper de chemin)

     ![Étape 3 de l'assistant — navigateur de dossier distant interactif](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/wizard-remote-browser.png)

   - Choix du **dossier local** (par défaut le workspace)
3. Clique **Envoyer le code** — c'est tout.

Aucun `sftp.json` à écrire, aucune séquence d'échappement à déboguer.

---

## Fonctionnalités

### Push en un clic

`Skylift : Envoyer le code` (`Ctrl+Alt+U` / `⌘⌥U`) walk les deux côtés, calcule le diff, et envoie uniquement ce qui a changé. Marche en solo ou en équipe grâce à la comparaison directe local↔distant (pas de manifest périmé après un `git pull`).

### Revoir et envoyer (upload sélectif)

`Skylift : Revoir et envoyer` (`Ctrl+Alt+Shift+U`) ouvre un panneau avec un tableau triable, filtrable et cherchable de chaque changement. Chaque ligne affiche taille+mtime locale côte à côte avec celles du distant, plus une flèche directionnelle :

- **→ verte** : le local est plus récent (upload logique)
- **← rouge** : le distant est plus récent (téléchargement = défaut sûr — uploader écraserait un distant plus récent)

![Panneau Revoir et envoyer : tableau triable, chips de filtre par statut, colonnes émetteur/récepteur avec flèches directionnelles vertes/rouges](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/review-table.png)

Les dossiers absents du serveur sont remontés dans une card séparée avec actions cascadantes (Tout envoyer / Tout ignorer / Toujours ignorer ce dossier / Personnalisé par fichier).

![Revoir et envoyer : card des dossiers absents sur le distant avec dropdowns cascadants](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/review-folder-card.png)

### Diagnostiquer les anomalies

`Skylift : Diagnostiquer les anomalies` trouve les fichiers présents sur le serveur mais absents en local — typique du « ce truc était dans le repo, on l'a supprimé mais le serveur l'a encore ». Actions par fichier : **Supprimer**, **Télécharger**, **Sauvegarder** (renommer dans un dossier de backup), **Ignorer** (ajouter aux patterns d'ignore).

Les dossiers entièrement absents en local apparaissent dans une card séparée. La colonne **Localisation** rend explicite que le fichier vit sur le serveur et que l'action cible le serveur, jamais ta copie locale.

![Panneau Diagnostic des anomalies : card des dossiers en trop en haut, tableau par-fichier en dessous avec colonnes Localisation / Source](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/diagnose-table.png)

### Envoyer le fichier courant

Clic droit sur n'importe quel fichier dans l'éditeur → **Skylift : Envoyer le fichier courant**, ou `Ctrl+Alt+P` / `⌘⌥P`. Upload single-file via la cible active.

### Comparer avec le distant

Clic droit → **Skylift : Comparer avec le distant**. Diff côte à côte via l'éditeur de diff natif de VS Code.

### Uploads atomiques (sécurité grade rsync)

Chaque upload écrit d'abord dans `.<fichier>.skylift_tmp.<aléa>`, puis fait un `posix-rename` atomique (sur OpenSSH moderne ; fallback `delete + rename` sur les serveurs plus vieux). Un upload tué ne remplace jamais un fichier qui marche par une moitié corrompue. Après le rename, la mtime distante est alignée sur la mtime locale via `setattr` (sémantique rsync `-t`).

### Multi-cibles

Ajoute plusieurs destinations distantes et bascule entre elles depuis la sidebar. `Skylift : Envoyer vers plusieurs cibles` envoie un push à plusieurs serveurs en une seule action (staging + production, deux clusters, etc.).

### Auto-upload à la sauvegarde (opt-in)

Settings → Auto-upload à la sauvegarde : envoie le fichier courant à chaque save. Désactivé dans les workspaces non-trustés. Désactivé par défaut.

### Auto-push au commit git

Settings → Déclencheur Git → liste de branches surveillées (ex. `main`, `production`). Quand tu commits sur une branche surveillée, Skylift déclenche un push. Pratique pour les environnements de staging liés à une branche.

### Smart Push (workflow équipe sans config)

Push code walk les deux côtés à chaque exécution. Combiné à **Considérer même taille = même contenu** (ON par défaut, mirror les défauts `rclone` / `rsync`) et à la **préservation de mtime** par fichier (rsync `-t`), le diff de Skylift est précis quel que soit le setup : solo, équipe, avec ou sans git, après un `git pull` ou non.

### Skylift : Réinitialiser les mtimes depuis git (optionnel, utilisateurs git)

Aligne la mtime locale de chaque fichier tracked sur la date de son dernier commit. Les fichiers avec des modifs non commitées sont préservés. Rend le cache manifest de Skylift cohérent entre machines sur la même histoire git. À lancer à la demande. Requiert git dans le PATH ; affiche un message clair sinon.

### Skylift : Vérifier contre le distant (deep check)

Comparaison content-hash bulletproof : hashe chaque fichier des deux côtés et reporte les divergences. Lent (chaque fichier distant doit être téléchargé pour calculer son hash) mais absolu. À lancer quand tu suspectes un drift. Commande manuelle — ne tourne jamais dans le flow normal de push.

### Config project-scoped, versionnable

Toutes les préférences vivent dans `.skylift/config.json` à côté de ton code. Commit-le ; le reste de ton équipe récupère ton setup d'assistant automatiquement. **Les secrets ne sont PAS dans le JSON** — ils vivent dans le keychain de l'OS (DPAPI / Keychain / libsecret) via `SecretStorage` de VS Code.

![Panneau Settings : patterns d'ignore en chips colorés (built-in/global/local), toggles iOS-style pour les préférences, picker de langue, outils de maintenance](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/settings.png)

### UI multilingue

Français, anglais, espagnol, allemand, italien, portugais brésilien. Override par projet (Settings → Langue) ou suit la langue de VS Code.

### Modales custom

Une modale dans le webview inspirée de SweetAlert2 avec animations fluides remplace le `confirm()` natif. Utilisée pour les actions destructives (suppression d'un pattern global, purge du manifest, deep check) pour que l'UX matche le polish du reste du panneau.

![Modale custom : animation scale-in avec icône colorée, variante danger pour les actions destructives](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/modal.png)

### UI conscient du thème

Chaque panneau utilise les variables CSS de VS Code, donc Skylift se sent chez lui dans les thèmes dark, light et high-contrast sans configuration.

![Skylift en thème dark vs. light — même UI, couleurs adaptées au thème](https://raw.githubusercontent.com/idoexp/skylift/main/screenshots/dark-light.png)

---

## Travail en équipe

Le modèle de sync de Skylift est conçu pour fonctionner pareil que tu sois solo, en équipe, avec ou sans git :

- **Push code** et **Revoir et envoyer** walk LOCAL ET DISTANT à chaque exécution, puis diff direct sur le contenu. Le manifest par-target dans `.skylift/manifest.<target>.json` est purement un cache (pour skip les `readdir` SFTP redondants quand les dossiers n'ont pas bougé) — il n'est plus la source de vérité de ce qui est synchronisé. Push code reste précis quoi qu'il arrive après un `git pull` ou des modifs hors-bande.

- **Considérer même taille = même contenu** (Settings → Préférences) est **activé par défaut**. Quand local et distant ont la même taille, Skylift considère le fichier comme synchronisé même si les mtimes ont divergé. Mirror les défauts `rclone` / `rsync`. Le cas théorique « même taille, contenu différent » est ≈ 0 pour du code/texte. Utilise **Verify deep check** si tu veux une garantie absolue.

- **Skylift : Réinitialiser les mtimes depuis git** est un helper optionnel pour les utilisateurs git. Il aligne la mtime locale de chaque fichier tracked sur la date de son dernier commit. Après l'avoir lancé, deux devs sur le même checkout git ont des mtimes locales identiques pour chaque fichier tracked. À lancer après un `git pull` si tu veux que le manifest soit bit-stable entre machines.

- **Skylift : Vérifier contre le distant (deep check)** hashe chaque fichier des deux côtés et reporte les divergences. Lent mais absolu. À utiliser quand tu suspectes un drift ou avant un déploiement critique.

`.skylift/` est un artefact par-développeur et doit être ajouté au `.gitignore`. Skylift ne l'enverra pas sur le serveur (pattern d'ignore intégré) mais le commit créerait des conflits de merge à chaque push.

---

## Comparaison avec les autres extensions VS Code SFTP

| Fonctionnalité | Skylift | liximomo/vscode-sftp | Natizyskunk/vscode-sftp |
|---|---|---|---|
| Setup assistant / point-and-click | ✅ | ❌ JSON only | ❌ JSON only |
| Uploads atomiques (temp + rename) | ✅ | ❌ | ❌ |
| Diff visuel avant push | ✅ | ⚠ basique | ⚠ basique |
| Flèches direction + colonnes émetteur/récepteur | ✅ | ❌ | ❌ |
| Actions cascadantes au niveau dossier | ✅ | ❌ | ❌ |
| Filtres par statut (new / modified / ignored) | ✅ | ❌ | ❌ |
| Détection d'anomalies (fichiers orphelins distants) | ✅ | ❌ | ❌ |
| Préservation mtime (`rsync -t`) | ✅ | ❌ | ❌ |
| Deep check par hash de contenu | ✅ | ❌ | ❌ |
| Réinitialiser les mtimes depuis git | ✅ | ❌ | ❌ |
| UI multilingue (FR/ES/DE/IT/PT-BR) | ✅ | ❌ EN only | ❌ EN only |
| Settings dans le keychain | ✅ | ⚠ dans le JSON | ⚠ dans le JSON |
| Dernier commit | ✅ actif | ⚠ peu d'activité | ⚠ peu d'activité |

---

## FAQ

### Skylift a besoin de git ?

**Non.** Skylift fonctionne sur n'importe quel projet, avec ou sans git. La commande optionnelle `Skylift : Réinitialiser les mtimes depuis git` est la seule fonctionnalité qui requiert git dans le PATH ; si git n'est pas installé, la commande affiche une erreur claire et le reste de Skylift continue de fonctionner.

### Skylift va-t-il re-uploader tout après un `git pull` ?

**Non.** Push code utilise **Smart Push** : il walk local et distant à chaque exécution et compare directement. Combiné à la préférence **Considérer même taille = même contenu** (activée par défaut), Skylift reconnaît que les fichiers de même taille sont synchronisés même si les mtimes ont divergé après un `git pull`. Si tu veux que le manifest soit bit-stable entre machines quand même, lance `Skylift : Réinitialiser les mtimes depuis git` après chaque pull.

### Deux développeurs peuvent-ils partager une config `.skylift/` ?

Les **parties non-secrètes**, oui — commit `.skylift/config.json` dans git et ton équipe a la même config de cible. Les **secrets** (mots de passe, passphrases de clé) restent par-machine dans le keychain de l'OS. Le `manifest.<target>.json` par-target est un cache par-machine et doit être `.gitignore`d.

### Mes mots de passe sont-ils stockés en clair ?

**Non.** Les mots de passe et passphrases SSH sont stockés via l'API `SecretStorage` de VS Code, qui est backed par :
- **Windows** : DPAPI (Data Protection API)
- **macOS** : Keychain
- **Linux** : libsecret (GNOME Keyring / KWallet)

Jamais écrits sur disque en clair, jamais embarqués dans le marketplace, jamais loggés.

### Que se passe-t-il si l'upload est tué en plein transfert ?

**Le fichier original reste intact.** Skylift écrit dans `.<fichier>.skylift_tmp.<aléa>` d'abord, puis renomme atomiquement par-dessus la cible. Un upload tué laisse le fichier temp (filtré automatiquement par le pattern d'ignore enforced `*.skylift_tmp.*`) et le fichier en place intact. Au prochain Diagnose, tu verras une bannière « fichiers temp orphelins » avec un cleanup en un clic.

### Skylift supporte-t-il les clés `.ppk` (PuTTY) ?

**Oui.** Les clés au format PuTTY sont converties à la volée via `sshpk`. Pointe juste Skylift sur le fichier `.ppk` dans l'étape Auth de l'assistant.

### Skylift fonctionne-t-il en SSH-only, ou il a besoin de SFTP côté serveur ?

Il a besoin de SFTP activé (sous-système `sftp` dans `sshd_config`, qui est le défaut sur tout serveur OpenSSH moderne). SSH pur sans SFTP ne fonctionne pas.

### Quels serveurs sont supportés ?

Tout ce qui parle SFTP : OpenSSH (Linux/macOS/Windows OpenSSH server), Bitvise, ProFTPD avec SFTP, Pure-FTPd avec SFTP, AWS Transfer Family, Cerberus, GoAnywhere, et tout service SSH/SFTP standard. Si ton serveur parle SSH avec le sous-système SFTP, Skylift parle avec lui.

### Pourquoi mon premier push est lent sur un gros repo ?

Le premier push walk l'arborescence distante complète une fois pour construire le cache de dossiers. Les push suivants hit le cache (typiquement 1 round-trip pour le listing racine, puis ≈ 0 pour les sous-arbres stables). Sur 5000 fichiers dans 800 dossiers, le premier push prend environ 30 secondes ; le deuxième est quasi-instantané.

### Skylift collecte-t-il de la télémétrie ?

**Non.** Zéro télémétrie, zéro tracking, zéro phoning home. Skylift est entièrement local sauf pour la connexion SFTP vers ton propre serveur.

### Comment je signale un bug ?

[Ouvre une issue sur GitHub](https://github.com/idoexp/skylift/issues/new). Inclus ta version VS Code, ta version Skylift (Settings → Skylift → À propos, ou regarde le `package.json`), ton OS, et une description de ce que tu attendais vs. ce qu'il s'est passé. Les logs depuis `Skylift : Afficher les logs` (canal Output) aident beaucoup.

### Le code source est-il ouvert ?

Le **repo public** ([idoexp/skylift](https://github.com/idoexp/skylift)) héberge les releases, la documentation, le tracker d'issues et les release notes. Le code source complet est dans un dépôt privé pour le moment. Cela peut changer dans le futur. La licence du `.vsix` publié est **MIT**.

### Je peux sponsoriser ou contribuer ?

Les bug reports, feature requests, captures d'écran et traductions sont les bienvenus. Les contributions de code ne sont pas acceptées pour l'instant (closed source). Mets une étoile au repo si tu veux suivre les updates — c'est le signal de support le plus simple.

---

## Dépannage

### « Git not found on PATH » en lançant Réinitialiser les mtimes

Skylift utilise le binaire `git` du système. Installe-le depuis <https://git-scm.com/downloads> et assure-toi qu'il est dans le PATH. Sur Windows, cocher « Use Git from the Windows Command Prompt » dans l'installeur Git for Windows fait l'affaire.

### « Connection refused » ou « Connection timeout »

Vérifie que :
- L'hôte et le port sont corrects (port SFTP par défaut : 22).
- Ton firewall / proxy d'entreprise autorise le SSH/SFTP en sortie.
- Le serveur est up. `Skylift : Tester la connexion` depuis la sidebar te donne un message d'erreur ciblé.

### « Server host key was rejected »

Skylift vérifie la host key du serveur contre `~/.ssh/known_hosts` (comme `ssh`). Si c'est un nouveau serveur, connecte-toi manuellement avec `ssh user@host` une fois pour accepter la clé, puis ré-essaye depuis Skylift.

### « Tous mes fichiers apparaissent comme modifiés à chaque push »

Probablement un drift d'horloge entre ta machine et le serveur, ou tu es dans un setup multi-dev avec check mtime strict. Solutions :
- Vérifie que **Considérer même taille = même contenu** est ON (Settings → Préférences). Ça absorbe la plupart des cas.
- Après un `git pull`, lance `Skylift : Réinitialiser les mtimes depuis git`.
- Pour une garantie absolue, lance `Skylift : Vérifier contre le distant (deep check)`.

### « Push code est lent »

Le premier push sur une nouvelle cible walk l'arbo distante complète. Sur les pushs suivants, le cache de dossiers réduit ça à ≈ 0. Si c'est encore lent :
- Ajoute des patterns d'ignore plus agressifs (Settings → Patterns d'exclusion) pour `node_modules/`, les artefacts de build, etc.
- La concurrence du walk est de 4 round-trips en parallèle par défaut. Certains serveurs cappent les channels SFTP simultanés — si tu vois des erreurs « too many open channels », c'est ça.

---

## Roadmap

- **v0.4** : extension OpenSSH `[email protected]` pour le fast path du Verify deep check (évite le download fallback). Telemetry opt-in. Visualiseur d'historique de backup.
- **v0.5** : support cible WebDAV. Support cible S3.
- **v1.0** : stabilisation de l'API. Décision finale sur l'open-sourcing.

---

## Support

- **Bug reports** : <https://github.com/idoexp/skylift/issues>
- **Feature requests** : même endroit — pick le template « Feature request ».
- **Marketplace** : <https://marketplace.visualstudio.com/items?itemName=IdoExperiences.skylift>
- **Documentation** : ce README, [CHANGELOG](CHANGELOG.md).

---

## Licence

MIT — voir [LICENSE](LICENSE). Le `.vsix` publié est sous licence MIT. Le code source est privé pour l'instant et peut être ouvert dans le futur.

---

> Fait par [@idoexp](https://github.com/idoexp). Développé avec soin pour les développeurs qui déploient depuis VS Code tous les jours.
