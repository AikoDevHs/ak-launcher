# AK Launcher

Un petit launcher Windows pour **installer et mettre à jour mes jeux** en un clic.
Les jeux sont distribués dans les [Releases](../../releases) de ce dépôt ; le launcher les télécharge, les vérifie et les garde à jour.

## Installer

1. Ouvrez la page [**Releases**](../../releases) et téléchargez `AKLauncher.exe` dans la dernière release « AK Launcher ».
2. Lancez-le, choisissez un jeu dans la liste de gauche, cliquez sur **INSTALLER**.
3. Quand une nouvelle version sort, le bouton devient **METTRE À JOUR**. C'est tout.

Aucune installation, aucun compte, aucun droit administrateur.

> **Windows affiche « Windows a protégé votre ordinateur » ?**
> C'est normal pour un petit programme non signé (un certificat de signature coûte cher). Cliquez sur **Informations complémentaires → Exécuter quand même**.
> Si vous préférez ne pas me faire confiance sur parole, le code est ci-dessous : vous pouvez le lire et le compiler vous-même.

## Ce que fait (et ne fait pas) le launcher

| | |
|---|---|
| **Ce qu'il fait** | Télécharge la liste des jeux (`games.json`), puis les fichiers du jeu choisi depuis GitHub, en HTTPS. |
| **Vérification** | Chaque fichier est contrôlé par son empreinte **SHA-256** avant d'être installé. Un fichier corrompu est rejeté et rien n'est modifié. |
| **Où ça s'installe** | `%LOCALAPPDATA%\AKLauncher\` (votre profil utilisateur). Rien ailleurs, pas de droits administrateur. |
| **Réseau** | Uniquement GitHub. Le catalogue est refusé s'il pointe vers une autre adresse que `https://github.com/…`. |
| **Données personnelles** | Aucune télémétrie, aucun compte, aucune publicité. Comme tout téléchargement, GitHub voit votre adresse IP. |
| **Désinstaller** | Bouton **Désinstaller** dans le launcher, ou supprimez simplement le dossier `%LOCALAPPDATA%\AKLauncher`. |
| **Hors ligne** | Les jeux déjà installés restent lançables sans connexion. |

La vérification SHA-256 protège contre un téléchargement corrompu. Les empreintes viennent de ce dépôt : la confiance repose donc sur ce dépôt, d'où le code ouvert.

## Vérifier votre téléchargement

Chaque release du launcher indique le SHA-256 de `AKLauncher.exe`. Dans PowerShell :

```powershell
Get-FileHash .\AKLauncher.exe -Algorithm SHA256
```

Le résultat doit être identique à celui de la page de la release.

## Compiler vous-même

Le launcher tient en un seul fichier : [`src/AKLauncher/Program.cs`](src/AKLauncher/Program.cs) (environ 600 lignes de C#, sans dépendance).

```powershell
powershell -ExecutionPolicy Bypass -File build.ps1
```

Cela produit `dist\AKLauncher.exe` avec le compilateur fourni par Windows (.NET Framework 4, présent sur Windows 10 et 11).

## Jeux disponibles

| Jeu | Description |
|---|---|
| **GUN FIGHT** | FPS multijoueur : PVP et zombies en coopération, boutique, rangs et classement en ligne. |

Tous les jeux sont dans les [Releases](../../releases) (une release par version, par exemple `gunfight-v1.0.0`).

## Pour le développeur

- Ajouter ou publier un jeu : voir [`docs/PUBLISHING.md`](docs/PUBLISHING.md).
- Structure du dépôt :

```
src/AKLauncher/   code du launcher (C#)
games/            description de chaque jeu (nom, fichiers, build)
tools/            scripts de publication
docs/             documentation
build.ps1         compile le launcher
```

## Licence

[MIT](LICENSE) — le launcher est libre. Les jeux distribués ont leur propre licence.
