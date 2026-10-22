# Journal des versions

*[English](CHANGELOG.md) · [中文](CHANGELOG.zh-CN.md)*

## 1.2.0

Contient tout ce qu'apporte la 1.2.0-beta.1. Intel XeSS et XeFG restent **expérimentaux**.

### Nouveautés

- **Les presets DLSS et le moteur de reconstruction changent en cours de jeu.** Les presets de Super Resolution et
  de Ray Reconstruction, et le choix entre DLSS et XeSS Super Resolution, s'appliquent désormais sans redémarrage
  dans les jeux Direct3D 12, Direct3D 11 et Vulkan. Les presets de Frame Generation et le moteur de génération
  d'images demandent toujours un redémarrage. Dans les jeux Vulkan, XeSS doit déjà être choisi au lancement du jeu.
- **Smooth Motion avec le pilote NVIDIA 621.01** (RTX 30). Vérifié dans Enshrouded (Vulkan) sur le PC du mainteneur.
- **Smooth Motion dans les jeux Vulkan** fonctionne désormais toujours là où le profil NVIDIA du jeu l'exclut. Le
  réglage **Force Smooth Motion** disparaît ; une ligne `smoothMotionForce` restée dans `rtx-encore.jsonc` est
  ignorée. NVIDIA peut exclure un jeu pour une raison : des résultats inattendus restent possibles. Voir
  [Smooth Motion](docs/SMOOTH-MOTION.fr.md).
- **Le menu nomme le moteur et le preset.** La pastille Upscaling affiche DLSS SR, DLSS RR ou XeSS-SR et, pour DLSS,
  le preset demandé à la création ; la pastille Frame Generation affiche DLSS-G et le preset FG fourni par RTX
  Encore. Le pilote n'indique pas le modèle qu'il exécute réellement.
- Les modèles DLSS fournis par le pilote NVIDIA (par exemple via la surcharge DLSS de l'application NVIDIA) sont
  désormais reconnus par le menu.

### Corrigé

- **Smooth Motion pouvait rester éteint dans les jeux Vulkan** : il démarre maintenant de façon fiable quand le
  pilote est rechargé pendant le démarrage du jeu.
- **XeFG** : redimensionnement de la fenêtre dans Cyberpunk 2077, taille en fenêtre sans bordure, génération
  multi-images pilotée par le jeu (X4 vérifié dans GTA V Enhanced), menu affiché sur la présentation de XeFG, et XeFG
  créé avant l'initialisation Streamline du jeu.
- **XeSS Super Resolution** : jeux Direct3D 12 enregistrant sur leurs propres listes de commandes, état de calcul du
  jeu restauré après XeSS, intégrations Vulkan anciennes dirigées vers XeSS, et presets du repli DLSS mis à jour en
  direct.
- Le menu avertit de XeSS avec Ray Reconstruction seulement quand le jeu exécute réellement Ray Reconstruction.

Vérifié par le mainteneur : Smooth Motion dans Enshrouded avec le pilote 621.01, XeFG X2 et X4 dans Cyberpunk 2077,
XeFG X4 dans GTA V Enhanced, XeSS SR dans 007 First Light. Le changement de preset en cours de jeu n'a que des tests
automatisés ; les autres jeux, GPU et pilotes n'ont pas été testés.

### Problème connu

- **007 First Light** peut encore planter par intermittence au démarrage ; un autre lancement peut réussir.
- **L'enregistreur de SCSKiller n'est pas compatible avec RTX Encore.** Quand SCSKiller enregistre un jeu, son
  enregistreur prend la création de la swap chain du jeu avant RTX Encore : XeFG est refusé et le DLSS-G du jeu reste
  utilisé (le chip Frame Generation affiche DLSS-G). Dans The Witcher 3, le jeu pouvait aussi se figer au chargement,
  avec un reset du GPU au démarrage de la génération d'images. Vous pouvez garder SCSKiller : dans ses **Settings**, désactivez **Record in all
  compatible games** (et l'enregistrement sur la page du jeu si vous l'y avez activé), puis relancez le jeu. D'autres
  outils qui interceptent la création de swap chain de DXGI peuvent avoir le même effet.

## 1.2.0-beta.1

**XeSS / XeFG : TRÈS EXPÉRIMENTAL.** Cette intégration a été particulièrement pénible à mettre au point :) Attendez-vous à des bugs, crashs, artefacts et différences entre jeux et API graphiques. N’hésitez pas à remonter les problèmes : dans RTX Encore Hub, ouvrez la page **Santé** du jeu, choisissez **Préparer un diagnostic**, relisez l’archive pour retirer les informations personnelles, puis joignez-la à une [issue RTX Encore](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose). Indiquez les moteurs choisis, l’API graphique et le multiplicateur demandé. Le Hub ne téléverse pas l’archive à votre place.

- Les fichiers JSONC existants sont réordonnés par fonction, avec les options Intel près du choix des moteurs. Les valeurs choisies, commentaires et réglages personnels sont conservés.
- **Ray Reconstruction + Neural Rendering (Direct3D 12)** — Les jeux compatibles utilisant une profondeur linéaire peuvent désormais appliquer NR après RR. Vérifié localement dans 007 First Light en path tracing sur RTX 3070 Ti Laptop : NR fonctionne ; la qualité d’image et le coût n’ont pas été mesurés. Direct3D 11 et Vulkan ne prennent toujours pas en charge ce cas de profondeur linéaire.

