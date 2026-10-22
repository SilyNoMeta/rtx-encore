<div align="center">

# RTX Encore

**Génération d'images jusqu'à X6, Smooth Motion et Neural Rendering\
pour les GeForce RTX 20, RTX 30 et RTX 40.**

Un seul fichier à côté du jeu, un menu intégré, rien d'autre à installer.

[![Dernière version](https://img.shields.io/github/v/release/SilyNoMeta/rtx-encore?include_prereleases&style=for-the-badge&label=Version&color=76b900)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Téléchargements](https://img.shields.io/github/downloads/SilyNoMeta/rtx-encore/total?style=for-the-badge&label=T%C3%A9l%C3%A9chargements&color=2f81f7)](https://github.com/SilyNoMeta/rtx-encore/releases)

[![Cartes](https://img.shields.io/badge/Cartes-RTX%2020%20%7C%2030%20%7C%2040-76b900?style=flat-square&logo=nvidia&logoColor=white)](docs/COMPATIBILITY.fr.md)
[![API graphiques](https://img.shields.io/badge/API-DirectX%2011%20%7C%2012%20%7C%20Vulkan-0078d4?style=flat-square)](docs/COMPATIBILITY.fr.md)

**[Télécharger](https://github.com/SilyNoMeta/rtx-encore/releases)** ·
**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** ·
**[Installation](docs/INSTALLATION.fr.md)** ·
**[English](README.md)** ·
**[中文](README.zh-CN.md)**

</div>

> [!IMPORTANT]
> RTX Encore est un mod indépendant et expérimental. Il n'est ni affilié à NVIDIA ni approuvé par NVIDIA.
> Il se charge dans le jeu : ne l'utilisez pas dans un jeu en ligne ou protégé par un anti-triche, où il peut être
> bloqué ou faire signaler un compte. Sauvegardez chaque fichier que vous remplacez.

## 1.2.0

Presets DLSS et changement de moteur en cours de jeu, Smooth Motion avec le pilote NVIDIA 621.01, et le moteur et le
preset affichés dans le menu. Voir le [journal des modifications](CHANGELOG.fr.md).

**XeSS / XeFG : TRÈS EXPÉRIMENTAL.** Cette intégration a été particulièrement pénible à mettre au point :) Attendez-vous à des bugs, crashs, artefacts et différences entre jeux et API graphiques. N’hésitez pas à remonter les problèmes : dans RTX Encore Hub, ouvrez la page **Santé** du jeu, choisissez **Préparer un diagnostic**, relisez l’archive pour retirer les informations personnelles, puis joignez-la à une [issue RTX Encore](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose). Indiquez les moteurs choisis, l’API graphique et le multiplicateur demandé. Le Hub ne téléverse pas l’archive à votre place.

[Intel XeSS et XeFG (expérimental)](docs/INTEL-ENGINES.fr.md) · Réglages NR par passe, entrée HDR/exposition commune et politique explicite de compatibilité des fournisseurs.

## Ce qu'il fait

| | Fonction | En bref |
| :---: | :--- | :--- |
| 🎞️ | **[Génération d'images X2 à X6](docs/FRAME-GENERATION.fr.md)** | DLSS Multi Frame Generation sur RTX 20, 30 et 40, dans les jeux qui proposent DLSS Frame Generation. Multiplicateur fixe, mode Dynamique avec une fréquence cible, ou choix laissé au jeu. |
| 🛡️ | **Images générées plus propres** | Protection contre les déchirures sur la géométrie fine (grillages, câbles, feuillages) et contre les défauts dans les ombres en mouvement ; interface et texte nets. |
| ⚡ | **Génération d'images plus légère** | Une option de performance qui réduit le coût GPU de chaque image générée sans changer l'image. |
| 🌀 | **[Smooth Motion sur RTX 30](docs/SMOOTH-MOTION.fr.md)** | L'interpolation d'images du pilote NVIDIA, pour les jeux DirectX 11, DirectX 12 et Vulkan, y compris sans DLSS. Désactivé par défaut. |
| ✨ | **[Neural Rendering](docs/NEURAL-RENDERING.fr.md)** | NR sur RTX 20, 30 et 40, avec les options de performance Ampere développées par RTX Encore : traitement optimisé, précision Fast, résolution NR indépendante et mode ultra-rapide Open. Une à quatre passes, un style pour chacune, prise en charge du HDR. Expérimental, désactivé par défaut. |
| 🎛️ | **[Menu et overlay intégrés](docs/MENU-AND-OVERLAY.fr.md)** | Appuyez sur **Inser** dans le jeu : tous les réglages, l'état de chaque fonction, et un overlay avec la fréquence d'images, le multiplicateur, la régularité des images, le GPU et la VRAM. DirectX 11, DirectX 12 et Vulkan. |
| 🚀 | **[Fast super resolution sur RTX 30](docs/SUPER-RESOLUTION.fr.md)** | Un DLSS Super Resolution plus rapide pour RTX 30 dans les jeux DirectX 12 avec le preset M ou L : environ 16 à 20 % de temps GPU en moins par image upscalée sur le système mesuré, pour une image très proche de l'originale. Désactivée par défaut. |
| 🔍 | **Résolution de rendu DLSS** | Choisissez DLAA, Qualité, Équilibré, Performance, Ultra Performance ou une échelle personnalisée dans les jeux qui utilisent déjà DLSS. |
| 🧩 | **Un seul fichier universel** | `rtx-encore.dll` prend le nom que le jeu charge (`version.dll`, `dinput8.dll`, `winmm.dll`, `dxgi.dll` et quinze autres) ou s'installe comme plugin `.asi`. |

## Neural Rendering pensé pour Ampere

**Rendre NR plus exploitable sur RTX 30 est un axe majeur du développement de RTX Encore.** Nos travaux couvrent
un traitement plus rapide avec une sortie identique dans les tests locaux de référence, une précision Fast
optionnelle, une résolution NR indépendante du DLSS, une empreinte mémoire Open réduite et des options de
performance pour notre intégration d'OpenDLSS-NR.

Le **mode ultra-rapide** Open, **Ultra-fast mode**, a réduit le temps NR de **18,7 à 9,8 ms (−48 %)** et porté
les FPS affichés de **73,8 à 94,9 (+29 %)** dans le benchmark de Black Myth: Wukong sur un RTX 3070 Ti Laptop avec
génération d'images X3. C'est une comparaison existante avec un passage par configuration, pas un gain garanti.
Ce mode est expérimental, demande une seule passe NR et peut introduire du retard ou des traînées dans la couche NR
en mouvement.

La **résolution Ultra Performance** est une option distincte : réduisez la taille de travail NR en conservant votre
réglage de rendu DLSS préféré. Les autres réglages Open pour RTX 30 permettent d'échanger de petites différences
d'image ou davantage de VRAM contre moins de temps NR.
**[Voir les mesures, les réglages et les compromis](docs/NEURAL-RENDERING.fr.md).**

## Matériel pris en charge

| Carte | Génération d'images | Smooth Motion | Neural Rendering |
| :--- | :---: | :---: | :---: |
| RTX 40 | ✅ | fourni par NVIDIA | ✅ |
| RTX 30 | ✅ | ✅ | ✅ |
| RTX 20 | 🧪 expérimental | — | 🧪 expérimental |

RTX Encore est fait pour les RTX 20, 30 et 40 ; les RTX 50 tiennent la génération d'images et Smooth Motion de
NVIDIA. Détails, API graphiques, pilotes et notes par jeu : **[Compatibilité](docs/COMPATIBILITY.fr.md)**.

## Installer et mettre à jour avec RTX Encore Hub

**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** est le compagnon Windows de RTX Encore : la façon la plus simple d'installer le mod, et
de le tenir à jour avec les fichiers NVIDIA qui vont avec.

| | Avec le Hub |
| :---: | :--- |
| 🎮 | **Vos jeux au même endroit.** Les jeux Steam, Epic et GOG sont trouvés tout seuls, et la fiche de chaque jeu dit ce que RTX Encore peut y faire sur votre carte et votre pilote. |
| 📦 | **Les bons fichiers NVIDIA, pris pour vous.** Les versions de DLSS et de Streamline les plus récentes que ce RTX Encore prend en charge, les plugins Streamline restant un jeu complet, et le fichier Neural Rendering de NVIDIA téléchargé et vérifié. Beaucoup de problèmes signalés viennent de fichiers DLSS ou Streamline restés anciens. |
| 🔄 | **Des mises à jour en un clic.** Un nouveau RTX Encore est annoncé avec ses notes, **Tout mettre à jour** met à jour tous les jeux, et le Hub se met à jour lui-même. |
| ↩️ | **Un retour arrière.** Chaque fichier remplacé est sauvegardé d'abord, et **Restaurer le jeu** remet le jeu comme il était. |
| 🎚️ | **Des réglages expliqués.** Un formulaire pour le fichier de réglages, où ce qui n'a pas d'effet sur votre carte est grisé. |
| 🩺 | **Une première vérification.** **Lancer et vérifier** observe la première minute du jeu et dit si le mod s'est chargé. |
| 👥 | **Des profils de la communauté.** Des réglages par jeu partagés par d'autres joueurs, classés pour votre machine, à importer avec un retour arrière, ou à partager. Nouveaux depuis le Hub 1.0.0-rc.2, et expérimentaux. |

Téléchargez-le depuis **[ses publications](https://github.com/SilyNoMeta/rtx-encore-hub/releases)**. Le Hub est une version candidate : l'installation à
la main, ci-dessous, fonctionne exactement comme avant.

## Démarrage rapide

Le plus simple est **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** : installez-le, ouvrez la fiche du jeu et appuyez sur **Installer**. À la main :

1. Téléchargez la **[dernière version](https://github.com/SilyNoMeta/rtx-encore/releases)** et extrayez-la.
2. Copiez `rtx-encore.dll` à côté de l'exécutable du jeu et renommez-le `version.dll`.
3. Lancez le jeu. Le menu s'ouvre une fois au premier lancement ; **Inser** le rouvre.
4. Activez DLSS Frame Generation dans les réglages du jeu. S'il l'était déjà, désactivez-le puis réactivez-le.

Autres noms de fichier, plugin `.asi`, mise à jour et désinstallation : **[Installation](docs/INSTALLATION.fr.md)**.

> [!TIP]
> Commencez avec les réglages par défaut : le jeu, ou NVIDIA Profile Inspector, choisit le multiplicateur. Sur une
> carte de 8 Go, X2 ou X3 (ou X4 avec des textures Élevées ou Moyennes) est un point de départ plus sûr.

## Commandes

| Raccourci | Action |
| :--- | :--- |
| `Inser` | Ouvrir ou fermer le menu |
| `Ctrl + Alt + 2…6` | Multiplicateur fixe X2 à X6 |
| `Ctrl + Alt + Page préc. / Page suiv.` | Augmenter ou réduire le multiplicateur |
| `Ctrl + Alt + D` | Basculer entre les modes Fixe et Dynamique |
| `Ctrl + Alt + Haut / Bas` | Augmenter ou réduire la cible du mode Dynamique de 5 FPS (`Maj` : 1 FPS) |
| `Ctrl + Alt + G` | Rendre le choix au jeu |
| `Ctrl + Alt + O` / `P` | Afficher ou masquer l'overlay / le déplacer dans le coin suivant |

Chaque raccourci se change dans le menu. Ils n'agissent que lorsque la fenêtre du jeu a le focus.

## Documentation

| Guide | English | Français | 中文 |
| :--- | :---: | :---: | :---: |
| Installation, mise à jour, désinstallation | [Open](docs/INSTALLATION.md) | [Ouvrir](docs/INSTALLATION.fr.md) | [打开](docs/INSTALLATION.zh-CN.md) |
| Génération d'images | [Open](docs/FRAME-GENERATION.md) | [Ouvrir](docs/FRAME-GENERATION.fr.md) | [打开](docs/FRAME-GENERATION.zh-CN.md) |
| Super Resolution | [Open](docs/SUPER-RESOLUTION.md) | [Ouvrir](docs/SUPER-RESOLUTION.fr.md) | [打开](docs/SUPER-RESOLUTION.zh-CN.md) |
| Smooth Motion sur RTX 30 | [Open](docs/SMOOTH-MOTION.md) | [Ouvrir](docs/SMOOTH-MOTION.fr.md) | [打开](docs/SMOOTH-MOTION.zh-CN.md) |
| Neural Rendering | [Open](docs/NEURAL-RENDERING.md) | [Ouvrir](docs/NEURAL-RENDERING.fr.md) | [打开](docs/NEURAL-RENDERING.zh-CN.md) |
| Menu et overlay | [Open](docs/MENU-AND-OVERLAY.md) | [Ouvrir](docs/MENU-AND-OVERLAY.fr.md) | [打开](docs/MENU-AND-OVERLAY.zh-CN.md) |
| Fichier de réglages | [Open](docs/SETTINGS.md) | [Ouvrir](docs/SETTINGS.fr.md) | [打开](docs/SETTINGS.zh-CN.md) |
| Compatibilité | [Open](docs/COMPATIBILITY.md) | [Ouvrir](docs/COMPATIBILITY.fr.md) | [打开](docs/COMPATIBILITY.zh-CN.md) |
| Dépannage | [Open](docs/TROUBLESHOOTING.md) | [Ouvrir](docs/TROUBLESHOOTING.fr.md) | [打开](docs/TROUBLESHOOTING.zh-CN.md) |
| Journal des versions | [Open](CHANGELOG.md) | [Ouvrir](CHANGELOG.fr.md) | [打开](CHANGELOG.zh-CN.md) |

## Versions publiées

RTX Encore est distribué sous forme de versions prêtes à l'emploi sur cette page ; son code source n'est pas publié.
GitHub affiche le SHA-256 de chaque archive téléchargeable : comparez-le à celui des notes de version avant
d'installer.

La numérotation commence à 1.0.0. Les versions publiées auparavant sous le nom DLSSG-Transfusion, jusqu'à
`v1.4.5.3-rtx20-30-40`, ne sont plus distribuées ; la
[mise à niveau](docs/INSTALLATION.fr.md#mise-à-niveau-depuis-un-ancien-nom) conserve vos réglages.

## Signaler un problème

Ouvrez un [ticket](https://github.com/SilyNoMeta/rtx-encore/issues) en indiquant le jeu et son API graphique, votre
carte et la version du pilote, et le rapport que le menu copie pour vous (onglet **System**, bouton **Copy**).
Le [dépannage](docs/TROUBLESHOOTING.fr.md) donne les premières vérifications.

## Crédits

RTX Encore a commencé comme une suite de [DLSSG-Transfusion](https://github.com/TonyJoaca/DLSSG-Transfusion) de
TonyJoaca et est devenu depuis un projet à part entière. Il doit aussi beaucoup aux travaux de :

- [dashdogy/RTX40MFG-Unlock](https://github.com/dashdogy/RTX40MFG-Unlock) : recherche sur la génération d'images, rendu du menu et gestion des entrées ;
- [mavismmg/MFGAdaUnlock-RenoDx](https://github.com/mavismmg/MFGAdaUnlock-RenoDx) : recherche sur la qualité de la génération d'images ;
- [sdli1995/dlssg_for_sm86](https://github.com/sdli1995/dlssg_for_sm86) : recherche sur la génération d'images pour RTX 20 et RTX 30 ;
- [Intel XeSS](https://github.com/intel/xess) : interfaces SDK XeSS, XeFG et XeLL ;
- [maanHimself/OpenDLSS-NR](https://github.com/maanHimself/OpenDLSS-NR) : le moteur ouvert de Neural Rendering ;
- toutes les personnes qui testent une version et signalent un bug : plusieurs corrections n'existent que grâce à ces rapports.

Les composants inclus dans les versions publiées et leurs licences sont listés dans
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md).

## Avertissement

RTX Encore est fourni tel quel, sans garantie : vous l'utilisez à vos risques. La qualité d'image, la stabilité et
les performances varient selon le jeu, sa version, le pilote et le matériel. Les multiplicateurs élevés augmentent
la latence et l'occupation de la VRAM.

Les composants inclus dans les versions publiées gardent leur propre licence : voir
[THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md), qui doit accompagner toute copie d'une version. NVIDIA, GeForce,
RTX et DLSS sont des marques de NVIDIA Corporation. Les logiciels NVIDIA et les jeux restent soumis à leurs propres
conditions ; aucun n'est inclus dans les versions publiées.
