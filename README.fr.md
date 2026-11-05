<div align="center">

<h1>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/wordmark-dark-theme.png">
  <img src="assets/wordmark-light-theme.png" alt="RTX Encore" width="440">
</picture>
</h1>

**Génération d'images jusqu'à X6, Smooth Motion et Neural Rendering\
pour les GeForce RTX 20, RTX 30 et RTX 40 — désormais aussi dans les jeux sans DLSS.**

Un menu intégré, avec RTX Encore Hub pour installer le mod et les runtimes nécessaires.

[![Dernière version](https://img.shields.io/github/v/release/SilyNoMeta/rtx-encore?include_prereleases&style=for-the-badge&label=Version&color=76b900)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Téléchargements](https://img.shields.io/github/downloads/SilyNoMeta/rtx-encore/total?style=for-the-badge&label=T%C3%A9l%C3%A9chargements&color=2f81f7)](https://github.com/SilyNoMeta/rtx-encore/releases)
[![Sponsor](https://img.shields.io/badge/Sponsor-%E2%9D%A4-db61a2?style=for-the-badge&logo=githubsponsors&logoColor=white)](https://github.com/sponsors/SilyNoMeta)
[![Ko-fi](https://img.shields.io/badge/Ko--fi-Support-ff5e5b?style=for-the-badge&logo=kofi&logoColor=white)](https://ko-fi.com/silynometa)
[![Rejoindre le Discord](https://img.shields.io/badge/Rejoindre_Discord-454FC4?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/Zwmq4Xc2Br)

[![Cartes](https://img.shields.io/badge/Cartes-RTX%2020%20%7C%2030%20%7C%2040-76b900?style=flat-square&logo=nvidia&logoColor=white)](docs/COMPATIBILITY.fr.md)
[![API graphiques](https://img.shields.io/badge/API-DirectX%2011%20%7C%2012%20%7C%20Vulkan-0078d4?style=flat-square)](docs/COMPATIBILITY.fr.md)

**[Télécharger](https://github.com/SilyNoMeta/rtx-encore/releases)** ·
**[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** ·
**[Installation](docs/INSTALLATION.fr.md)** ·
**[English](README.md)** ·
**[中文](README.zh-CN.md)**

<sub>Gratuit, développé sur du temps personnel. S'il vous est utile, vous pouvez soutenir le projet sur <a href="https://github.com/sponsors/SilyNoMeta">GitHub Sponsors</a> ou <a href="https://ko-fi.com/silynometa">Ko-fi</a> ❤</sub>

<p>❤️ Merci à <a href="https://github.com/mennogreg">@mennogreg</a> et <a href="https://github.com/Scarhead666">@Scarhead666</a> pour leur soutien au développement de RTX Encore.</p>

</div>

**Le Discord de RTX Encore est ouvert.** Partagez vos essais de la 2.0.0, comparez vos réglages ou demandez de l’aide pour le mod et le Hub. [Rejoindre le Discord](https://discord.gg/Zwmq4Xc2Br).

> [!IMPORTANT]
> RTX Encore est un mod indépendant et expérimental. Il n'est ni affilié à NVIDIA ni approuvé par NVIDIA.
> Il se charge dans le jeu : ne l'utilisez pas dans un jeu en ligne ou protégé par un anti-triche, où il peut être
> bloqué ou faire signaler un compte. Sauvegardez chaque fichier que vous remplacez.

**[Dernière version : 2.0.0](https://github.com/SilyNoMeta/rtx-encore/releases/tag/v2.0.0)** · [Notes de version](CHANGELOG.fr.md) · [Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0)


## 2.0.0 : DLSS, XeSS, Neural Rendering et génération d'images pour les jeux sans DLSS

Beaucoup de jeux n'ont aucun DLSS. RTX Encore 2.0.0 leur apporte **NVIDIA DLSS Super Resolution ou DLAA, Intel XeSS, le Neural Rendering et la génération d'images**, à partir de l'image qu'ils affichent, en **DirectX 11, DirectX 12 et Vulkan**, sans ReShade ni aucun autre outil. Le jeu n'a besoin de rien : ni DLSS, ni vecteurs de mouvement, ni génération d'images à lui.

| | |
| :--- | :--- |
| 🔍 **Reconstruction** | Un DLAA à la résolution du jeu, ou une résolution de jeu plus basse agrandie jusqu'à celle de l'écran, avec **DLSS** ou **XeSS** (XeSS sur les Tensor Cores NVIDIA en DirectX 12). |
| ✨ **Neural Rendering** | Le NR de NVIDIA sur l'image affichée, avec tous les réglages NR de RTX Encore. |
| 🎞️ **Génération d'images** | **DLSS Frame Generation** X2 à X6 en DirectX 11, DirectX 12 et Vulkan ; **Intel XeFG** jusqu'à X6 en DirectX 12 (Tensor Cores), Vulkan (HDR10 compris) et DirectX 11. Changez le multiplicateur en jouant. |
| 🧭 **Estimation du mouvement** | L'estimation propre à RTX Encore, sur la carte graphique, ou la méthode shader de vort_Motion, ou NVIDIA Optical Flow. |

Appuyez sur **Inser**, puis choisissez le **Moteur** dans Reconstruction et Génération d’images : **Aucun**, **DLSS / XeSS** ou **DLSS-FG / XeFG** selon la fonction. Les intégrations natives conservent **Jeu**. [Installation, prérequis et limites](docs/GAMES-WITHOUT-DLSS.fr.md).

| 🎞️ **[Génération d’images](docs/FRAME-GENERATION.fr.md)** | 🌀 **[Smooth Motion](docs/SMOOTH-MOTION.fr.md)** | ✨ **[Neural Rendering](docs/NEURAL-RENDERING.fr.md)** | 🎮 **[Jeux sans DLSS](docs/GAMES-WITHOUT-DLSS.fr.md)** |
| :--- | :--- | :--- | :--- |
| Choisissez le multiplicateur dans les jeux compatibles. | Interpolation pour les jeux sans FG natif. | Ajustez l’image avec des réglages NR indépendants. | DLSS, XeSS, NR et génération d'images là où le jeu n'en a pas. |

## Lancer le jeu

Installez avec **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, choisissez votre jeu, puis appuyez sur **Inser** en jeu. Installation manuelle ? [Les quatre étapes sont ici](#démarrage-rapide).

<details>
<summary>Moteurs expérimentaux et aide</summary>

XeSS, XeFG et l’adaptateur XeFG natif → DLSS FG sont expérimentaux et exigent des entrées et runtimes compatibles. Dans Mirage, choisissez DLSS-FG dans Encore, redémarrez, puis activez le FG Intel dans le jeu. Le FSR reste natif. [Guide des moteurs](docs/INTEL-ENGINES.fr.md). En cas de problème, préparez un diagnostic depuis la page Santé du Hub, relisez-le, puis joignez-le à une [issue](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose).

</details>

## Ce qu'il fait

| | Fonction | En bref |
| :---: | :--- | :--- |
| 🧪 | **[Moteurs Intel (expérimental)](docs/INTEL-ENGINES.fr.md)** | Reconstruction XeSS ou génération d’images XeFG avec XeLL, selon les entrées du jeu et les runtimes compatibles. Désactivés par défaut. |
| 🔆 | **[Netteté à chaud](docs/SUPER-RESOLUTION.fr.md)** | Off / Game / Custom, après reconstruction et NR. Custom de 0 à 100 %, défaut 30 % ; rendu et fonctionnement Vulkan encore à valider. |
| 🎞️ | **[Génération d'images X2 à X6](docs/FRAME-GENERATION.fr.md)** | DLSS Multi Frame Generation sur RTX 20, 30 et 40, dans les jeux à génération DLSS ou Intel native compatible. Multiplicateur fixe, mode Dynamique avec une fréquence cible, ou choix laissé au jeu. |
| 🛡️ | **Images générées plus propres** | Protection contre les déchirures sur la géométrie fine (grillages, câbles, feuillages) et contre les défauts dans les ombres en mouvement ; interface et texte nets. |
| ⚡ | **Génération d'images plus légère** | Une option de performance qui réduit le coût GPU de chaque image générée sans changer l'image. Poussée encore plus loin dans la 1.6.0 : sur RTX 4090, une image générée coûte 34 % de temps GPU de moins qu'option désactivée en 1080p X2, 16 % en 1080p X6. |
| 🌀 | **[Smooth Motion sur RTX 20 / 30](docs/SMOOTH-MOTION.fr.md)** | L'interpolation d'images du pilote NVIDIA, y compris sans DLSS : DirectX 11, DirectX 12 et Vulkan sur RTX 30 ; prise en charge **expérimentale sur RTX 20**, en DirectX 12 avec le pilote 621.01 et les variantes V1 / V2. Désactivé par défaut. |
| ✨ | **[Neural Rendering](docs/NEURAL-RENDERING.fr.md)** | NR sur RTX 20, 30 et 40, avec les options de performance Ampere développées par RTX Encore : traitement optimisé, précision Fast, résolution NR indépendante. Une à quatre passes, un style pour chacune, prise en charge du HDR. Expérimental, désactivé par défaut. |
| 🎮 | **[Jeux sans DLSS](docs/GAMES-WITHOUT-DLSS.fr.md)** | Reconstruction DLSS ou XeSS (DLAA ou jusqu’à la résolution de l’écran), Neural Rendering et génération d’images (DLSS Frame Generation ou Intel XeFG, X2 à X6) dans les jeux DirectX 11, DirectX 12 et Vulkan qui n’ont pas de DLSS, à partir de l’image qu’ils affichent, avec l’estimation du mouvement propre à RTX Encore. Nouveau dans la 2.0.0. Expérimental, désactivé par défaut. |
| ⚡ | **[XeSS et XeFG sur les Tensor Cores NVIDIA](docs/INTEL-ENGINES.fr.md)** | Deux options expérimentales indépendantes font tourner la reconstruction XeSS et la génération d’images XeFG sur les Tensor Cores NVIDIA, en DirectX 12, dans les jeux avec DLSS comme dans les jeux sans. Désactivées par défaut ; les puces du menu portent un badge **Tensor Cores** tant qu’elles tournent. Qualifiées sur RTX 4090 / NVIDIA 621.01 ; les autres RTX 30, 40 et 50 sont admises sans validation en jeu. |
| 🎛️ | **[Menu et overlay intégrés](docs/MENU-AND-OVERLAY.fr.md)** | Appuyez sur **Inser** dans le jeu : tous les réglages, l'état de chaque fonction, et un overlay avec la fréquence d'images, le multiplicateur, la régularité des images, le GPU et la VRAM. DirectX 11, DirectX 12 et Vulkan. En onze langues depuis la 2.0.0. |
| 🚀 | **[Fast super resolution sur RTX 30](docs/SUPER-RESOLUTION.fr.md)** | Un DLSS Super Resolution plus rapide pour RTX 30 dans les jeux DirectX 12 avec le preset M ou L : environ 16 à 20 % de temps GPU en moins par image upscalée sur le système mesuré, pour une image très proche de l'originale. Désactivée par défaut. |
| 🔍 | **Résolution de rendu DLSS** | Choisissez DLAA, Ultra (75 %), Qualité, Équilibré, Performance, Ultra Performance ou une échelle personnalisée dans les jeux qui utilisent déjà DLSS. |
| 🧩 | **Un seul fichier universel** | `rtx-encore.dll` prend le nom que le jeu charge (`version.dll`, `dinput8.dll`, `winmm.dll`, `dxgi.dll` et quinze autres) ou s'installe comme plugin `.asi`. |

## Neural Rendering pensé pour Ampere

RTX Encore propose un traitement NVIDIA NR optimisé, une précision Fast optionnelle et une résolution NR indépendante du DLSS. Une à quatre passes et les réglages de rendu par passe restent disponibles. [Voir les réglages et mesures](docs/NEURAL-RENDERING.fr.md).

## Matériel pris en charge

| Carte | Génération d'images | Smooth Motion | Neural Rendering |
| :--- | :---: | :---: | :---: |
| RTX 40 | ✅ | fourni par NVIDIA | ✅ |
| RTX 30 | ✅ | ✅ | ✅ |
| RTX 20 | 🧪 expérimental | 🧪 expérimental (DirectX 12, pilote 621.01) | 🧪 expérimental |

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

Téléchargez le Hub stable depuis **[ses publications](https://github.com/SilyNoMeta/rtx-encore-hub/releases)**, en installateur ou ZIP portable. L’installation manuelle, ci-dessous, reste disponible.

## Démarrage rapide

Le plus simple est **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)** : installez-le, ouvrez la fiche du jeu et appuyez sur **Installer**. À la main :

1. Téléchargez la **[dernière version](https://github.com/SilyNoMeta/rtx-encore/releases)** et extrayez-la.
2. Copiez `rtx-encore.dll` à côté de l'exécutable du jeu et renommez-le `version.dll`.
3. Lancez le jeu. Le menu s'ouvre une fois au premier lancement ; **Inser** le rouvre.
4. Activez DLSS Frame Generation dans les réglages du jeu. S'il l'était déjà, désactivez-le puis réactivez-le. Dans un
   jeu sans DLSS, activez plutôt ce que vous voulez dans le menu : voir **[Jeux sans DLSS](docs/GAMES-WITHOUT-DLSS.fr.md)**.

Autres noms de fichier, plugin `.asi`, mise à jour et désinstallation : **[Installation](docs/INSTALLATION.fr.md)**.

> [!TIP]
> Commencez avec les réglages par défaut : le jeu, ou NVIDIA Profile Inspector, choisit le multiplicateur. Sur une
> carte de 8 Go, X2 ou X3 (ou X4 avec des textures Élevées ou Moyennes) est un point de départ plus sûr.

### Composants pour une installation manuelle

Ce que RTX Encore ajoute aux jeux sans DLSS ou sans génération d'images fonctionne avec les composants exacts avec lesquels il a été validé ; le Hub les met en place. À la main, prenez ces fichiers, puis relancez le jeu :

| Pour | Fichiers | Téléchargement | Où les mettre |
|---|---|---|---|
| DLSS Frame Generation 310.9.16 et Streamline 2.14.16 | `nvngx_dlssg.dll` et les fichiers `sl.*.dll`, dans `bin/x64` (pas `development`) | [SDK NVIDIA Streamline 2.14.16](https://github.com/NVIDIA-RTX/Streamline/releases/download/v2.14.16/streamline-sdk-v2.14.16.zip) (tout le dossier `bin/x64` convient) | à côté de l'exécutable du jeu |
| DLSS / DLAA 310.9.16 | `nvngx_dlss.dll` | [nvngx_dlss-310.9.16.zip](https://encore-dl.nometa.fr/nvngx_dlss-310.9.16.zip) | à côté de l'exécutable du jeu |
| Neural Rendering 310.9.16 | `nvngx_dlssnr.dll` | [nvngx_dlssnr-310.9.16.zip](https://encore-dl.nometa.fr/nvngx_dlssnr-310.9.16.zip) | à côté de la DLL de RTX Encore |
| XeSS-SR 2.0.2.68, XeFG 1.3.1.78, XeLL 1.3.2.10 | `libxess.dll`, `libxess_fg.dll`, `libxell.dll`, dans `bin` | [SDK Intel XeSS 3.0.2](https://github.com/intel/xess/releases/download/v3.0.2/XeSS_SDK_3.0.2.zip) | à côté de la DLL de RTX Encore, ou dans `rtx-encore-runtimes\intel` à côté d'elle |

Uniquement ces fichiers exacts : les autres builds ou versions sont refusées, et le réglage **Moteur** du menu liste ce qu'il a trouvé.

## Commandes

| Raccourci | Action |
| :--- | :--- |
| `Inser` | Ouvrir ou fermer le menu |
| `Ctrl + Alt + 2…6` (pavé numérique également) | Multiplicateur fixe X2 à X6 |
| `Ctrl + Alt + Page préc. / Page suiv.` | Augmenter ou réduire le multiplicateur |
| `Ctrl + Alt + D` | Basculer entre les modes Fixe et Dynamique |
| `Ctrl + Alt + Haut / Bas` ou `Ctrl + Alt + Plus / Moins` (également `+` / `−` du pavé numérique) | Augmenter ou réduire la cible du mode Dynamique de 5 FPS (maintenir aussi `Maj` : 1 FPS) |
| `Ctrl + Alt + G` | Rendre le choix au jeu |
| `Ctrl + Alt + O` | Afficher ou masquer l'overlay |
| `Ctrl + Alt + P` | Déplacer l'overlay dans le coin suivant |
| `Ctrl + Alt + N` | Activer ou désactiver le Neural Rendering |
| `Ctrl + Alt + F9` | Enregistrer une rafale de captures SR (DirectX 12 ; activer d'abord la capture SR dans le menu) |

Ce sont les raccourcis par défaut ; chacun se change dans le menu. Ils n'agissent que lorsque la fenêtre du jeu a le focus.

## Documentation

| Guide | English | Français | 中文 |
| :--- | :---: | :---: | :---: |
| Installation, mise à jour, désinstallation | [Open](docs/INSTALLATION.md) | [Ouvrir](docs/INSTALLATION.fr.md) | [打开](docs/INSTALLATION.zh-CN.md) |
| Génération d'images | [Open](docs/FRAME-GENERATION.md) | [Ouvrir](docs/FRAME-GENERATION.fr.md) | [打开](docs/FRAME-GENERATION.zh-CN.md) |
| Super Resolution | [Open](docs/SUPER-RESOLUTION.md) | [Ouvrir](docs/SUPER-RESOLUTION.fr.md) | [打开](docs/SUPER-RESOLUTION.zh-CN.md) |
| Smooth Motion sur RTX 20 / 30 | [Open](docs/SMOOTH-MOTION.md) | [Ouvrir](docs/SMOOTH-MOTION.fr.md) | [打开](docs/SMOOTH-MOTION.zh-CN.md) |
| Neural Rendering | [Open](docs/NEURAL-RENDERING.md) | [Ouvrir](docs/NEURAL-RENDERING.fr.md) | [打开](docs/NEURAL-RENDERING.zh-CN.md) |
| Jeux sans DLSS | [Open](docs/GAMES-WITHOUT-DLSS.md) | [Ouvrir](docs/GAMES-WITHOUT-DLSS.fr.md) | [打开](docs/GAMES-WITHOUT-DLSS.zh-CN.md) |
| Intel XeSS et XeFG | [Open](docs/INTEL-ENGINES.md) | [Ouvrir](docs/INTEL-ENGINES.fr.md) | [打开](docs/INTEL-ENGINES.zh-CN.md) |
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
- [maximedgoulet/FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5) : l’idée du Neural Rendering fovéal ;
- [jlrouzies-fr/DLSS5-Feeder](https://github.com/jlrouzies-fr/DLSS5-Feeder) : le Neural Rendering alimenté par l’image affichée, et sa validation du mouvement ;
- [vortigern11/vort_Shaders](https://github.com/vortigern11/vort_Shaders) (vort_Motion) : l’estimation du mouvement par shaders pour les jeux sans DLSS ;
- [crosire/reshade](https://github.com/crosire/reshade) (add-on Generic Depth) : la façon de trouver le depth buffer dans lequel dessine un jeu ;
- [Dear ImGui](https://github.com/ocornut/imgui) d’Omar Cornut : la bibliothèque avec laquelle sont faits le menu en jeu et l’overlay ;
- [Noto Sans SC](https://github.com/google/fonts/tree/main/ofl/notosanssc) : police du menu ;
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