- **Intel XeSS et XeFG (expérimental)** — RTX Encore 1.2.0-beta.1 permet de choisir les moteurs de reconstruction et de génération d’images. Laissez **Game** pour conserver le moteur habituel du jeu. Les modes Intel sont expérimentaux, désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d’un moteur ne prouve pas son activation.
- Chacune des quatre passes NR possibles peut remplacer l’intensité, le ton local, la structure locale, la structure de peau et le masque de personnage, ou suivre la valeur globale. Les valeurs numériques vont de 0 à 2 ; le masque prend `inherit`, `true` ou `false`. Après la première passe, le ton hérité vient de `nrLaterPassLocalTone`. Une intensité à zéro laisse la passe tourner et ne supprime pas son coût GPU/VRAM.
- L’entrée HDR et l’exposition automatique utilisent les réglages communs `srHdr` et `srAutoExposure`, pour le moteur de reconstruction sélectionné. Ils suivent le jeu par défaut (`game`) et demandent un redémarrage. Ils n’activent pas le HDR de l’écran et ne convertissent pas une image SDR en HDR. Un format d’entrée incorrect peut dégrader l’image. Certains modèles DLSS imposent l’exposition automatique ; celle de XeSS a un coût GPU et est déconseillée en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge. RR et la génération d’images ne changent pas.
- La politique locale `providerPatchPolicy` vaut `heuristic` par défaut. `exact` limite la compatibilité des fournisseurs NVIDIA aux fichiers examinés ; `heuristic` permet aussi une compatibilité encadrée avec d’autres fichiers NVIDIA signés et admissibles. Cette admission reste non qualifiée et est signalée comme telle. Redémarrez après un changement. Le Hub exclut ce réglage des profils partagés ; ce n’est pas une promesse de compatibilité en jeu.

Des essais Vulkan locaux limités dans No Man’s Sky et DOOM: The Dark Ages étayent la route XeFG X2 ; l’essai DOOM a été accepté par le mainteneur en plein écran fenêtré, NR coupé. Cela ne qualifie ni le plein écran exclusif, ni DirectX 11, ni XeFG avec NR, ni tous les multiplicateurs élevés ou les autres jeux. Le menu et l’overlay peuvent être indisponibles sur une route Intel ; utilisez le Hub pour régler le mod et relire les constats de session.

Le crash intermittent de 007 First Light au démarrage reste non résolu. Les nouveaux forçages HDR/exposition et les admissions de fournisseurs ont des contrôles statiques et automatisés locaux ; cela ne constitue pas une validation visuelle ou en jeu généralisée.

