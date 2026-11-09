# Jeux sans DLSS

*[English](GAMES-WITHOUT-DLSS.md) · [中文](GAMES-WITHOUT-DLSS.zh-CN.md)*

Beaucoup de jeux n’ont aucun DLSS : ni upscaler, ni génération d’images, ni Neural Rendering. Depuis la 2.0.0, RTX
Encore leur apporte les trois, à partir de l’image qu’ils affichent, en DirectX 11, DirectX 12 et Vulkan, sans ReShade
ni aucun autre outil :

- **Reconstruction** : un DLAA sur l’image affichée par le jeu, ou un upscaling d’une résolution de jeu plus basse vers
  celle de l’écran, avec NVIDIA DLSS Super Resolution ou Intel XeSS (XeSS sur les Tensor Cores NVIDIA en DirectX 12).
- **Neural Rendering** : le [Neural Rendering](NEURAL-RENDERING.fr.md) de NVIDIA sur cette même image, après la
  reconstruction ou seul.
- **Génération d’images** : NVIDIA DLSS Frame Generation (DirectX 11, DirectX 12 et Vulkan, X2 à X6) ou Intel XeFG (DirectX 12
  jusqu’à X6 avec les Tensor Cores, Vulkan X2 à X6 avec le HDR10, DirectX 11 X2 à X6), avec un multiplicateur que vous
  changez en jouant.

Chaque fonction marche seule ou avec les autres. Elles sont expérimentales et désactivées par défaut. Les jeux qui ont
leur propre DLSS le gardent : ces options y restent masquées et RTX Encore travaille avec le DLSS du jeu comme avant.

