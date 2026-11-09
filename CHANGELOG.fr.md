# Journal des versions

*[English](CHANGELOG.md) · [中文](CHANGELOG.zh-CN.md)*

## 2.2.0

**RTX Encore 2.2.0 ajoute la mise à l'échelle spatiale et un overlay personnalisable.** Les jeux sans DLSS peuvent utiliser NVIDIA Image Scaling (NIS), avec FXAA ou SMAA en option avant ou après la mise à l'échelle. L'overlay propose plusieurs styles et des métriques au choix. XeFG sur les Tensor Cores s'étend aux RTX 20, et XeSS sur les Tensor Cores reçoit des optimisations pour les RTX 30/40.

### Mise à l'échelle et anticrénelage

- **NVIDIA Image Scaling (NIS)** est un nouveau choix pour les jeux sans DLSS, y compris en DirectX 11. **FXAA** et **SMAA** peuvent s'appliquer avant ou après NIS, ou rester désactivés. Choisissez le rendu qui convient au jeu : ces filtres spatiaux ne recréent pas les détails absents de l'image source. Les réglages correspondants sont dans le Hub 2.2.0.
- La mise à l'échelle spatiale se combine mieux avec le moteur de reconstruction choisi et conserve plus fidèlement la géométrie de l'image du jeu. La correction pour Les Sims 4 a été confirmée en jeu. La taille de la fenêtre NIS tient également compte de la mise à l'échelle Windows quand le moteur est chargé dynamiquement.
- **XeSS Super Resolution sur les Tensor Cores** reçoit des chemins optimisés pour les RTX 30 et RTX 40. La conformité des images a été vérifiée sur RTX 3070 Ti et RTX 4090 ; les mesures sur RTX 3070 Ti varient selon la résolution, sans promesse de gain universel.

### Génération d'images et overlay

- **DLSS-G et mode Auto du jeu :** le nouveau réglage `overrideAutoFrameGeneration`, activé par défaut, demande à NVIDIA de garder la génération d'images active quand le jeu demande Auto et que NVIDIA la couperait dans une scène lourde. Désactivez-le pour transmettre Auto tel quel. Il s'applique en direct et figure dans le Hub 2.2.0. Merci à [@avikpal693](https://github.com/avikpal693) pour le signalement ([#116](https://github.com/SilyNoMeta/rtx-encore/issues/116)) ; le build final n'a pas été confirmé par son auteur.
- **XeFG sur les Tensor Cores NVIDIA** admet maintenant les RTX 20 (Turing), à titre expérimental. La compatibilité dépend toujours du pilote, du runtime, du jeu et de l'API graphique ; cette version ne prétend pas valider XeFG sur RTX 20 dans tous les jeux. Merci à [@MxHassan195](https://github.com/MxHassan195) pour la demande ([#117](https://github.com/SilyNoMeta/rtx-encore/issues/117)).
- **Overlay Studio :** choisissez les métriques affichées et un style visuel. Le nouveau style classique rétablit les bandeaux de texte blancs ; le graphique optionnel et les métriques choisies restent disponibles. Le Hub 2.2.0 permet de prévisualiser et régler les styles et chaque métrique.
- **Baldur's Gate 3 :** le menu ImGui/MCM du Script Extender s'ouvre de nouveau avec DLSS Frame Generation ajouté en DirectX 11. Confirmé en jeu par le mainteneur avec un build de test.
- **Kingdom Come: Deliverance :** DLSS Frame Generation ajouté ne s'attache plus à la petite fenêtre de test du jeu ; la fenêtre du jeu fonctionne en X2. L'override Streamline du pilote NVIDIA est aussi accepté pour DLSS-G natif. Les deux corrections ont été confirmées en jeu par le mainteneur avec des builds de test.

L'archive finale est construite et vérifiée séparément de ces essais en jeu. Les chemins expérimentaux restent dépendants du jeu ; en cas d'échec, indiquez le jeu, l'API graphique, le GPU et le diagnostic du Hub.


## 2.1.0

**RTX Encore 2.1.0 rend Intel XeFG fluide à partir de X3.** Un nouveau réglage, **Cadence du jeu régulière pour XeFG**, activé par défaut, supprime les saccades de caméra que pouvaient montrer les multiplicateurs élevés de XeFG. Les jeux qui ont le DLSS mais pas de génération d'images donnent maintenant leur propre profondeur et leur propre mouvement à la génération d'images qu'ajoute RTX Encore, XeFG tourne aussi sur les Tensor Cores en DirectX 11 et Vulkan, et une longue liste de corrections suit : Baldur's Gate 3, Final Fantasy VII Rebirth, Forza Horizon 6, Kingdom Come: Deliverance, Islands of Insight et Assassin's Creed Black Flag Resynced.

### Cadence du jeu régulière pour XeFG (nouveau, activé par défaut)

À partir de X3, XeFG pouvait faire saccader la caméra alors même que le compteur d'images semblait correct : les images du jeu alternaient entre courtes et longues, si bien que chaque groupe d'images générées couvrait une durée différente. **Cadence du jeu régulière pour XeFG** lance les images du jeu à intervalle régulier, juste sous la cadence que le jeu peut tenir, et l'ajuste en continu au jeu. Le mouvement reste régulier sur chaque image générée.

- Activé par défaut, dans l'onglet **Fluidité** du menu avec les autres réglages XeFG, et dans [RTX Encore Hub 2.1.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.1.0). Il s'applique à chaud : désactivez-le puis réactivez-le pour voir la différence.
- Il coûte quelques FPS à la cadence propre du jeu. Il ne presse jamais une image en retard et ne rattrape jamais en rafale.
- Il fonctionne avec la cadence variable de XeFG (activée par défaut) à partir de X3 ; X2 ne change pas.
- La sortie de XeFG elle-même est plus régulière : les images générées gardent leur espacement après un à-coup et ne dérivent plus avec le temps.

Validé par le mainteneur dans Assassin's Creed Black Flag Resynced en X4 sans limite d'images : l'alternance a disparu et le mouvement est très fluide. Merci à **Konan** sur le [Discord](https://discord.gg/Zwmq4Xc2Br) pour les signalements et les logs (Black Flag Resynced, Forza Horizon 6, Avatar: Frontiers of Pandora) et pour avoir repéré que la saccade suivait la cadence variable de XeFG. Réglage `xefgSourcePacing`, voir [Réglages](docs/SETTINGS.fr.md) et [Moteurs Intel](docs/INTEL-ENGINES.fr.md).

### Génération d'images avec la profondeur et le mouvement du jeu

- **Jeux avec DLSS mais sans génération d'images** (Baldur's Gate 3, par exemple) : le DLSS Frame Generation ou le XeFG qu'ajoute RTX Encore utilise maintenant la profondeur et les vecteurs de mouvement que le jeu donne déjà à son DLSS, au lieu de les estimer à partir de l'image, en DirectX 11 et Vulkan. Quand les données du jeu ne sont pas utilisables, la génération d'images revient à l'estimation du mouvement de RTX Encore, et le menu affiche alors **Estimation du mouvement**. Le mainteneur a vu X4 généré dans Baldur's Gate 3 dans les quatre combinaisons (DirectX 11 et Vulkan, DLSS-FG et XeFG) sur une RTX 3070 Ti Laptop.
- **Baldur's Gate 3 (DirectX 11) : DLSS Frame Generation restait « en attente »** même avec les fichiers Streamline en place. Corrigé ; les écrans 10 bits (RGB10) sont aussi acceptés. Merci à [@deusimperatork](https://github.com/deusimperatork) pour le signalement ([#109](https://github.com/SilyNoMeta/rtx-encore/issues/109)). L'auteur du signalement n'a pas encore retesté.
- **Baldur's Gate 3 (Vulkan) : Neural Rendering et DLSS Frame Generation ensemble**, sans aucun outil tiers. Vérifié par le mainteneur : NR après la super résolution et DLSS-FG X2 puis X4 en 1440p.

### Tensor Cores en DirectX 11 et Vulkan (expérimental)