Merci à [@SanyaGames227](https://github.com/SanyaGames227) pour la demande de réglages d’entrée HDR et d’exposition automatique ([#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)). Il s’agit d’une demande de fonction, pas d’une validation de la bêta finale par le rapporteur.

[Intel XeSS et XeFG (expérimental)](docs/INTEL-ENGINES.fr.md)

Merci à [@Theoryedz](https://github.com/Theoryedz) pour le signalement dans [#58](https://github.com/SilyNoMeta/rtx-encore/issues/58). Le rapporteur n’a pas testé la version corrigée.

## 1.1.4

Reprend tout le contenu de la 1.1.3.

### Corrections

- **Neural Rendering scintillait avec deux passes ou plus** ([#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)). Dans Aion 2, l'image scintillait, surtout dans les menus chargés comme la création de personnage, pendant que l'indicateur Neural Rendering passait au jaune puis au vert. Avec deux passes ou plus, une image pouvait rester sans Neural Rendering quand le jeu soumettait son propre travail au même moment. Ce n'est plus le cas.
- **Aion 2 : le curseur du menu restait bloqué au milieu de l'écran**, ce qui empêchait de modifier les réglages. Le jeu ne ramène plus le curseur au centre tant que le menu est ouvert.

Vérifié sur le PC du mainteneur dans Aion 2 avec deux passes : le scintillement n'apparaît plus sur l'écran de création de personnage. Les auteurs des signalements n'ont pas testé cette version. Le mainteneur a depuis confirmé la correction du curseur dans Aion 2. Les autres jeux, les autres cartes graphiques et trois ou quatre passes n'ont pas été testés.

### Problème connu

- **007 First Light** peut encore planter au démarrage de façon intermittente. Cette version ne corrige pas ce problème ; une nouvelle tentative peut réussir.

### Remerciements

- [@unsungloser](https://github.com/unsungloser) — a signalé le scintillement avec plusieurs passes Neural Rendering dans Aion 2 et fourni les logs et une vidéo ([#62](https://github.com/SilyNoMeta/rtx-encore/issues/62)).

## 1.1.3

### Corrections

- **The Witcher 3: Wild Hunt (DirectX 12) plantait au démarrage avec la 1.1.2** ([#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)). Cette version corrige cette régression tout en conservant les améliorations du menu de la 1.1.2.

Vérifié localement sur la copie GOG du mainteneur : le démarrage réussit avec le menu actif, sous `version.dll` comme sous `dxgi.dll`. Il s'agit de vérifications automatisées du démarrage, sans test de jeu prolongé. L'auteur du signalement Steam n'a pas encore testé la correction.

### Problème connu

- **007 First Light** peut encore planter au démarrage de façon intermittente. Cette version ne corrige pas ce problème ; une nouvelle tentative peut réussir.

### Remerciements

- [@Herror7](https://github.com/Herror7) — a signalé la régression au démarrage de The Witcher 3 et fourni les logs qui ont aidé à en identifier la cause ([#60](https://github.com/SilyNoMeta/rtx-encore/issues/60)).

## 1.1.2

Tout ce qu'ont apporté les pré-versions 1.1.2, et les corrections venues après 1.1.2-beta.3.

### Nouveautés

- **Smooth out X3 / X4** ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)). Dans certains jeux, X3 et X4 affichaient les images générées par rafales, si bien que le mouvement paraissait plus saccadé qu'en X2 : Baldur's Gate 3 avec bg3fgvk, par exemple. Avec ce réglage activé, Streamline espace lui-même les images générées au lieu de laisser faire le pilote NVIDIA, désormais dans les jeux Vulkan comme en DirectX 12. Il reste désactivé par défaut : activez-le dans le menu, section Frame generation sous le multiplicateur, dans RTX Encore Hub, ou avec `flipMeteringBypass` dans la section `frameGeneration`, puis relancez le jeu. Un réglage existant est conservé. Voir [Génération d'images](docs/FRAME-GENERATION.fr.md#lisser-x3--x4).
- **Ligne des versions de l'overlay** : elle liste maintenant toutes les DLL NVIDIA chargées par le jeu, Ray Reconstruction et Neural Rendering compris. Les DLL de même version partagent une ligne (`SR-RR-FG-NR 310.9.16`), et une DLL non chargée n'apparaît pas.

### Corrections

- **Neural Rendering faisait planter certains jeux DirectX 12** ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)). Activer Neural Rendering pouvait faire planter le jeu un instant plus tard sur les cartes RTX 20, 30 et 40, quel que soit le pilote : Resident Evil Requiem, Pragmata et 007 First Light, par exemple. Neural Rendering laisse désormais l'état de rendu du jeu tel qu'il l'a trouvé. L'indicateur DLSS de NVIDIA affiché à l'écran pouvait aussi le faire échouer ; Neural Rendering dispose maintenant de tout ce dont l'indicateur a besoin. Si une étape échoue encore au démarrage de Neural Rendering, le journal indique laquelle.
- **Le menu de RTX Encore ne s'affichait pas** (Insert ne faisait rien) dans d'autres jeux :
  - **Pillars of Eternity II: Deadfire** et les autres jeux DirectX 11 qui font leur rendu sur un seul thread, quel que soit le nom sous lequel RTX Encore est installé ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
  - **Avec RTX Encore installé sous le nom `dxgi.dll`**, dans les jeux qui y prennent leur affichage ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
  - **Les jeux avec une ancienne version de NVIDIA Streamline**, comme The Last of Us Part I ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)) et Dying Light 2. Dans Dying Light 2, installez RTX Encore sous le nom `winmm.dll`.
  - **Les jeux DirectX 11** qui créent leur affichage d'une manière que le menu ne suivait pas, y compris ceux dont le rendu est confié à une partie du jeu chargée après le démarrage.
- **Overlays** (RivaTuner Statistics Server, Steam, Epic, Rockstar Social Club, Overwolf, GOG Galaxy, Discord, Ubisoft Connect) : le menu de RTX Encore ne s'attache plus à leurs propres fenêtres ni à leurs petites surfaces auxiliaires, seulement à celles du jeu.

Vérifié en jeu sur le PC du mainteneur (RTX 3070 Ti Laptop) : Resident Evil Requiem plantait dès le démarrage de Neural Rendering et fonctionne maintenant avec ; Black Myth: Wukong fonctionne toujours, avec et sans l'indicateur DLSS ; le menu s'ouvre dans Dying Light 2, Rise of the Tomb Raider et The Blood of Dawnwalker ; dans No Man's Sky, les rafales en X4 disparaissent avec Smooth out X3 / X4. Pas encore confirmés en jeu : Pragmata, 007 First Light, Pillars of Eternity II, The Last of Us Part I et Baldur's Gate 3.

Build Windows Release, les six variantes distribuées ; les tests de régression automatiques passent hors jeu.

### Merci

- [@Theoryedz](https://github.com/Theoryedz) — le signalement dans Resident Evil Requiem, puis des tests dans Pragmata et 007 First Light avec et sans l'indicateur DLSS, avec les journaux et les relevés de crash qui ont montré où le jeu s'arrêtait au démarrage de Neural Rendering ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)).
- [@sewer86](https://github.com/sewer86) — le signalement de Pillars of Eternity II, avec des logs de 1.1.2-beta.1 et 1.1.2-beta.3 qui ont montré où le menu se perdait ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).
- [@deusimperatork](https://github.com/deusimperatork) — le signalement de Baldur's Gate 3, avec des logs et des captures PresentMon qui ont montré les images générées arrivant par rafales en X4 ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)).
- [@SanyaGames227](https://github.com/SanyaGames227) — des logs de The Last of Us Part I avec le pilote 591.86 qui ont montré que le menu ne s'attachait jamais ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)).

## 1.1.2-beta.3 — pré-version

### Corrections

- **Neural Rendering avec l'indicateur DLSS de NVIDIA activé** ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)). Avec l'indicateur DLSS de NVIDIA affiché à l'écran, activer Neural Rendering pouvait faire planter le jeu environ deux secondes plus tard sur les cartes RTX 20, 30 et 40 (signalé dans Resident Evil Requiem sur une RTX 3090 Ti). Neural Rendering dispose maintenant de tout ce dont l'indicateur a besoin. Si une étape échoue encore au démarrage de Neural Rendering, le journal indique laquelle et comment désactiver l'indicateur.

  Vérifié en jeu sur le PC du mainteneur (RTX 3070 Ti Laptop, Black Myth: Wukong, DirectX 12) avec l'indicateur activé : Neural Rendering démarre et fonctionne. La version 1.1.1 ne plantait pas non plus sur ce PC, la correction n'est donc pas encore confirmée dans Resident Evil Requiem.

- **Le menu de RTX Encore ne s'affichait pas avec RTX Encore installé sous le nom `dxgi.dll`** dans les jeux qui y prennent leur affichage, comme Pillars of Eternity II: Deadfire (DirectX 11, [#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)). Pas encore confirmé en jeu : Pillars of Eternity II n'a pas pu être testé sur le PC du mainteneur.

Build Windows Release, les six variantes distribuées ; les tests de régression automatiques passent hors du jeu.

### Merci

- [@Theoryedz](https://github.com/Theoryedz) — le signalement dans Resident Evil Requiem, avec les journaux et les relevés de crash qui ont montré où le jeu s'arrêtait au démarrage de Neural Rendering ([#58](https://github.com/SilyNoMeta/rtx-encore/issues/58)).
- [@sewer86](https://github.com/sewer86) — le signalement de Pillars of Eternity II, avec des logs de 1.1.2-beta.1 qui ont montré où le menu se perdait ([#57](https://github.com/SilyNoMeta/rtx-encore/issues/57)).

## 1.1.2-beta.2 — pré-version

### Nouveautés

- **Le contournement du flip metering fonctionne aussi dans les jeux Vulkan** ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)). Dans certains jeux Vulkan, X3 et X4 affichaient les images générées par rafales, si bien que le mouvement paraissait plus saccadé qu'en X2 : Baldur's Gate 3 avec bg3fgvk, par exemple. Avec le réglage activé, Streamline espace lui-même les images générées au lieu de laisser faire le pilote NVIDIA. Il reste désactivé par défaut : activez-le dans le menu (**Compatibility** > **Flip metering bypass**) ou avec `flipMeteringBypass`, puis relancez le jeu. Voir [Génération d'images](docs/FRAME-GENERATION.fr.md#options-de-compatibilité).

  Vérifié en jeu sur le PC du mainteneur (RTX 3070 Ti Laptop, X4) : dans No Man's Sky, les rafales disparaissent avec le réglage ; dans DOOM: The Dark Ages, le pilote espaçait déjà bien les images et le réglage ne change rien de visible. Baldur's Gate 3 n'est pas encore confirmé.

### Changements

- Le réglage s'appelle maintenant **Flip metering bypass**, et sa description dit quand l'essayer.
- **Ligne des versions de l'overlay** : elle liste maintenant toutes les DLL NVIDIA chargées par le jeu, Ray Reconstruction et Neural Rendering compris. Les DLL de même version partagent une ligne (`SR-RR-FG-NR 310.9.16`), et une DLL non chargée n'apparaît pas.

Build Windows Release, les six variantes distribuées ; les tests de régression automatiques passent hors du jeu.

### Merci

- [@deusimperatork](https://github.com/deusimperatork) — le signalement Baldur's Gate 3, avec des logs et des captures PresentMon qui ont montré les images générées arrivant par rafales en X4 ([#56](https://github.com/SilyNoMeta/rtx-encore/issues/56)).

## 1.1.2-beta.1 — pré-version

### Corrections

- **Le menu de RTX Encore ne s'affichait pas** (Insert ne faisait rien) dans d'autres jeux :
  - **Les jeux avec une ancienne version de NVIDIA Streamline**, comme The Last of Us Part I ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)) et Dying Light 2. Smooth Motion et DLSS y fonctionnaient déjà ; seul le menu manquait. Dans Dying Light 2, installez RTX Encore sous le nom `winmm.dll`.
  - **Les jeux DirectX 11** qui créent leur affichage d'une façon que le menu ne suivait pas encore, y compris ceux dont le rendu est confié à une partie du jeu chargée après le démarrage.
- **Overlays** (RivaTuner Statistics Server, Steam, Epic, Rockstar Social Club, Overwolf, GOG Galaxy, Discord, Ubisoft Connect) : le menu de RTX Encore ne s'attache plus à leurs propres fenêtres ni à leurs petites surfaces auxiliaires, seulement à celle du jeu.

Vérifié en jeu sur le PC du mainteneur (RTX 30) : le menu s'ouvre maintenant dans Dying Light 2 (DirectX 11) ; il s'ouvre toujours dans Rise of the Tomb Raider (Epic Games Store) et The Blood of Dawnwalker. The Last of Us Part I n'a pas pu être testé ici et n'est pas encore confirmé. Build Windows Release, les six variantes distribuées ; les tests de régression automatiques passent hors du jeu.

### Merci

- [@SanyaGames227](https://github.com/SanyaGames227) — les logs de The Last of Us Part I avec le pilote 591.86, qui ont montré que le menu ne s'attachait jamais ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52)).

## 1.1.1

### Corrections

- **Jeux avec l'overlay Epic Online Services** : cet overlay accompagne les jeux qui utilisent les services en ligne d'Epic, quelle que soit la boutique où vous les avez achetés. Deux problèmes liés à lui sont corrigés :
  - **Smooth Motion sur RTX 30 en DirectX 12** : le jeu pouvait s'arrêter avec « Device removed » (`0x887A002B`) peu après le lancement, dès que l'overlay apparaissait. Rise of the Tomb Raider (Epic Games Store) quittait après les conditions d'utilisation ; il atteint maintenant le menu avec Smooth Motion activé.
  - **Le menu de RTX Encore ne s'affichait pas** (Insert ne faisait rien et le message du premier lancement n'apparaissait jamais) dans les jeux qui préparent leur affichage d'une certaine façon, comme Rise of the Tomb Raider, avec ou sans Smooth Motion ([#50](https://github.com/SilyNoMeta/rtx-encore/issues/50), merci à @kakaroto-cmd d'avoir signalé le problème et fourni les logs).

  Les deux corrections ont été vérifiées dans Rise of the Tomb Raider (Epic Games Store, DirectX 12, RTX 3070 Ti Laptop). Le menu a été vérifié avec Smooth Motion désactivé ; les autres jeux avec cet overlay ne sont pas encore confirmés.

## 1.1.0

Cette version arrive avec le **premier sponsoring** de RTX Encore. Merci, sincèrement : c'est beaucoup pour un projet fait sur des soirées libres.

### Nouveautés

- **Fast super resolution (RTX 30)**, désactivée par défaut : sur les cartes RTX 30 en DirectX 12, les presets **M** et **L** de DLSS Super Resolution utilisent une version plus rapide du modèle de NVIDIA. Sur un RTX 3070 Ti Laptop, l'upscaling a pris environ **18 % de temps GPU en moins en M et 15 % en moins en L** sur notre banc, et **22 % de moins dans The Witcher 3 et 25 % de moins dans Black Myth: Wukong** en jeu (les deux en M). L'image reste très proche de celle de NVIDIA, sans différence visible dans nos essais. Ce sont des premiers chiffres, sur un portable et deux jeux : le gain dépendra de votre carte et du jeu. Voir [Super Resolution](docs/SUPER-RESOLUTION.fr.md#fast-super-resolution-rtx-30).
- **Le preset de modèle SR choisi dans RTX Encore s'applique désormais même si le profil du pilote NVIDIA du jeu en impose un autre** (le forçage DLSS de l'application NVIDIA), avec DLSS Super Resolution 310.9.1 et 310.9.16. Avec les autres versions, le pilote garde la main.
- **Neural Rendering accepte le `nvngx_dlssnr.dll` 310.9.16 de NVIDIA** en plus de 310.8.0. Ce fichier a deux réseaux, au choix dans le nouveau **NR model preset**, à côté des presets de modèle SR et RR : **B** (par défaut), plus petit et bien plus rapide, et **A**, le réseau qu'exécute 310.8.0. Dans un test local en 1080p sur une RTX 3070 Ti Laptop, B a pris environ 15 ms de temps NR et A environ 27 ms. Les optimisations de RTX Encore pour les RTX 20 et 30 s’appliquent maintenant aussi aux deux réseaux de ce fichier, avec la même image. Avec 310.8.0, rien ne change. La prise en charge de 310.9.16 est expérimentale et peu testée. Voir [Neural Rendering](docs/NEURAL-RENDERING.fr.md).
- La DLSS-G 310.9.16 du SDK Streamline 2.14.16 est acceptée elle aussi, avec les mêmes fonctions que celle que télécharge l'application NVIDIA : presets A, B et C, Lite, Fast frame generation et UI recomposition.
- La Super Resolution 310.9.16 du SDK Streamline 2.14.16, le même fichier que sur le GitHub DLSS de NVIDIA, est reconnue elle aussi : le preset de modèle SR choisi dans RTX Encore s'applique même si le profil du pilote en impose un autre. Ce fichier exécute le code de la 310.9.1 : il n'a donc pas les presets A à D. Fast super resolution fonctionne avec lui.
- **Un raccourci pour activer et couper le Neural Rendering** : **Ctrl+Alt+N** par défaut, modifiable dans l'éditeur de raccourcis du menu ([#47](https://github.com/SilyNoMeta/rtx-encore/issues/47), demandé par @SiTWulf).
- **Smooth Motion sur RTX 30 accepte aussi le pilote NVIDIA 591.86** ([#52](https://github.com/SilyNoMeta/rtx-encore/issues/52), demandé par @SanyaGames227). Vérifié hors jeu seulement : pas encore confirmé en jeu.

- Le dossier `rtx-encore-logs` contient aussi `rtx-encore-status.json`, l’état de la session en cours, que lit RTX Encore Hub.

### Changements

- `nrPreset` devient `dlssNrPreset`, dans la section `dlssPresets` à côté des autres presets de modèle, et prend `"auto"`, `"a"` ou `"b"` (par défaut `"b"`). Les fichiers de réglages des versions précédentes restent valables : leur `nrPreset` est renommé avec sa valeur, et `0` se lit auto, `1` a, `2` b, `3` auto.
- Le bloc **Component compatibility** du menu est regroupé dans **Session details**, qui liste aussi les versions Streamline Frame Generation, SR et RR et chaque fonction que les fichiers chargés ne permettent pas, avec la raison.

### Corrections

- **Deux copies de RTX Encore dans le même jeu**, sous deux noms de fichier (par exemple `version.dll` et `dxgi.dll`), le modifiaient toutes les deux ; avec Ray Reconstruction et le Neural Rendering actifs, l'image était très dégradée. La première copie tourne désormais seule et le journal nomme l'autre, à retirer.
- **Tailles de DLSS Super Resolution et Ray Reconstruction** : les tailles d'entrée et de sortie sont désormais lues telles que le jeu les soumet, avant que DLSS crée la fonction ; dans Starfield, le journal pouvait les montrer inversées ([#51](https://github.com/SilyNoMeta/rtx-encore/issues/51), journal fourni par @Avictuu). La hausse de consommation signalée dans cette issue n'est pas traitée par ce changement.
- **Smooth Motion sur RTX 30 avec les pilotes 616.64 et 620.12** : une partie du travail dont Smooth Motion a besoin était refusée avec ces pilotes. Cela peut expliquer [#35](https://github.com/SilyNoMeta/rtx-encore/issues/35) (signalé par @CorwinMaximus) ; pas encore confirmé en jeu.
- La documentation précise qu'avec Ray Reconstruction, le Neural Rendering travaille sur l'image débruitée par RR, jamais sur l'image brute du ray tracing ([#48](https://github.com/SilyNoMeta/rtx-encore/issues/48), soulevé par @wisecase2).

## 1.0.0

La première version stable de **RTX Encore**. Elle fait suite à `v1.4.5.3-rtx20-30-40`, publiée sous le nom DLSSG-Transfusion, et la remplace ; la numérotation repart de 1.0.0. Cette entrée rassemble tout ce qui a changé depuis, y compris ce qu’ont apporté les bêtas et les versions candidates.

### Nouveautés

- **Menu et overlay intégrés.** Appuyez sur **Inser** dans le jeu : tous les réglages, l’état de chaque fonction, et un overlay avec la fréquence d’images, le multiplicateur, le rythme d’affichage, le GPU, la VRAM et les versions de DLSS, DLSS-G et Streamline chargées. Fonctionne dans les jeux DirectX 11, DirectX 12 et Vulkan ; ReShade n’est plus nécessaire. **System → Component compatibility** liste les composants chargés, les fonctions qu’ils permettent et la raison d’un choix indisponible. Voir [Menu et overlay](docs/MENU-AND-OVERLAY.fr.md).
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, le compagnon Windows : il installe RTX Encore dans vos jeux avec les derniers fichiers DLSS et Streamline qualifiés et le fichier Neural Rendering de NVIDIA, met à jour tous les jeux en un clic, et peut toujours remettre un jeu comme avant. L’installation à la main fonctionne comme avant ; voir [Installation](docs/INSTALLATION.fr.md#le-plus-simple--rtx-encore-hub).
- **Neural Rendering** sur RTX 20, RTX 30 et RTX 40, appliqué à l’image produite par DLSS : une à quatre passes avec un style pour chacune, prise en charge du HDR, et le choix entre le moteur NVIDIA et le moteur Open expérimental. Désactivé par défaut ; nécessite le `nvngx_dlssnr.dll` 310.8.0 de NVIDIA. Open ne prend pas en charge les RTX 20.
- **Notre travail de performance NR pour Ampere (RTX 30) :** traitement plus rapide à résultat identique dans les tests de référence locaux du moteur NVIDIA ; précision Rapide NVIDIA avec environ 17 % de temps NR en moins mesuré sur RTX 30 ; résolution NR indépendante, Ultra Performance comprise ; précision Rapide d’Open et réglages facultatifs vitesse/VRAM. Notre **mode Ultra-rapide** a fait passer le temps NR de **18,7 à 9,8 ms (−48 %)** et les FPS affichés de **73,8 à 94,9 (+29 %)** dans le benchmark de Wukong sur une RTX 3070 Ti Laptop en FG X3, une mesure par configuration ; une passe NR, artefacts de mouvement possibles, résultats non garantis. Voir [Neural Rendering](docs/NEURAL-RENDERING.fr.md).
- **Plus de versions de Streamline.** Dynamic MFG natif et recomposition de l’interface avec les Streamline de production qualifiés 2.11.1, 2.12.0, 2.14.1 et 2.14.16 ; X2–X4 adaptatif d’Encore pour les anciens Streamline DirectX 12 qualifiés 2.7.2–2.10.3 ; prise en charge des fichiers que télécharge la bêta de l’application NVIDIA du 13 octobre 2026 (Streamline 2.14.16, DLSS-G 310.9.16). Vulkan garde son mode adaptatif ; DirectX 11 garde Smooth Motion X2. Voir [Compatibilité](docs/COMPATIBILITY.fr.md#versions-de-streamline).
- **Presets DLSS :** choix indépendants des presets Super Resolution, Ray Reconstruction et génération d’images, appliqués après un redémarrage, dont le preset de génération d’images **C (Lite)**, le mode performance plus rapide de NVIDIA, avec le DLSS-G 310.9.16.0 qualifié. A/B gardent Normal, et **Fast frame generation** reste indépendante du preset. Par défaut, les choix du jeu sont conservés.
- **Contrôle de la fréquence d’images :** une limite par NVIDIA Reflex avec saisie directe des FPS, **Use game settings** et **For this display** (une marge qui croît avec la fréquence de l’écran : 116 FPS à 120 Hz, 138 à 144 Hz, 224 à 240 Hz), une demande de présentation sans V-Sync, et des cheveux en ray tracing compatibles dans The Witcher 3.
- **Smooth Motion sur RTX 30**, réglé depuis le menu, accepte les pilotes NVIDIA 620.12, 617.42, 617.14, 616.92 et 616.64 (617.14 vérifié en jeu). **Force Smooth Motion** (désactivé par défaut) couvre les jeux Vulkan dont le profil NVIDIA le laisse éteint, et l’overlay et le menu signalent quand Smooth Motion ne tourne pas réellement. Voir [Smooth Motion](docs/SMOOTH-MOTION.fr.md).
- **Un seul fichier universel :** `rtx-encore.dll` accepte dix-neuf noms de fichier, pour cohabiter avec les mods qui utilisent déjà les noms habituels.
- **Panneau facultatif pour Cyber Engine Tweaks** dans Cyberpunk 2077 (expérimental).
- **`disableInGameMenu`** laisse le menu de côté, pour les jeux qui le refusent (Star Wars Outlaws) ; la génération d’images continue de fonctionner.
- **Diagnostics :** journaux de session dans un dossier `rtx-encore-logs` ; un relevé automatique du système, des versions des composants chargés, de la qualité et des résolutions DLSS, et des messages normaux de Streamline/NGX avec les motifs de refus ; des logs d’exception séparés pour chaque processus, avec le thread qui a terminé le processus. En option : `logPerformance` (temps d’image, température, puissance, fréquences et limites du GPU), `logGraphics`, `logStreamline`, `logNr`, `profileNrGpu`, `profileFrameGpu` (temps GPU de chaque passe DLSS sur DirectX 12), et pour les rapports de plantage `logDxgiWrappers` et `crashDetails` (détails mémoire et instantané `.dmp`). Voir [Paramètres](docs/SETTINGS.fr.md).
- Un avertissement quand l’ancien module dlssg_for_sm86 est encore installé à côté de RTX Encore : avec les deux actifs, la génération d’images peut ne présenter aucune image générée.

### Changements

- Les fichiers s’appellent `rtx-encore.*`. Réglages, raccourcis et un ancien plugin sont repris automatiquement : voir [Mise à niveau depuis un ancien nom](docs/INSTALLATION.fr.md#mise-à-niveau-depuis-un-ancien-nom).
- Le fichier de réglages est `rtx-encore.jsonc`, organisé en sections, avec un commentaire sur chaque clé ; plusieurs réglages et entrées du menu ont des noms plus clairs, et un ancien fichier de réglages est repris avec ses valeurs.
- La génération d’images coûte 2 à 3 % de temps GPU en moins sur les RTX 40.
- Le texte de l’overlay est plus compact, avec un réglage de taille ; quand les plugins Streamline du jeu sont plus récents que son cœur Streamline, l’overlay affiche les deux versions.
- L’ancien add-on ReShade n’est plus fourni.

### Corrections

- **Star Wars Outlaws** ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)) : le jeu complet (Ubisoft Connect) se ferme au lancement quand le menu y est attaché ; mettez `"disableInGameMenu": true`, ou appliquez le profil communautaire du jeu dans le Hub, et le jeu démarre avec la génération d’images jusqu’à X4, comme l’ont confirmé deux utilisateurs avec des builds de test. Corrigés aussi : un plantage quand les réglages vidéo affichent les options de génération d’images, et un plantage après un alt-tab depuis le menu principal.
- **Starfield :** un plantage à l’activation de la génération d’images avec le menu présent ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), un problème de fermeture avec Streamline 2.12 ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)), et les changements de recomposition de l’interface ignorés pendant la génération d’images ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- **Forza Horizon 6** qui se fermait sur ses logos de démarrage avec le menu ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- **Final Fantasy VII Rebirth** qui masquait la génération d’images sur les RTX 20 et 30 ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- **Where Winds Meet** qui ne démarrait pas (0xc0000142) avec `version.dll` ou `winmm.dll` ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)) ; utilisez `version.dll` dans ce jeu. Corrigé aussi : un plantage du nom `dinput8.dll` avec l’overlay Epic.
- **Euro Truck Simulator 2 :** le menu n’apparaissait pas ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)) ; installez RTX Encore sous le nom `winmm.dll` dans ce jeu.
- Les problèmes de démarrage de **Cyberpunk 2077** liés au plugin Reflex lors des redémarrages de Streamline ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), et **The Witcher 3** avec la génération d’images dynamique ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)) : confirmation en jeu en attente.
- Le plantage au démarrage de **The Last of Us** avec OptiScaler et l’ancien Streamline du jeu ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)) : confirmation en jeu en attente.
- Le menu qui refusait de s’attacher en DirectX 12 avec **Special K** ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- L’activité de DLSS **Ray Reconstruction** non détectée ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)) ; Neural Rendering signale maintenant quand son entrée de profondeur est indisponible.
- Les changements de limite Reflex appliqués en retard ou non restaurés selon les threads du jeu.
- Le premier lancement après une mise à jour depuis un ancien fichier de réglages, où certains réglages de démarrage ne prenaient effet qu’au deuxième lancement ; et l’outil de rapport de plantage d’un jeu qui écrivait ses propres journaux de session à la place des vrais.