**Testé par le mainteneur** (RTX 4090, NVIDIA 621.01) : PEAK en DirectX 12, DirectX 11 et Vulkan (reconstruction,
Neural Rendering, DLSS Frame Generation X2 et X4, XeFG X2 à X4), The Sims 4 en DirectX 11 (XeFG X2), Football Life
2027 en DirectX 11 (DLAA, upscaling vers la 4K et Neural Rendering, en HDR). Les autres jeux, cartes et pilotes n’ont
pas été vérifiés : dites-nous ce que vous voyez dans une [issue](https://github.com/SilyNoMeta/rtx-encore/issues).

## Le principe, en bref

RTX Encore prend l’image terminée juste avant son affichage. Le mouvement entre deux images est estimé à partir des
images elles-mêmes, sur la carte graphique (par l’estimation propre à RTX Encore, par défaut), et le depth buffer du
jeu est trouvé automatiquement quand le jeu dessine avec. Le DLSS ou XeSS, le Neural Rendering et la génération
d’images travaillent ensuite sur cette image, et c’est le résultat qui s’affiche.

Comme l’image est terminée, **le HUD et les textes sont traités avec elle** : agrandis, retouchés et, avec la
génération d’images, interpolés comme le reste. La plupart du temps, cela ne se voit pas ; un petit texte sur une scène
en mouvement peut laisser une légère traînée, et un HUD qui bouge vite peut se dédoubler dans les images générées.

## Prérequis

- Une carte NVIDIA RTX 20, 30, 40 ou 50 et un pilote récent. Les choix Optical Flow de l’estimation du mouvement
  utilisent NVIDIA Optical Flow, qui fait partie du pilote.
- DirectX 11, DirectX 12 ou Vulkan. En Vulkan, le device du jeu doit accepter les fonctions que RTX Encore demande à
  son démarrage ; sinon, ces options n’ont pas d’effet dans ce jeu. Un jeu Vulkan peut mettre jusqu’à dix secondes de
  plus à démarrer, le temps que la génération d’images se prépare.
- Les fichiers NVIDIA et Intel que chaque fonction utilise, que **RTX Encore Hub 2.0.0 installe pour vous** quand vous
  installez ou mettez à jour le mod dans un jeu sans DLSS (voir plus bas). À la main :
  - DLSS : le fichier NVIDIA `nvngx_dlss.dll` à côté de l’exécutable du jeu ou de RTX Encore.
  - Neural Rendering : le fichier NVIDIA `nvngx_dlssnr.dll`, comme sur la page
    [Neural Rendering](NEURAL-RENDERING.fr.md#prérequis).
  - DLSS Frame Generation : les six fichiers NVIDIA de génération d’images à côté de l’exécutable du jeu, dans les
    versions que RTX Encore accepte pour cet usage (Streamline 2.14.16 et DLSS Frame Generation 310.9.16). Les autres
    versions sont refusées, et le jeu tourne alors sans génération d’images.
  - XeSS et XeFG : les runtimes Intel, comme sur la page [Intel XeSS et XeFG](INTEL-ENGINES.fr.md).
- La génération d’images demande aussi que le **Smooth Motion** de NVIDIA soit désactivé dans RTX Encore pour ce jeu :
  les deux ne se cumulent pas.

## Installer avec RTX Encore Hub

[RTX Encore Hub 2.0.0](https://github.com/SilyNoMeta/rtx-encore-hub/releases/tag/v2.0.0) sait quels jeux n’ont pas
de DLSS. Pour un tel jeu, **Installer**, **Configurer avec le mod** et les mises à jour placent à côté du mod tout ce
dont cette page a besoin : les fichiers DLSS et Neural Rendering, les fichiers de génération d’images à côté de
l’exécutable quand le jeu n’a pas de génération d’images à lui, et les runtimes Intel. Rien n’est activé pour autant :
les réglages restent ceux du mod par défaut, et vous choisissez chaque fonction dans le menu en jeu ou dans le
formulaire de réglages du Hub. **Restaurer le jeu** retire tout cela. Un jeu qui a sa propre génération d’images ne
reçoit jamais les fichiers de génération d’images.

## L’activer

Ouvrez le menu (**Inser**). Dans un jeu sans DLSS, les onglets **Qualité d’image** et **Fluidité** montrent ces commandes à la
place de celles des jeux avec DLSS.

### Reconstruction

Dans **Qualité d’image → Reconstruction → Moteur**, choisissez **DLSS** ou **XeSS** pour activer la reconstruction, ou **Aucun** pour la désactiver. Il n’y a plus d’interrupteur d’activation séparé. La sortie par défaut est **Résolution du jeu (DLAA)** : DLSS fonctionne à la résolution native ; XeSS applique son anticrénelage natif. Pour agrandir l’image, lancez le jeu en **Fenêtré**, pas en fenêtré sans bordure, à la résolution d’entrée souhaitée, puis choisissez **Résolution de l’écran** dans Encore. Encore agrandit la sortie et retire les bordures. Par exemple, 2560 × 1440 vers 3840 × 2160 correspond à environ 67 % par dimension.

**Nouveau dans la 2.2.0 :** choisissez **NIS** dans le même menu Moteur pour une mise à l'échelle spatiale sans runtime DLSS ou Intel, y compris en DirectX 11. Réglez la **netteté** de 0 à 100 %, puis choisissez éventuellement **SMAA** ou **FXAA** avant la mise à l'échelle (résolution source) ou après (résolution de sortie). Ces filtres agissent sur la couleur SDR et comprennent le HUD ; comparez-les en jeu.

### Résolution et mise à l’échelle Windows

Avec **Résolution de l’écran**, le jeu choisit la résolution d’entrée et Encore conserve la sortie à la résolution de l’écran. **1920×1080 → 2560×1440 correspond à 75 % par axe**. Une sortie annoncée en 1440p ne signifie pas que le jeu calcule l’image en 1440p ; il est normal que l’image remplisse toujours le même écran. Réappliquer le 1080p quand le jeu fonctionne déjà en 1080p ne change pas la résolution.

La mise à l’échelle Windows est un réglage distinct. Sur un écran 2560×1440 à **150 %**, un jeu sans prise en charge haute résolution peut ne voir qu’environ **1707×960**. Cela peut plafonner sa résolution et rendre les textes flous. Gardez votre mise à l’échelle Windows habituelle ; vérifiez d’abord la prise en charge DPI du jeu.

Pour un jeu Unreal Engine présentant ce symptôme :

1. Fermez le jeu et sauvegardez son `Engine.ini` dans son dossier de configuration, souvent `%LOCALAPPDATA%\<Jeu>\Saved\Config\WindowsNoEditor\` (certains jeux utilisent `Windows`). Il s’agit du fichier du jeu, pas de la configuration d’Encore.
2. Dans la section `[/Script/Engine.UserInterfaceSettings]` existante, mettez `bAllowHighDPIInGameMode=True`. Ajoutez la section si elle n’existe pas et conservez les autres réglages :

   ```ini
   [/Script/Engine.UserInterfaceSettings]
   bAllowHighDPIInGameMode=True
   ```

3. Relancez le jeu. Activez **DLSS** ou **XeSS**, choisissez **Résolution de l’écran**, puis appliquez la résolution souhaitée dans les graphismes du jeu. Pour comparer sur un écran 1440p, appliquez 2560×1440 puis 1920×1080. Ce dernier choix doit donner une entrée 1080p et une sortie 1440p, soit environ 75 %.
4. Vérifiez aussi les boutons du jeu. Si le curseur reste décalé, remettez le réglage précédent et signalez le jeu, la mise à l’échelle Windows, la résolution du jeu et le mode de sortie Encore. Une image plus nette ne prouve pas à elle seule que les clics sont alignés.

Epic documente ce [réglage haute résolution d’Unreal](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UUserInterfaceSettings/bAllowHighDPIInGameMode?application_version=4.27). Cette configuration a été confirmée dans **Islands of Insight, DirectX 11, sur un écran 1440p à 150 %**, avec la version testée pour ce correctif. Elle ne garantit pas tous les jeux Unreal ni les anciennes versions d’Encore : un jeu peut ignorer l’option ou utiliser un autre dossier. Restaurez le réglage sauvegardé si cela n’aide pas. La reconstruction ajoutée continue de traiter les textes et le HUD avec le reste de l’image.

### Neural Rendering

Onglet **Qualité d’image**, section **Neural Rendering** : activez **Neural Rendering**. Dans un jeu sans DLSS, ce seul
interrupteur lance le NR sur l’image affichée, après la reconstruction si elle est active, ou seul. Tous les réglages
NR de la page [Neural Rendering](NEURAL-RENDERING.fr.md) s’appliquent ensuite (passes, styles, résolution, précision
Fast sur RTX 30, luminosité HDR). **Ctrl + Alt + N** l’active et le désactive en jouant.

### Génération d’images

Dans **Fluidité → Génération d’images → Moteur**, choisissez **DLSS-FG** ou **XeFG**, ou **Aucun** pour désactiver la génération ajoutée. Les deux moteurs prennent en charge DirectX 11, DirectX 12 et Vulkan sur les configurations compatibles. Désactivez Smooth Motion et relancez le jeu lorsque demandé. Le mode **Fixe** permet X2 à X6 ; X5 et X6 restent expérimentaux. **Jeu** désigne l’intégration native du jeu, pas l’activation dans un jeu sans génération d’images.

### Estimation du mouvement

Sous les commandes de reconstruction, **Estimation du mouvement** choisit comment RTX Encore mesure le mouvement entre
deux images, pour la reconstruction, le Neural Rendering et la génération d’images à la fois. Vous pouvez en changer en
jouant.

| Choix | Ce que c’est | Coût en 4K sur une RTX 4090 |
| :--- | :--- | :--- |
| **RTX Encore** (par défaut) | L’estimation propre à RTX Encore, dans les shaders de la carte graphique : des vecteurs fins, un par 2×2 pixels, qui suivent les bords des objets et s’appuient sur le depth buffer quand le jeu en a un. Pensée pour la génération d’images à partir de ces images ; sans accélérateur. | environ 1,0 ms par image |
| **Shader (vort)** | Estimé aussi dans des shaders, selon la méthode de vort_Motion de vortigern11. Comparez-le avec RTX Encore dans votre jeu. | environ 0,6 ms par image |
| **Optical Flow (Qualité)** | NVIDIA Optical Flow, l’accélérateur des cartes RTX. | — |
| **Optical Flow (Rapide)** | NVIDIA Optical Flow dans son mode rapide : environ une milliseconde de moins par image, mais un mouvement un peu moins précis, ce qui peut laisser plus de traînées sur les mouvements rapides. | — |

## XeSS à la place du DLSS

- XeSS va de 1x (anticrénelage natif) à 3x : sur un écran 4K, la résolution du jeu doit être d’au moins 1280×720. En
  dessous, l’image du jeu est affichée étirée, sans XeSS.
- **Tensor Cores NVIDIA (expérimental)**, proposé sous le moteur quand XeSS est choisi dans un jeu DirectX 12, fait
  tourner XeSS sur les Tensor Cores, avec la même qualification que sur la page [Intel XeSS et XeFG](INTEL-ENGINES.fr.md).
  Les jeux DirectX 11 et Vulkan utilisent le XeSS standard et ne montrent pas l’interrupteur. L’activer ou le désactiver
  recrée XeSS (un petit à-coup). La ligne sous l’interrupteur indique ce qu’a utilisé la dernière image XeSS : Tensor
  Cores actifs, prêts mais inutilisés, ou indisponibles et pourquoi ; la puce **Reconstruction** porte un badge vert
  **Tensor Cores** tant qu’ils tournent.
- Les premières images après le lancement du jeu s’affichent sans XeSS, le temps que RTX Encore vérifie le runtime
  Intel.

## La génération d’images, API par API

| | DLSS Frame Generation | Intel XeFG |
| :--- | :--- | :--- |
| DirectX 12 | X2 à X6 | X2 à X6 ; **Tensor Cores NVIDIA** disponibles, avec la même qualification que sur la page [Intel XeSS et XeFG](INTEL-ENGINES.fr.md) |
| Vulkan | X2 à X6 | X2 à X6, multiplicateur changé à chaud ; SDR et HDR10 |
| DirectX 11 | X2–X6 | X2 à X6, multiplicateur modifiable en jouant ; SDR et HDR10 |

- **HDR.** Un jeu qui affiche en HDR10 (10 bits) sur un écran que Windows fait tourner en HDR reçoit la génération
  d’images en HDR. Un jeu qui affiche en scRGB (16 bits flottants) garde sa propre sortie : pas de génération d’images
  là, le reste de cette page s’applique toujours.
- Le **plein écran exclusif** en DirectX 11 est affiché comme une fenêtre sans bordure tant que XeFG tourne.
- Le mainteneur a trouvé la cadence régulière dans PEAK en DirectX 12 et en Vulkan à X4. En Vulkan, quand le jeu
  affiche sans V-Sync, les images générées sont montrées sans attendre le rafraîchissement de l’écran, comme le fait
  DLSS Frame Generation ; la fréquence du jeu n’est alors pas tenue au taux de rafraîchissement divisé par le
  multiplicateur.
- En DirectX 11, l’image suivante du jeu attend le travail de XeFG sur la précédente, comme en DirectX 12, et le X3
  est régulier. Là où l’écran autorise le tearing, la sortie n’est pas tenue à son taux de rafraîchissement : le
  mainteneur a vu XeFG au-delà de 144 FPS sur un écran 144 Hz dans The Sims 4, et X3 à X6 dans Skyrim Special Edition.
- Un jeu qui a déjà DLSS Frame Generation, Streamline, Intel XeFG ou une génération d’images AMD le garde : RTX Encore
  ne lui ajoute jamais une seconde génération d’images. Pour ces jeux, voir [Génération d’images](FRAME-GENERATION.fr.md)
  et [Intel XeSS et XeFG](INTEL-ENGINES.fr.md).

## Agrandir jusqu’à la résolution de l’écran

1. Choisissez **Résolution de l’écran** comme sortie.
2. Dans le jeu, choisissez une résolution plus basse (par exemple 2560×1440 sur un écran 4K) et le mode **fenêtré**.
3. RTX Encore rend la fenêtre sans bordure sur tout l’écran et le DLSS (ou XeSS) y agrandit l’image du jeu. La souris
   fonctionne toujours à la résolution du jeu. Cela s’applique au prochain changement de résolution du jeu, ou après
   un redémarrage.

Dans les jeux **Unity**, choisissez **Fenêtré**, pas **Plein écran fenêtré** : sous la résolution de l’écran, le plein
écran fenêtré étire lui-même l’image avant que RTX Encore ne la voie, et le DLSS ne peut alors faire qu’un DLAA. Le
plein écran exclusif garde la résolution du jeu.

Par rapport aux jeux qui ont un DLSS, l’image agrandie est un peu plus douce : le jeu ne fait pas trembler sa caméra
pour la reconstruction (jitter), qui reconstruit donc moins bien les détails fins. Le DLAA à la résolution du jeu n’a
pas cette limite.

## Limites

- Le HUD est traité avec l’image (voir plus haut), et interpolé avec elle dans les images générées.
- Les mouvements rapides, les zones découvertes et les ralentis zoomés peuvent laisser plus de traînées que dans un
  jeu avec DLSS, dans l’image agrandie comme dans les images générées.
- Sans depth buffer (ou s’il n’est pas trouvé), tout fonctionne quand même, avec un mouvement un peu moins stable.
- L’image agrandie est un peu plus douce que le DLSS d’un jeu, qui dispose du jitter de la caméra ; le DLAA n’a pas
  cette limite.
- XeFG sur les Tensor Cores fonctionne aussi à titre expérimental en DirectX 11 et Vulkan ; XeSS sur les Tensor Cores y est proposé lorsque le chemin le permet. Remplacer le DLSS d'un jeu Vulkan par XeSS conserve le XeSS standard.
- Une sortie scRGB (16 bits flottants) garde les images du jeu : pas de génération d’images.
- La génération d’images et le Smooth Motion de NVIDIA ne se combinent pas : désactivez Smooth Motion.
- Les jeux DirectX 11 qui tournent par-dessus DirectX 12 (certaines couches de traduction) ne sont pas pris en charge.
- Les jeux Vulkan dont le device refuse les fonctions supplémentaires fonctionnent normalement, sans ces options.
- Rien de tout cela n’a encore été validé sur RTX 20, RTX 30 ou RTX 50, ni avec un autre pilote que le 621.01.

## Si quelque chose ne marche pas

- Vérifiez que les options sont visibles : elles sont masquées dans les jeux où RTX Encore voit le DLSS ou la
  génération d’images du jeu.
- La puce **Reconstruction** du menu indique le moteur qui tourne et l’échelle ; la puce **Génération d’images** indique
  le moteur et le multiplicateur, ou pourquoi rien n’est généré.
- Rien n’est agrandi, retouché ni généré : vérifiez que les fichiers sont là. Dans le Hub, la page du jeu les liste et
  propose d’écrire ceux qui manquent ; à la main, voir les [prérequis](#prérequis). Sans les bons fichiers de
  génération d’images, le jeu tourne simplement sans génération d’images.
- XeSS ne tourne pas : vérifiez que le Hub a installé les runtimes Intel pour ce jeu, puis relancez le jeu. Sous
  **Moteur**, le menu dit quand XeSS est choisi mais que le DLSS a tourné à sa place, et pourquoi.
- La génération d’images ne démarre pas : désactivez **Smooth Motion**, vérifiez que le moteur a été choisi avant le
  redémarrage, et que le jeu n’a pas sa propre génération d’images.
- L’image est noire ou étirée : voir [Agrandir jusqu’à la résolution de l’écran](#agrandir-jusquà-la-résolution-de-lécran)
  pour le mode de fenêtre, et la limite 3x de XeSS plus haut.
- Signalez-le avec le rapport du menu (onglet **System**, **Copy**) : voir le [dépannage](TROUBLESHOOTING.fr.md).

## Compatibilité et limites mesurées

La prise en charge d’une API ne garantit pas tous les jeux. Le DLSS-G fixe peut demander x2–x6 sur les routes qualifiées ; x5/x6 restent expérimentaux et dépendent du GPU et des runtimes chargés. Les facteurs XeFG dépendent du profil de runtime admis.

L’estimateur de mouvement hybride Encore mesure 0,985 ms GPU en 3840×2160 sur la RTX 4090 de test. Ce chiffre concerne l’estimation du mouvement, pas le coût total de la reconstruction et du FG ; il varie selon le matériel et la scène.

La reconstruction pour les jeux sans DLSS traite aussi les menus : un texte nouvellement apparu peut laisser des traces pendant la convergence de l’historique (observé avec le DLAA preset M dans Elden Ring). La désactiver doit laisser fonctionner le FG activé séparément. Le démarrage du FG seul a été vérifié dans Elden Ring avec la reconstruction désactivée.

Le plafond interne de 60 FPS rendus d’Elden Ring est distinct de la limite d’affichage/V-Sync. Encore ne retire pas le limiteur du moteur. Le déverrouilleur de FPS testé séparément n’est pas inclus dans Encore.

## Disponibilité et versions des runtimes

Les moteurs dont les runtimes manquent ou sont incompatibles ne sont pas proposés dans le menu. Les anciennes versions compatibles restent prises en charge : NVIDIA 310.9.1 n’est pas refusée simplement parce qu’une version plus récente existe. Un avertissement intégré recommande une mise à jour via RTX Encore Hub puis un redémarrage du jeu. Versions recommandées : NVIDIA **310.9.16**, XeSS-SR **2.0.2.68**, XeFG **1.3.1.78**, XeLL **1.3.2.10**. La génération DLSS ajoutée exige précisément **Streamline 2.14.16 + DLSS-G 310.9.16**. Une mise à jour ne rend pas compatible un jeu ou un GPU non pris en charge.