- **XeFG sur les Tensor Cores NVIDIA** tourne maintenant aussi dans les jeux DirectX 11 et Vulkan sans génération d'images. Dans Baldur's Gate 3, en DirectX 11 et Vulkan, le mainteneur a vu le badge vert **Tensor Cores** actif et XeFG X4. La parité d'image et le gain de performance n'ont pas encore été mesurés.
- **XeSS Super Resolution sur les Tensor Cores** est maintenant proposé dans les jeux DirectX 11 et Vulkan sans DLSS, et quand XeSS remplace le DLSS d'un jeu DirectX 11. Pas encore validé dans un jeu. Le remplacement du DLSS dans un jeu Vulkan garde le XeSS standard.

### Corrections

- **Final Fantasy VII Rebirth : la génération d'images restait indisponible** avec la surcharge DLSS de l'app NVIDIA (ou de NVIDIA Profile Inspector) activée : le jeu utilise alors les fichiers Streamline téléchargés par le pilote, que RTX Encore ne reconnaissait pas, et une carte RTX 30 était traitée comme incapable de générer des images. Ils sont reconnus désormais. Merci à [@yannduwooz83](https://github.com/yannduwooz83) pour le signalement et pour avoir trouvé le réglage en cause ([#106](https://github.com/SilyNoMeta/rtx-encore/issues/106)).
- **XeFG avec RTSS (RivaTuner Statistics Server, MSI Afterburner) :** RTSS reprenait la création de la swap chain du jeu après RTX Encore et faisait sauter XeFG dès la deuxième création : XeFG pouvait rester inactif ou échouer quand le jeu recréait sa fenêtre. RTX Encore se replace maintenant devant RTSS et lui transmet chaque création, les deux continuent de fonctionner. Vérifié avec RTSS actif dans les tests automatisés XeFG de RTX Encore ; pas encore en jeu. Merci à **Konan** sur Discord, dont les signalements RTSS ont mis sur la piste.
- **Kingdom Come: Deliverance (DirectX 11) :** le menu ne s'ouvrait pas quand le jeu était lancé par Ultimate ASI Loader, et le jeu plantait en perdant le focus (Alt+Tab, réduction) avec DLSS Frame Generation. Les deux sont corrigés et confirmés en jeu par le mainteneur. Merci à **SMN888** sur Discord pour le signalement et le diagnostic.
- **Assassin's Creed Black Flag Resynced (XeFG) :** les blocages d'une seconde et les écrans noirs sont corrigés, le menu continue de fonctionner avec XeFG, et l'overlay affiche la cadence propre du jeu séparément de la sortie générée. Jeu confirmé par le mainteneur ; les changements du menu et du compteur ne sont pas encore confirmés en jeu. Merci à **Konan** sur Discord pour le signalement.
- **Forza Horizon 6 :** XeSS Super Resolution en DirectX 12 s'active maintenant au lieu de se replier, XeSS sur les Tensor Cores continue de tourner après une réinitialisation du périphérique graphique par le jeu, et un plantage avec l'overlay Steam est évité. Confirmé en jeu par le mainteneur. Merci à **Konan** sur Discord pour le signalement.
- **Islands of Insight et autres jeux sans DLSS agrandis à la résolution de l'écran :** l'image, sa taille et le curseur de la souris restent alignés lors des redimensionnements et dans la fenêtre sans bordure, la génération d'images démarre de façon fiable en DirectX 11, et **Aucun** désactive vraiment l'upscaler ajouté. Confirmé en jeu par le mainteneur. Merci à **SMN888** sur Discord, dont les essais dans des jeux sans DLSS (avec la mise à l'échelle de Windows) ont mené à ces corrections. Un nouveau guide explique comment la mise à l'échelle de l'affichage de Windows peut limiter la résolution du jeu et rendre le texte flou dans les jeux Unreal Engine : [Résolution et mise à l'échelle Windows](docs/GAMES-WITHOUT-DLSS.fr.md#résolution-et-mise-à-léchelle-windows).
- **Menu :** le DLSS propre au jeu apparaît comme **Jeu (DLSS)** dans le choix du moteur, et une indication dit quels composants Streamline installer avec le Hub quand DLSS-FG ne peut pas démarrer ; avec **Aucun** comme moteur d'upscaling, l'interrupteur Tensor Cores de XeSS est masqué et **Estimation du mouvement** passe dans Génération d'images quand seule la génération d'images l'utilise.
- **Installation manuelle :** la [liste des composants](README.fr.md#composants-pour-une-installation-manuelle) vaut maintenant pour tous les jeux, avec le Ray Reconstruction (`nvngx_dlssd.dll`) du SDK DLSS 310.9.16 de NVIDIA.

### Ce qui a été vérifié

Les jeux cités ont été joués par le mainteneur avec des builds de test de ces changements sur une RTX 4090 ou une RTX 3070 Ti Laptop. Ils n'ont pas été retestés avec les binaires finaux de la 2.1.0. Les vérifications de la release couvrent la compilation, les suites de non-régression de ces fonctions et l'intégrité de l'archive.

**Limite connue :** RTSS (RivaTuner Statistics Server) est désormais maintenu derrière XeFG (voir Corrections), mais son overlay n'a pas encore été essayé avec XeFG en jeu. Si un jeu plante encore ou si XeFG reste inactif avec lui, désactivez l'overlay RTSS et signalez-le.

## 2.0.1

- **Installation manuelle avec les fichiers de NVIDIA :** la génération d'images pour les jeux qui n'en ont pas accepte aussi le DLSS Frame Generation 310.9.16 officiel de NVIDIA (le `nvngx_dlssg.dll` du SDK Streamline 2.14.16 et du SDK DLSS) : tous les fichiers peuvent venir des SDK de NVIDIA. Il était refusé par une erreur de détection. Merci à [@Kbx-7](https://github.com/Kbx-7) pour le signalement ([#108](https://github.com/SilyNoMeta/rtx-encore/issues/108)).
- **Fichiers Intel XeSS à côté de RTX Encore :** XeSS-SR, XeFG et XeLL sont aussi trouvés à côté de la DLL de RTX Encore, pas seulement dans `rtx-encore-runtimes\intel`.
- Fichiers et liens de téléchargement pour une installation manuelle : [README](README.fr.md#composants-pour-une-installation-manuelle). [RTX Encore Hub 2.0.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.1) installe les fichiers 310.9.16 officiels de NVIDIA.

## 2.0.0

Les moteurs proposent Aucun pour la reconstruction et la génération ajoutées ; les runtimes absents sont masqués et les anciennes versions compatibles signalées. Ultra (75 %) rejoint les échelles DLSS SR et XeSS. Le DLSS-FG ajouté prend en charge DirectX 11, DirectX 12 et Vulkan.

**RTX Encore 2.0.0 est une version majeure.** Les jeux qui n'ont aucun DLSS reçoivent désormais DLSS, la génération d'images et le Neural Rendering, portés par un moteur d'estimation du mouvement que RTX Encore a écrit pour eux. RTX Encore change aussi d'identité visuelle, avec un nouveau logo, et ouvre son Discord.

**Le Discord de RTX Encore est ouvert.** Partagez vos essais de la 2.0.0, comparez vos réglages ou demandez de l’aide pour le mod et le Hub. [Rejoindre le Discord](https://discord.gg/Zwmq4Xc2Br).

RTX Encore 2.0.0 apporte **NVIDIA DLSS Super Resolution et DLAA, Intel XeSS, le Neural Rendering et la génération d'images aux jeux qui n'ont pas de DLSS**, en DirectX 11, DirectX 12 et Vulkan, à partir de l'image qu'ils affichent, sans ReShade ni aucun autre outil. Le jeu n'a besoin de rien à lui : ni DLSS, ni vecteurs de mouvement, ni génération d'images. Tout est expérimental et désactivé par défaut ; [Jeux sans DLSS](docs/GAMES-WITHOUT-DLSS.fr.md) est le guide complet.

- **Reconstruction pour les jeux sans DLSS :** un DLAA à la résolution du jeu, ou une résolution de jeu plus basse agrandie jusqu'à celle de l'écran (le jeu en mode fenêtré, RTX Encore tenant une fenêtre sans bordure sur l'écran), avec **DLSS** ou **XeSS** ; le moteur change à chaud. XeSS tourne sur les Tensor Cores NVIDIA dans les jeux DirectX 12, avec la même qualification qu'ailleurs, et la puce **Reconstruction** porte un badge **Tensor Cores** tant qu'ils tournent.
- **Neural Rendering sur l'image affichée :** dans un jeu sans DLSS, l'interrupteur **Neural Rendering** lance le NR directement sur l'image affichée, après la reconstruction ou seul, avec tous les réglages NR.
- **Génération d'images pour les jeux sans génération d'images :** **DLSS Frame Generation** X2 à X6 en DirectX 11, DirectX 12 et Vulkan ; **Intel XeFG** X2 à X6 en DirectX 12 (Tensor Cores NVIDIA disponibles), en Vulkan (SDR et HDR10) et en DirectX 11. Choix du jeu donne X2, Fixe donne votre multiplicateur, changé à chaud. En DirectX 11, l'image suivante du jeu attend le travail de XeFG, le X3 est régulier, et là où l'écran autorise le tearing la sortie n'est plus bornée à sa fréquence de rafraîchissement. Le HUD est interpolé avec l'image.
- **Estimation du mouvement, quatre choix :** **RTX Encore**, l'estimation propre au mod sur la carte graphique, par défaut (un vecteur par 2×2 pixels, qui suit les bords des objets et s'appuie sur le depth buffer ; environ 0,8 ms par image 4K sur une RTX 4090) ; **Shader (vort)**, la méthode de vort_Motion de vortigern11 (environ 0,6 ms) ; **Optical Flow (Qualité)** et **Optical Flow (Rapide)**, NVIDIA Optical Flow. Changez à chaud et comparez.
- **Depth buffer trouvé automatiquement** quand le jeu dessine avec, dans les trois API ; sans lui, tout fonctionne quand même, avec un mouvement un peu moins stable.
- **Le Hub installe tout :** [RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) place à côté du mod les fichiers DLSS, Neural Rendering, génération d'images et Intel qu'un jeu sans DLSS peut utiliser quand vous y installez ou mettez à jour le mod, et les retire avec **Restaurer le jeu**. Rien n'est activé pour autant.
- **Installation manuelle :** ce qui est ajouté aux jeux sans DLSS ou sans génération d'images fonctionne avec les composants exacts validés (DLSS Frame Generation 310.9.16 avec Streamline 2.14.16, DLSS 310.9.16, Neural Rendering 310.9.16, XeSS-SR 2.0.2.68, XeFG 1.3.1.78 + XeLL 1.3.2.10). Le Hub les installe ; à la main, les fichiers et leurs liens de téléchargement sont dans le [README](README.fr.md#composants-pour-une-installation-manuelle).

Validé par le mainteneur sur une RTX 4090 (NVIDIA 621.01) : PEAK en DirectX 12 (DLAA, upscaling 1440p vers 4K, NR, DLSS Frame Generation X2 et X4, XeFG X4 avec Tensor Cores), DirectX 11 (XeFG X2) et Vulkan (DLAA, upscaling, NR, DLSS Frame Generation X4, XeFG X4) ; The Sims 4 en DirectX 11 (XeFG X2 et X3, au-delà de 144 FPS sur un écran 144 Hz) ; Skyrim Special Edition en DirectX 11 (XeFG X3 à X6) ; Football Life 2027 en DirectX 11 et HDR (DLAA et NR, 1440p vers 4K et NR). Limites à connaître : le HUD est traité et interpolé avec l'image ; l'image agrandie est un peu plus douce que le DLSS d'un jeu, qui dispose du jitter de la caméra (le DLAA n'a pas cette limite) ; dans les jeux Unity, choisissez Fenêtré, pas Plein écran fenêtré, sous la résolution de l'écran ; un jeu qui affiche en scRGB (16 bits flottants) garde ses propres images, sans génération d'images ; les Tensor Cores ne s'appliquent qu'en DirectX 12 ; désactivez Smooth Motion avec la génération d'images. Aucune RTX 20, RTX 30 ou RTX 50 et aucun autre pilote n'ont encore été testés.

### Aussi dans la 2.0.0

- **Menu en onze langues :** l'allemand, l'espagnol, le néerlandais, le polonais, le portugais brésilien, le vietnamien, le russe et le japonais rejoignent l'anglais, le français et le chinois simplifié. L'arabe, langue du Hub, s'affiche en anglais dans le menu. Le menu repose désormais sur Dear ImGui 1.93. Merci à [@Matarto](https://github.com/Matarto) d'avoir demandé le polonais ([issue 12 du Hub](https://github.com/SilyNoMeta/rtx-encore-hub/issues/12)).
- **Modèles DLSS les plus récents de NVIDIA App :** l'ancienne option « Force NVIDIA OTA models » (onglet Système) fait désormais ce que son nom dit, pour la génération d'images, Super Resolution, Ray Reconstruction et Neural Rendering : quand NVIDIA App a téléchargé un modèle plus récent que celui du jeu, et que RTX Encore le qualifie, c'est ce modèle qui sert, sans rien écrire dans les fichiers du jeu ni de NVIDIA App. Cela donne le preset C de génération d'images dans un jeu qui livre un DLSS-G plus ancien, et un indice sous le preset FG le dit. Génération d'images validée dans Cyberpunk 2077 sur une RTX 4090 et une RTX 3070 Ti Laptop ; Super Resolution, Ray Reconstruction et Neural Rendering pas encore vérifiés en jeu. Merci à [@Kbx-7](https://github.com/Kbx-7) pour l'idée ([#101](https://github.com/SilyNoMeta/rtx-encore/issues/101)).
- **XeFG dans Ghost of Tsushima :** XeFG était accepté en X2 mais ne générait rien : le jeu transmet sa profondeur et son mouvement d'une façon que RTX Encore ne prenait pas. C'est fait ; le mainteneur a vu X2 et X4 générés en jeu. Merci à [@MoDymm-debug](https://github.com/MoDymm-debug) pour le signalement et les journaux ([#100](https://github.com/SilyNoMeta/rtx-encore/issues/100)).
- **Plantage en activant ou désactivant la génération d'images avec XeFG (Crimson Desert) :** changer la génération d'images du jeu quand XeFG était le moteur pouvait fermer le jeu. Reproduit sur une RTX 3070 Ti Laptop et corrigé ; plusieurs bascules à la suite ont ensuite tourné sans plantage. Merci à [@pradeepkumar35](https://github.com/pradeepkumar35) pour le signalement ([#103](https://github.com/SilyNoMeta/rtx-encore/issues/103)). Le rapporteur n'a pas encore retesté.
- **Tensor Cores dans les jeux qui livrent un Agility SDK DirectX 12 (Crimson Desert et d'autres) :** depuis la 1.5.3, XeFG et XeSS Tensor Cores étaient refusés dans tous ces jeux et tournaient sur le chemin Intel standard. Les deux tournent de nouveau, vérifié dans Crimson Desert sur une RTX 4090 (trouvé en reproduisant la #103). Le chemin XeSS Tensor Cores accepte aussi le hotfix NVIDIA 621.12, identifié d'après le paquet du pilote, pas encore validé en jeu.
- **No Man's Sky ne démarre pas (Vulkan) :** depuis la 1.2.2, un runtime Intel XeSS trouvé à côté du jeu était préparé au lancement même avec DLSS sélectionné, et sur la configuration du rapporteur le jeu plantait là. XeSS n'est plus touché que s'il est le moteur sélectionné au lancement du jeu ; la bascule à chaud vers XeSS en Vulkan n'est plus proposée. Non reproduit sur la machine du mainteneur ; merci à [@sweddd](https://github.com/sweddd) pour le signalement et les journaux ([#104](https://github.com/SilyNoMeta/rtx-encore/issues/104)). Le rapporteur n'a pas encore confirmé.
- **XeFG à la place du DLSS Frame Generation du jeu dans No Man's Sky (Vulkan) :** la sortie HDR10 est acceptée (pas le scRGB : le DLSS-G du jeu reste) ; un plantage à la première image avec ReShade 6.8 installé est corrigé ; les images générées s'affichent sans attendre le rafraîchissement de l'écran, comme le fait le DLSS-G du jeu, de sorte que la fréquence du jeu n'est plus tenue au taux de rafraîchissement divisé par le multiplicateur ; le multiplicateur change à chaud. Validé par le mainteneur en X4 en HDR. **Limite connue :** avec le jeu en DLAA, XeFG ne génère rien (le mouvement pleine résolution du jeu ne correspond pas au contrat de XeFG) : choisissez un mode de qualité DLSS.
- **XeSS Super Resolution dans No Man's Sky (Vulkan) :** le depth buffer du jeu était refusé et le DLSS continuait de tourner. XeSS le prend désormais ; quand XeSS est choisi mais que le DLSS a tourné à sa place, le menu le dit sous **Moteur**, avec la raison.
- **Fluidifier X3 / X4 devient Automatique :** le réglage est désormais **Automatique** (par défaut), **Activé** ou **Désactivé**. En Automatique, RTX Encore observe comment les images générées arrivent à l'écran à partir de X3 et apprend, pour ce jeu, si Streamline doit les cadencer lui-même ; une notice dit ce qu'il a vu et ce qu'il a changé, appliqué au prochain lancement. Le Hub montre le choix appris et peut le remettre à zéro. Vérifié dans PEAK en DirectX 12.
- **Football Life 2027 (et d'autres exécutables protégés) se fermait environ 1,5 s après le lancement** avec RTX Encore installé. Corrigé ; le menu s'attache comme d'habitude.
- **Texte des menus dans les jeux sans DLSS (Skyrim Special Edition) :** le texte immobile était déformé avec l'estimation du mouvement RTX Encore ; il garde désormais un mouvement exactement nul. Le texte qui défile, apparaît en fondu ou grossit dans un menu laisse moins de traînées derrière DLSS ou XeSS. Pas encore confirmé dans le jeu. XeFG continue aussi de fonctionner quand un jeu DirectX 11 déplace son affichage sur un autre thread, comme Skyrim SE.
- **Menu :** les deux premiers onglets s'appellent désormais **Fluidité** et **Qualité d'image**, avec des noms distincts dans chaque langue ; les moteurs ajoutés se choisissent directement dans **Moteur**, avec **Aucun** pour les désactiver ; un badge vert **Tensor Cores** sur les puces Reconstruction et Génération d'images tant que XeSS ou XeFG tournent sur les Tensor Cores ; les interrupteurs Tensor Cores sont masqués dans les jeux DirectX 11 et Vulkan, où ils ne peuvent rien faire ; **Fast super resolution** n'est proposée que sur RTX 30, où elle s'applique.

### Ce qui a été vérifié

Les jeux nommés ci-dessus ont été joués par le mainteneur avec des builds de test de ces changements sur une RTX 4090 (et une RTX 3070 Ti Laptop pour les #101 et #103). Les jeux n'ont pas été retestés avec les binaires finaux 2.0.0. Les contrôles de release couvrent la compilation, les suites de régression de ces fonctions et l'intégrité de l'archive ; la suite de tests complète n'a pas été relancée.

## 1.6.0

La 1.6.0 pousse l'accélération de la génération d'images encore plus loin : chaque image générée coûte encore moins sur RTX 40 et RTX 30, et reste identique à celle de NVIDIA, pixel pour pixel.

RTX 4090, DLSS-G 310.9.16, temps GPU de chaque image générée :

| Cas | Optimisations désactivées | 1.5.3 | 1.6.0 | sur 1.5.3 | sur désactivé |
| :--- | ---: | ---: | ---: | ---: | ---: |
| 1080p X2 | 0,781 ms | 0,538 ms | 0,512 ms | −4,8 % | −34,4 % |
| 1080p X6 | 0,330 ms | 0,292 ms | 0,276 ms | −5,5 % | −16,4 % |
| 4K X2 | 1,406 ms | 1,120 ms | 1,100 ms | −1,8 % | −21,8 % |
| 4K X6 | 0,766 ms | 0,687 ms | 0,675 ms | −1,7 % | −11,9 % |

RTX 3070 Ti : les étapes réécrites tournent 4 à 5 % (premier réseau) et 21 à 24 % (second réseau) plus vite que dans la 1.5.3 ; le gain sur l'image entière n'a pas encore été mesuré en jeu, ni sur RTX 20.

- **Plantage avec ReShade et le Neural Rendering (Starfield) :** avec des add-ons ReShade 6.8 comme Luma, le jeu pouvait se fermer sur une corruption de tas quelques minutes après le démarrage ou la re-création du Neural Rendering. Le Neural Rendering réutilise désormais ses descriptor heaps au lieu d'en créer et détruire un par image, ce qui sollicitait sans cesse un chemin fragile du proxy ReShade. Reproduit et corrigé sur l'installation Starfield du mainteneur avec les mods du rapporteur ; le mainteneur a ensuite joué 35 minutes avec le correctif et ces mêmes mods (ReShade et Luma), sans plantage. Merci au rapporteur de l'[issue 98](https://github.com/SilyNoMeta/rtx-encore/issues/98) pour ses journaux détaillés.
- **Curseur du menu dans Starfield et les autres jeux Raw Input :** dans les jeux qui lisent la souris sans déplacer le curseur Windows, le curseur du menu restait figé au milieu de l'écran jusqu'à ce que le menu pause du jeu le libère. Le menu déplace désormais son curseur à partir des paquets souris eux-mêmes. Vérifié dans Starfield ; overlay ReShade ouvert, ReShade garde toujours la souris pour lui, comme pour tout autre overlay.

### Problème connu

- **Starfield avec Luma 2.0 (HDR natif) :** les images générées prennent un aspect en relief, avec des franges rouges et cyan, quand la caméra bouge. Cela vient de la sortie HDR de Luma associée au DLSS Frame Generation, pas de cette version : le défaut apparaît aussi avec les versions précédentes et disparaît quand Luma est désactivé. Désactivez Luma ou sa sortie HDR.

## 1.5.3

- **Menu Microsoft Flight Simulator 2024 :** corrige l’apparition du menu dans les configurations Streamline compatibles où il restait absent. Merci à [@jtsorlinis](https://github.com/jtsorlinis) pour le signalement et les journaux détaillés ([#94](https://github.com/SilyNoMeta/rtx-encore/issues/94)). La confirmation du correctif par le rapporteur reste attendue.

- **XeFG avec les add-ons ReShade et les installations manuelles :** le moteur XeFG refusait de démarrer quand le device du jeu atteignait Streamline à travers un proxy ReShade (RenoDX et autres add-ons), et quand le mod était installé à la main sans le Hub, alors même que le jeu livrait les runtimes Intel qualifiés. Les deux cas passent désormais ; les runtimes installés par le Hub gardent la priorité quand ils existent. Signalé dans Crimson Desert ([issue 93](https://github.com/SilyNoMeta/rtx-encore/issues/93)) ; pas encore confirmé en jeu par le rapporteur.
- **Hotfix NVIDIA 621.12 :** les options XeFG et XeSS Tensor Cores, jusqu'ici refusées avec tout pilote autre que 621.01, acceptent aussi le hotfix 621.12 du 27 octobre. Son fichier pilote a été identifié depuis le paquet lui-même, sans validation en jeu ; si le pilote ne se comporte pas exactement comme attendu, les options reviennent au chemin Intel classique. Le Smooth Motion pilote sur RTX 20/30 exige toujours l'un de ses pilotes listés.
- **Preset F de Ray Reconstruction :** le preset de modèle RR propose désormais F, le transformer de seconde génération (RR2) livré avec la DLL Ray Reconstruction 310.9.1 et suivantes. Les DLL plus anciennes l'ignorent et gardent leur défaut ([issue 95](https://github.com/SilyNoMeta/rtx-encore/issues/95)).
- **XeFG et XeSS Tensor Cores sur RTX 30, toutes les RTX 40 et RTX 50 :** les deux options n'étaient admises que sur la RTX 4090. Elles acceptent désormais tout adaptateur NVIDIA RTX 30, RTX 40 et RTX 50 avec NVIDIA 621.01, avec la même accélération compilée pour chaque architecture. Les RTX 20 restent refusées et conservent le chemin Intel classique. Seule la RTX 4090 a été validée en jeu à ce jour ; indiquez votre GPU, votre pilote et votre jeu quand vous l'essayez ([issue 99](https://github.com/SilyNoMeta/rtx-encore/issues/99)).

Merci aussi à [@pradeepkumar35](https://github.com/pradeepkumar35) (#93), [@dm1z0r](https://github.com/dm1z0r) (#95) et [@Edward24RD](https://github.com/Edward24RD) (#99) pour leurs signalements et demandes. Les nouvelles admissions Tensor et 621.12 restent expérimentales ; la qualification en jeu demeure limitée à la configuration RTX 4090 / 621.01 antérieure. Aucun nouveau gain de performances n’est revendiqué. Les binaires finaux 1.5.3 n’ont pas de retest distinct en jeu. La publication vérifie compilation, régressions ciblées et intégrité des archives ; la suite complète n’a pas été relancée.

## 1.5.2

- **V-Sync activée :** le réglage V-Sync de Frames → Frame pacing propose désormais Jeu, Désactivé et Activé. Activé demande aux jeux DirectX de présenter avec V-Sync, pour les jeux qui grisent leur V-Sync avec la génération d’images et les portables à graphismes hybrides où la V-Sync du panneau de configuration NVIDIA est sans effet. Un réglage forcé dans le panneau NVIDIA garde la priorité ; sans effet dans les jeux Vulkan ou avec XeSS-FG ; Activé peut ajouter de la latence avec un multiplicateur élevé sur un écran 60 Hz. Un choix « V-Sync désactivée » enregistré est conservé. Merci à [@pradeepkumar35](https://github.com/pradeepkumar35) pour la demande ([#93](https://github.com/SilyNoMeta/rtx-encore/issues/93)).

Le propriétaire a contrôlé les bascules à chaud dans Crimson Desert sur RTX 3070 Ti Laptop avec graphiques hybrides, NVIDIA 621.01, D3D12 et DLSS-G 310.9.16. Le jeu a accepté les changements de présentation V-Sync demandés sans échec de Present pendant les sessions observées. La disparition visible du tearing, la régularité des images affichées et la latence ajoutée n’ont pas été mesurées ; le rapporteur n’a pas encore testé le correctif. Ces observations concernent un build de test ; les binaires finaux 1.5.2 n’ont pas de retest distinct en jeu.

Utilisez le menu en jeu pour le nouveau choix à trois valeurs : le Hub 1.5.2 ne le propose pas encore. La suite automatisée complète n’a pas été relancée ; compilation, contrôles ciblés et intégrité des livrables ont été vérifiés.

## 1.5.1

- **Compatibilité du menu et du démarrage :** corrige le menu absent dans les jeux compatibles qui créent leur affichage via Streamline, ainsi qu’un échec au démarrage avec le proxy `dxgi.dll` dans MechWarrior 5. Merci à [@kisoo8](https://github.com/kisoo8) pour le signalement et les journaux ([#92](https://github.com/SilyNoMeta/rtx-encore/issues/92)). Le propriétaire a confirmé le menu et les commandes de génération d’images dans A Plague Tale: Requiem, ainsi que le fonctionnement D3D11/D3D12 dans MechWarrior 5, sur des builds de test.
- **Smooth Motion RTX 20 :** V2 est désormais toujours utilisée. Le sélecteur V1/V2 et les fichiers V1 inutilisés sont retirés ; les anciens choix enregistrés sont ignorés. La fonction reste expérimentale, avec les mêmes conditions d’API graphique et de pilote. Le Hub 1.5.2 masque le sélecteur retiré pour le mod 1.5.1 et les suivants.
- **Nom du projet :** harmonise les identifiants du menu et des diagnostics avec RTX Encore.

Les binaires finaux n’ont pas de retest distinct en jeu. La validation de publication se limite à la compilation, aux contrôles ciblés et à l’intégrité des livrables ; la suite automatisée complète n’a pas été relancée.

- **Soutien :** merci à [@Scarhead666](https://github.com/Scarhead666) pour son soutien au développement de RTX Encore. Les remerciements aux sponsors publics figurent dans l’onglet À propos et les README du projet.

## 1.5.0

- **XeSS Tensor Cores :** nouvelle option NVIDIA Tensor indépendante, désactivée par défaut, pour XeSS Super Resolution, avec bascule à chaud et optimisation de la reconstruction finale. Inclut les corrections d’exposition automatique et de dimensions de rendu fractionnaires.
- **XeFG Tensor Cores :** nouvelle option NVIDIA Tensor indépendante, désactivée par défaut, pour XeFG, avec facteurs fixes X2–X6 dans la capacité admise et activation/désactivation à chaud. Améliore la récupération après changement de Ray Reconstruction dans les configurations compatibles.
- **Hub 1.5.0 et profils :** expose les deux interrupteurs, explique leur périmètre matériel initial et les conserve dans les profils communautaires. Installez le nouveau mod jeu fermé ; changer de moteur FG demande toujours un redémarrage.

Qualification initiale : RTX 4090, NVIDIA 621.01, DirectX 12 et runtimes Intel compatibles fournis par le Hub. Une préparation non prise en charge conserve le chemin Intel classique ; une erreur d’exécution Tensor impose un redémarrage. Les deux options restent expérimentales.

Des mesures XeSS TW3 sur un build antérieur montrent environ −30,7 % de temps GPU XeSS et +2,8 à +3,2 % de FPS source rendus dans la scène mesurée. Les changements finaux ultérieurs ont été qualifiés par replays déterministes, sans nouvelle comparaison en jeu. Aucun gain général de FPS ou de latence XeFG n’est établi. Le mainteneur a confirmé les commandes XeFG à chaud et la récupération RR sur des builds de test ; les binaires finaux 1.5.0 n’ont pas de retest distinct en jeu. [Conditions et commandes](docs/INTEL-ENGINES.fr.md).

## 1.4.7

- **The Witcher 3 :** corrige le scintillement persistant des ombres avec NR après un voyage rapide, encore présent en 1.4.6. Le mainteneur a testé le build corrigé sans scintillement, puis reproduit le problème après réinstallation de la 1.4.6 d’origine. Merci à [@Scarhead666](https://github.com/Scarhead666) pour le signalement et les comparaisons de versions ([#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)). Le rapporteur n’a pas encore testé ce nouveau correctif.

- **Documentation :** complète les tableaux des raccourcis par défaut et indique le caractère expérimental de Smooth Motion sur RTX 20, avec ses conditions DirectX 12 et pilote.

Les tests automatisés locaux couvrent les chemins corrigés. La comparaison en jeu du mainteneur utilisait un build de test ; les binaires finaux 1.4.7 n’ont pas de retest distinct en jeu.

## 1.4.6

- **Menu plus lisible :** les contrôles avancés n'apparaissent que lorsque leur fonction maîtresse est activée, notamment pour le NR fovéal, les détails de l'overlay et les captures. Leurs valeurs enregistrées sont conservées.
- **Mirage et Shadows :** corrige le menu absent dans Mirage et la remontée du multiplicateur dans Shadows. Dans les jeux DirectX 12 compatibles comme Mirage, choisissez **DLSS-FG** dans Encore et activez le **FG Intel** du jeu pour utiliser la génération NVIDIA avec les multiplicateurs fixes qualifiés. Le FSR natif et les jeux déjà en DLSS FG conservent leur moteur. Merci à [@kisoo8](https://github.com/kisoo8) pour le signalement et les logs ([#89](https://github.com/SilyNoMeta/rtx-encore/issues/89)). Le mainteneur a confirmé le dernier build de test dans Mirage, avec les artefacts de mouvement corrigés ; les binaires de release n'ont pas de retest distinct en jeu.
- **Reflex et latence :** ajoute Jeu / On / On+Boost et le rapport de latence dans le menu et l'overlay. Le réglage ne diminue pas le choix du jeu et se suspend en Dynamic ou lorsque XeLL gère la latence. Aucun gain de latence n'est garanti. Merci à [@kisoo8](https://github.com/kisoo8) pour les logs ([#87](https://github.com/SilyNoMeta/rtx-encore/issues/87)) ; le mainteneur a testé les commandes en jeu.
- **REFramework :** améliore la reconnaissance du runtime NR lorsqu'un même runtime compatible est chargé depuis un chemin redirigé. Merci à [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) pour le signalement ([#90](https://github.com/SilyNoMeta/rtx-encore/issues/90)). Correctif couvert par des tests automatisés, sans validation distincte en jeu.

- **The Witcher 3 :** tentative de correction du scintillement des ombres avec NR après un voyage rapide, signalé depuis la 1.4.3 par [@Scarhead666](https://github.com/Scarhead666) ([#88](https://github.com/SilyNoMeta/rtx-encore/issues/88)). Cette tentative était insuffisante : le rapporteur et le mainteneur ont ensuite confirmé le scintillement avec la 1.4.6.

## 1.4.5

- **Smooth Motion natif sur RTX 40 / RTX 50 :** le réglage existant peut activer le chemin natif du pilote NVIDIA avec un pilote pris en charge. Choisissez l'API réelle du jeu puis redémarrez-le. Le comportement RTX 20/30 et les versions exactes des pilotes restent inchangés. [Conditions](docs/SMOOTH-MOTION.fr.md).
- **Compteur et statut :** utilise l'estimation simple 2x demandée, y compris sur les cartes natives. Il ne s'agit pas d'une mesure des images affichées ou interpolées. Le statut distingue un pilote préparé d'une API graphique mal choisie.
- **Menu DirectX 11 :** corrige l'absence du menu dans certains jeux DirectX 11, notamment Men of War II.
- **Réglages à chaud et stabilité :** améliore les changements de preset DLSS, les travaux graphiques différés, la gestion des ressources et l'absence de suivi de fin d'exécution. Certaines configurations DirectX 12 autorisent au plus quatre remplacements à chaud ; redémarrez lorsque le menu le demande avant d'en effectuer d'autres. Le coût mémoire des ressources conservées n'a pas été mesuré en jeu.
- **HUD et commandes :** réessaie les allocations de textures HUD temporairement refusées pendant une durée limitée et corrige la réapplication des commandes de génération d'images en attente.

Le propriétaire a confirmé le menu dans Men of War II et la préparation/présentation Smooth Motion native sur RTX 4090 avec le pilote 621.01, sur des builds de test. Cela ne mesure pas les images interpolées, la qualité, la latence ou la stabilité prolongée. RTX 50 et Vulkan natif n'ont pas de qualification matérielle correspondante. Les tests automatisés couvrent les chemins corrigés ; les binaires finaux 1.4.5 n'ont pas de retest distinct en jeu. Intel XeSS / XeFG, Pre-SR et Smooth Motion expérimental sur RTX 20 conservent leurs limites. Le [Hub 1.4.4](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.4) ajoute les réglages natifs correspondants.

## 1.4.4

- **Avatar: Frontiers of Pandora :** corrige le grain coloré et les artefacts verts du Neural Rendering dans les scènes HDR très sombres. L'effet NR est volontairement atténué dans les ombres les plus faibles pour préserver l'image d'origine. Merci à [@DangerousBerries](https://github.com/DangerousBerries) pour son signalement ([#80](https://github.com/SilyNoMeta/rtx-encore/issues/80)).
- **Where Winds Meet et compatibilité NR Direct3D :** corrige le démarrage du Neural Rendering lorsqu'un jeu embarque un ancien compilateur de shaders. Conservez les fichiers d'origine du jeu ; il n'est pas nécessaire de renommer ou remplacer son compilateur. Merci à [@mikotorz](https://github.com/mikotorz) pour son signalement ([#85](https://github.com/SilyNoMeta/rtx-encore/issues/85)).
- Le [Hub 1.4.2](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.2) détecte l'exécutable de Where Winds Meet et demande confirmation des destinations d'installation incertaines.

Le propriétaire a confirmé l'amélioration dans la scène Avatar testée et le fonctionnement du NR dans Where Winds Meet sur des builds de test. Ces confirmations ne couvrent pas toutes les scènes ou configurations ; les binaires finaux 1.4.4 n'ont pas de nouveau test en jeu distinct. Les tests automatisés locaux couvrent la recomposition des ombres HDR et la compatibilité des shaders Direct3D. Qualité d'image, performances et stabilité temporelle ne sont pas qualifiées de façon générale. Intel XeSS / XeFG et Pre-SR restent expérimentaux.

## 1.4.3

- **Zenless Zone Zero :** corrige la compatibilité XeFG avec les fichiers Streamline d'origine du jeu et améliore le démarrage du menu intégré et du Neural Rendering. Conservez les fichiers d'origine du jeu. Merci à [@WBW333](https://github.com/WBW333) pour son signalement ([#83](https://github.com/SilyNoMeta/rtx-encore/issues/83)).
- **Composants Intel :** reconnaît les composants compatibles déjà installés dans les dossiers natifs du jeu. Le [Hub 1.4.1](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.1) apporte les changements correspondants d'installation et de réparation.
- **Neural Rendering :** accepte la profondeur flottante R16, dont le format signalé dans Alan Wake 2, ainsi que des formats supplémentaires de mouvement normalisé et d'entrées flottantes selon l'API graphique. Merci à [@MrMicko](https://github.com/MrMicko) pour son signalement et son journal Alan Wake 2 ([#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)).

Les correctifs ZZZ ont été vérifiés localement en jeu avec XeFG, le menu intégré et NR sur un build de test. Le nettoyage final au démarrage et les binaires finaux 1.4.3 n'ont pas de validation en jeu distincte. La conversion des entrées NR dispose de tests automatisés locaux, dont WARP et Vulkan sur RTX 4090 ; ils ne valident pas la qualité d'image ou les performances du NR NVIDIA. Aucune confirmation du correctif Alan Wake 2 par son auteur n'est revendiquée. Intel XeSS / XeFG et Pre-SR restent expérimentaux.

## 1.4.2

- **Comparaison Pre-SR / Post-SR :** corrige l'aperçu des captures fortement pixellisé dans le jeu. Affichez l'image entière dans le panneau, ou décochez **Fit to panel** pour l'inspecter à **1x**, avec un pixel capturé par pixel écran, puis zoomez et déplacez le cadrage pour examiner les détails.
- **Inspection des captures :** actualise l'aperçu après une nouvelle capture et affiche côte à côte les captures de dimensions différentes au lieu de superposer des images qui ne correspondent pas.

Le placement des pixels, l'échelle d'affichage et l'actualisation des captures disposent de tests CPU automatisés locaux et d'aperçus synthétiques du panneau. Les binaires finaux 1.4.2 ne sont pas validés en jeu ; qualité d'image, performances, coûts mémoire et latence restent à mesurer. Les captures sont des images successives, et le HDR est présenté en aperçu SDR. Les réglages et rapports de comparaison existants du Hub restent compatibles.

## 1.4.1

- **Stabilité :** améliore la gestion des changements de résolution, des réglages de reconstruction, de la capture du HUD et de la génération d'images lorsqu'une opération graphique échoue.
- **Gestion des erreurs :** corrige des problèmes de conservation des ressources, de chargement et d'allocation mémoire susceptibles de laisser un état périmé ou d'interrompre le jeu.
- **Réglages et diagnostics :** améliore la validation de la configuration, l'état du menu et la fiabilité des diagnostics optionnels.
- **Archives :** renforce les contrôles lors de la préparation des fichiers d'installation.

Les correctifs disposent de compilation locale et de tests CPU automatisés. Les binaires finaux 1.4.1 ne sont pas validés en jeu ; qualité d'image, performances, latence et coûts mémoire restent à mesurer. Certains risques de synchronisation et de durée de vie des composants restent ouverts. Intel XeSS / XeFG, Pre-SR et Smooth Motion sur RTX 20 restent expérimentaux.

## 1.4.0

- **NR avant reconstruction expérimental (Pre-SR) :** applique le Neural Rendering avant DLSS Super Resolution classique, avec une à quatre passes et un profil de modèle et une apparence indépendants par passe. Post-SR reste le défaut. Pre-SR exige une entrée uniforme à la taille de rendu ; RR, DLAA, fovéation, tailles avec marges et mouvements avec jitter ne sont pas disponibles. Une entrée non prise en charge conserve l'image SR d'origine.
- **Laboratoire image et performances :** mesure séparément Post-SR et Pre-SR, inspecte les captures avec zoom ou balayage A/B, et exporte JSON, CSV et une visionneuse locale. Les mesures manquantes restent indisponibles. Les compteurs mémoire gardent leurs périmètres distincts ; ils ne prouvent pas une économie causée par Pre-SR. Une paire n'est comparable que si les conditions SR/FG observées et leur association au viewport sont qualifiées. Les routes actuelles sans cette association affichent des mesures individuelles et captures, sans gain relatif.
- **Éditeur des passes NR :** cartes adaptatives, ordre de traitement affiché, modèle et apparence indépendants par passe. [Prérequis et limites des comparaisons](docs/NEURAL-RENDERING.fr.md).
- **Commandes DLSS-G :** corrige la priorité des modes Fixe, Dynamique et Jeu choisis dans Encore et applique sa cible dynamique au lieu de conserver celle du jeu. Une cible n'est pas un plafond strict de FPS affichés. Merci à [@wer2357666](https://github.com/wer2357666) pour le rapport et les journaux ([Hub #4](https://github.com/SilyNoMeta/rtx-encore-hub/issues/4)).
- **Langues du menu en jeu :** anglais, français et chinois simplifié, choix enregistré par jeu et repli anglais pour les messages non traduits. Merci à [@pandaligx](https://github.com/pandaligx) pour la demande ([#78](https://github.com/SilyNoMeta/rtx-encore/issues/78)).
- **Onglet À propos :** version, liens du projet et du Hub, GitHub Sponsors, Ko-fi et remerciements aux sponsors publics, dont [@mennogreg](https://github.com/mennogreg).

Les nouveaux chemins Pre-SR, changements multipasses, instruments de comparaison et commandes DLSS-G ont une compilation locale et des tests CPU ; GPU, qualité en jeu, performances et latence physique restent non validés pour cette version. Les captures séquentielles sont des images différentes et ne prouvent pas la stabilité temporelle. Intel XeSS / XeFG et Smooth Motion sur RTX 20 restent expérimentaux. Le [Hub 1.4.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v1.4.0) ajoute les réglages correspondants et la lecture des rapports.

## 1.3.1

- **Alan Wake 2 :** corrige le Neural Rendering qui ne démarrait pas avec l'exposition du jeu et améliore la gestion de la souris lorsque le menu est ouvert. Merci à [@SymbioticTortoise](https://github.com/SymbioticTortoise) pour le rapport et les journaux ([#76](https://github.com/SilyNoMeta/rtx-encore/issues/76)).
- **Neural Rendering :** accepte davantage de formats d'exposition flottants (RG16, RG32 et RGB32) sous DirectX 11, DirectX 12 et Vulkan, selon les formats disponibles dans chaque API.
- **Neural Rendering Vulkan :** active les chemins RTX 20, RTX 40 et RTX 50 en plus des RTX 30. Merci à [@Yukk1No](https://github.com/Yukk1No) pour le rapport sur RTX 4070 SUPER ([#74](https://github.com/SilyNoMeta/rtx-encore/issues/74)). Ces générations supplémentaires restent expérimentales et n'ont pas été validées sur de vraies cartes avec cette version.
- **Compatibilité Vulkan :** améliore la compatibilité avec les autres mods graphiques, à la suite des rapports Baldur's Gate 3 / bg3fgvk de [@deusimperatork](https://github.com/deusimperatork) ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)). Les combinaisons avec OptiScaler restent non prises en charge.
- **WRC Generations :** prend en charge l'intégration Streamline 1.3.3 d'origine et corrige l'état Frame Generation qui restait sur Waiting dans le menu quand le jeu activait la FG. Conservez ensemble les fichiers Streamline d'origine ; le mélange de composants anciens et récents n'est pas pris en charge. Merci à [@dm1z0r](https://github.com/dm1z0r) pour le rapport ([#75](https://github.com/SilyNoMeta/rtx-encore/issues/75)).

Le mainteneur a confirmé le fonctionnement d'Alan Wake 2 et l'état FG du menu de WRC sur des builds de test. Le menu de WRC indique le mode activé, sans mesurer le multiplicateur affiché. Les binaires finaux 1.3.1 n'ont pas été retestés en jeu. Les formats d'exposition supplémentaires ont des tests automatisés locaux, sans validation visuelle ou de performances sur chaque API. Intel XeSS / XeFG et Smooth Motion sur RTX 20 restent expérimentaux.

## 1.3.0

- Neural Rendering fovéal (expérimental, désactivé par défaut) : le NR travaille à pleine force au milieu de
  l’image et plus légèrement vers les bords, pour plus de FPS avec un effet NR plus doux près des bords. Dans le
  menu, activez **Foveated NR** et choisissez un préréglage (Light, Balanced, Strong) ; les valeurs derrière se
  trouvent sous **Foveated NR: custom values**. Strong a donné +27 % de FPS moyen dans Cyberpunk 2077 sur une
  RTX 3070 Ti Laptop. Merci à [maximedgoulet](https://github.com/maximedgoulet) pour l’idée
  ([FoveatedDLSS5](https://github.com/maximedgoulet/FoveatedDLSS5)).

- Le Neural Rendering suit correctement le mouvement dans les jeux dont les vecteurs de mouvement sont en pleine
  résolution (Unreal Engine 5, Black Myth: Wukong) : il n’en voyait qu’une partie.

- **Baldur's Gate 3 (Vulkan) avec bg3fgvk :** ne plante plus au lancement, et le Neural Rendering, DLSS et DLSS Frame Generation y fonctionnent désormais ensemble. Merci à [@deusimperatork](https://github.com/deusimperatork) pour ses rapports et journaux détaillés ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)). L'utilisation simultanée d'OptiScaler n'est pas prise en charge dans ce jeu.

- Retrait du panneau optionnel Cyber Engine Tweaks. Utilisez le menu intégré (Inser) ou le Hub. Au premier lancement, les anciens réglages du panneau sont copiés à côté du mod si aucun fichier actuel n’existe ; l’original est conservé.

- Retrait du moteur expérimental OpenNR et de ses réglages. Neural Rendering NVIDIA reste disponible. Les anciennes configurations OpenNR migrent avec NR désactivé ; réactivez-le pour utiliser NVIDIA NR.

- Smooth Motion sur RTX 20 avec NVIDIA 621.01 et DirectX 12. Activez Smooth Motion ;
  RTX Encore sélectionne automatiquement la carte. Choisissez **V1 (test 1, défaut)**
  ou **V2 (test 2)** dans le Hub ou le menu en jeu des RTX 20, puis relancez le jeu.
  Merci à [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) pour les tests
  sur RTX 2070 dans cinq jeux et sa préférence V1 dans Resident Evil Requiem, et à
  [@MxHassan195](https://github.com/MxHassan195) pour les deux essais sur RTX 2060
  dans The Witcher 3 et sa préférence pour la fluidité de V2
  ([#53](https://github.com/SilyNoMeta/rtx-encore/issues/53)). Les performances dépendent
  de la carte et du jeu ; ces retours portent sur les builds expérimentaux, pas la DLL finale.

## 1.2.2

- **Netteté après reconstruction.** Choisissez Off, Game ou Custom pour DLSS, DLAA, Ray Reconstruction et le remplacement XeSS. Désactivée par défaut ; Custom démarre à 30 % et les changements sont immédiats. Une intensité élevée peut créer des halos et amplifier le bruit. Un DLSS d'origine antérieur à 2.5.1 conserve sa propre netteté. Merci à [@aziznasrimay45-sys](https://github.com/aziznasrimay45-sys) pour la demande ([#70](https://github.com/SilyNoMeta/rtx-encore/issues/70)).
- **Smooth Motion démarre correctement dans SurrounDead (DirectX 12).** Le mainteneur a confirmé son fonctionnement en jeu sur une RTX 3070 Ti Laptop avec le pilote NVIDIA 621.01.
- **Baldur's Gate 3 (Vulkan) :** correction d'un cas où le menu intégré basculait dans une fenêtre séparée avec bg3fgvk et OptiScaler F5. Merci à [@deusimperatork](https://github.com/deusimperatork) pour le signalement ([#72](https://github.com/SilyNoMeta/rtx-encore/issues/72)).
- **Compatibilité de DLSS Frame Generation avec OptiScaler F5 :** correction d'un cas où Frame Generation devenait indisponible sur RTX 20/30, notamment dans Final Fantasy VII Rebirth. Merci à [@lovebabyx](https://github.com/lovebabyx) pour le signalement ([#73](https://github.com/SilyNoMeta/rtx-encore/issues/73)).

La netteté dispose de contrôles automatisés locaux ; son fonctionnement Vulkan, son aspect en jeu et ses performances restent à valider. Les auteurs des signalements #72 et #73 n'ont pas confirmé les correctifs sur cette release. Intel XeSS et XeFG restent expérimentaux. [Réglages de netteté](docs/SUPER-RESOLUTION.fr.md).

## 1.2.1

- Passez de DLSS à XeSS et inversement sans redémarrer dans No Man’s Sky, avec des runtimes Intel compatibles installés avant le lancement.

- **Elden Ring avec ERSS-FG :** meilleure compatibilité au démarrage et pour les menus. Merci à
  [@mennogreg](https://github.com/mennogreg) pour le [signalement](https://github.com/SilyNoMeta/rtx-encore/issues/64).
- **Compatibilité du menu** avec OptiScaler dans Quarantine Zone et ReShade dans HumanitZ. Merci à
  [@PhantomGamers](https://github.com/PhantomGamers) ([#54](https://github.com/SilyNoMeta/rtx-encore/issues/54)) et
  [@sweddd](https://github.com/sweddd) ([#68](https://github.com/SilyNoMeta/rtx-encore/issues/68)) pour leurs signalements.
- **Neural Rendering** conserve les passes configurées quand la résolution de rendu change. Merci à
  [@amernet91](https://github.com/amernet91) pour le [signalement](https://github.com/SilyNoMeta/rtx-encore/issues/65).
- Mise à jour de la compatibilité des versions des runtimes Intel.
- **La compatibilité des runtimes NVIDIA** n'exige plus que le fichier figure dans une liste prédéfinie.
  Une ancienne entrée `providerPatchPolicy` est ignorée. Les contrôles Intel restent inchangés.

## 1.2.0

**Intel XeSS et XeFG sont nouveaux et expérimentaux.** Attendez-vous à des bugs, crashs, artefacts et
différences entre jeux et API graphiques. N'hésitez pas à remonter les problèmes : dans RTX Encore Hub, ouvrez la
page **Santé** du jeu, choisissez **Préparer un diagnostic**, relisez l'archive pour retirer les informations
personnelles, puis joignez-la à une [issue RTX Encore](https://github.com/SilyNoMeta/rtx-encore/issues/new/choose).
Indiquez les moteurs choisis, l'API graphique et le multiplicateur demandé. Le Hub ne téléverse pas l'archive à
votre place.

### Nouveautés

- **Intel XeSS et XeFG (expérimental).** Choisissez le moteur de reconstruction (DLSS ou XeSS Super Resolution) et
  le moteur de génération d'images (DLSS-G ou XeFG avec XeLL). Laissez **Game** pour conserver le moteur habituel
  du jeu. Les modes Intel sont désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d'un
  moteur ne prouve pas son activation. Installez les runtimes Intel avec RTX Encore Hub. Voir
  [Intel XeSS et XeFG (expérimental)](docs/INTEL-ENGINES.fr.md).
- **Ray Reconstruction + Neural Rendering (Direct3D 12).** Les jeux compatibles utilisant une profondeur linéaire
  peuvent désormais appliquer NR après RR. Direct3D 11 et Vulkan ne prennent toujours pas en charge ce cas.
- **Réglages Neural Rendering par passe.** Chacune des quatre passes NR possibles peut remplacer l'intensité, le ton
  local, la structure locale, la structure de peau et le masque de personnage, ou suivre la valeur globale. Les
  valeurs numériques vont de 0 à 2 ; le masque prend `inherit`, `true` ou `false`. Après la première passe, le ton
  hérité vient de `nrLaterPassLocalTone`. Une intensité à zéro laisse la passe tourner et ne supprime pas son coût
  GPU/VRAM.
- **L'entrée HDR et l'exposition automatique** utilisent les réglages communs `srHdr` et `srAutoExposure`, pour le
  moteur de reconstruction sélectionné. Ils suivent le jeu par défaut (`game`) et demandent un redémarrage. Ils
  n'activent pas le HDR de l'écran et ne convertissent pas une image SDR en HDR ; un format d'entrée incorrect peut
  dégrader l'image. Certains modèles DLSS imposent l'exposition automatique ; celle de XeSS a un coût GPU et est
  déconseillée en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge.
- Les fichiers JSONC existants sont réordonnés par fonction, avec les options Intel près du choix des moteurs. Les
  valeurs choisies, commentaires et réglages personnels sont conservés.
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
XeFG X4 dans GTA V Enhanced, XeSS SR dans 007 First Light, XeFG X2 dans No Man's Sky et DOOM: The Dark Ages (Vulkan,
fenêtre sans bordure, NR coupé), NR après RR dans 007 First Light en path tracing (qualité d'image et coût non
mesurés). Le changement de preset en cours de jeu, les forçages HDR/exposition et les admissions de fournisseurs
n'ont que des tests automatisés ; les autres jeux, GPU et pilotes n'ont pas été testés. Sur une route Intel, le menu
peut être indisponible : utilisez le Hub pour régler le mod.

### Problème connu

- **007 First Light** peut encore planter par intermittence au démarrage ; un autre lancement peut réussir.
- **L'enregistreur de SCSKiller n'est pas compatible avec RTX Encore.** Quand SCSKiller enregistre un jeu, son
  enregistreur prend la création de la swap chain du jeu avant RTX Encore : XeFG est refusé et le DLSS-G du jeu reste
  utilisé (le chip Frame Generation affiche DLSS-G). Dans The Witcher 3, le jeu pouvait aussi se figer au chargement,
  avec un reset du GPU au démarrage de la génération d'images. Vous pouvez garder SCSKiller : dans ses **Settings**, désactivez **Record in all
  compatible games** (et l'enregistrement sur la page du jeu si vous l'y avez activé), puis relancez le jeu. D'autres
  outils d'enregistrement et overlays peuvent avoir le même effet.

Merci à [@SanyaGames227](https://github.com/SanyaGames227) pour la demande de réglages d'entrée HDR et
d'exposition automatique ([#63](https://github.com/SilyNoMeta/rtx-encore/issues/63)), et à
[@Theoryedz](https://github.com/Theoryedz) pour le signalement dans
[#58](https://github.com/SilyNoMeta/rtx-encore/issues/58). Les rapporteurs n'ont pas testé cette version.

Merci à [@dashdogy](https://github.com/dashdogy) (MFG Unlocker) pour l'idée de changer les presets DLSS et le moteur de reconstruction pendant que le jeu tourne.

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