### Limites connues

- La prise en charge des RTX 20 est expérimentale et n’a pas été validée sur une vraie carte RTX 20 ; un échec de génération d’images sur RTX 20 ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)) est encore à l’étude.
- Dans Star Wars Outlaws (jeu complet), le menu en jeu n’est pas disponible.
- Neural Rendering à plusieurs passes, le moteur Open, le panneau Cyber Engine Tweaks et les cheveux en ray tracing sur RTX 20 et RTX 30 ont été peu ou pas testés en jeu. X5 et X6 sont expérimentaux et demandent beaucoup de VRAM.
- Plusieurs corrections ci-dessus ont été confirmées avec des builds de test ou des essais locaux, pas avec ce build final ; quand l’auteur d’un signalement n’a pas refait l’essai, c’est indiqué. Les vérifications automatiques ne remplacent pas les tests de chaque jeu, carte et pilote.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. Le build final 1.0.0 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué au-delà des mesures citées.

### Remerciements

- [@ivanmestre](https://github.com/ivanmestre) — le signalement Star Wars Outlaws, les logs, les tests d’isolation et de trace et les instantanés de plantage ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).
- [@yannduwooz83](https://github.com/yannduwooz83) — les tests de Star Wars Outlaws et la comparaison avec le projet précédent ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)), le signalement et les logs de Final Fantasy VII Rebirth ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@Avictuu](https://github.com/Avictuu) — les tests répétés de Starfield, les comparaisons de composants, les logs Reflex et de fermeture ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21), [#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- [@mennogreg](https://github.com/mennogreg) — les signalements, logs et tests répétés de Starfield ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), le signalement OptiScaler ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)), la confirmation sur Ray Reconstruction ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@hunan-NRT](https://github.com/hunan-NRT) — le signalement Ray Reconstruction et les tests de Cyberpunk ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) — les signalements et logs de Cyberpunk ([#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), le signalement Euro Truck Simulator 2 ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)).
- [@wisecase2](https://github.com/wisecase2) — les logs de session de The Witcher 3 et les éléments sur Cyberpunk ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@avikpal693](https://github.com/avikpal693) et [@wer2357666](https://github.com/wer2357666) — les signalements et logs de Forza Horizon 6 ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- [@lovebabyx](https://github.com/lovebabyx) — les logs RTX 2070 Super qui ont montré ce qui manquait au journal de session ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)).
- [@wd3dmlys](https://github.com/wd3dmlys) — la piste sur l’architecture GPU ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@ranmo-1](https://github.com/ranmo-1) — le signalement Where Winds Meet ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)).
- [@PhantomGamers](https://github.com/PhantomGamers) — la formule de marge selon la fréquence de l’écran ([#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)) et le signalement Special K ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- [@PrincessofCelery](https://github.com/PrincessofCelery) — les signalements qui ont amélioré le diagnostic des gels ([#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)).
- [@CorwinMaximus](https://github.com/CorwinMaximus) — la demande du pilote 620.12 ([#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)).
- [@hasnainb237](https://github.com/hasnainb237) — les logs Star Wars Zero Company qui ont montré l’ancien module resté en place ([#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)).

Merci à toutes celles et tous ceux qui ont partagé leurs logs et testé les bêtas et les versions candidates.

## Versions antérieures

Les versions jusqu'à `v1.4.5.3-rtx20-30-40` ont été publiées sous le nom DLSSG-Transfusion. Elles ne sont plus distribuées.
