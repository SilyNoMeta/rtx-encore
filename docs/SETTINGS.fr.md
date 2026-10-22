# Fichier de réglages

*[English](SETTINGS.md) · [中文](SETTINGS.zh-CN.md)*

Tous les réglages se changent dans le [menu](MENU-AND-OVERLAY.fr.md) (**Inser**), ce qui est le plus simple. Cette
page s'adresse à ceux qui préfèrent modifier le fichier.

## Le fichier

- Nom : `rtx-encore.jsonc`, à côté du fichier du mod. Il est créé au premier lancement avec les valeurs par défaut.
- Format : JSON avec des commentaires `//`. Chaque clé porte un commentaire qui dit ce qu'elle fait ; certains
  éditeurs signalent les commentaires, le mod les lit sans problème.
- Les réglages sont regroupés en sections. La plupart s'appliquent en direct ; le commentaire indique quand il faut
  relancer le jeu.
- Le raccourci et la disposition du menu sont enregistrés dans le même fichier, dans sa section `menuState`.
- Un fichier de réglages laissé par une version antérieure est repris avec ses valeurs : voir
  [Installation](INSTALLATION.fr.md#mise-à-niveau-depuis-un-ancien-nom).

Aucune clé ne contient de chemin ni de commande : un fichier de réglages peut être partagé tel quel.

## Clés principales

### `renderingEngines` / `xefg`

| Réglage | Défaut | Effet |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` ou `xess` ; remplace le traitement Super Resolution/DLAA compatible par XeSS. Ray Reconstruction garde son moteur original. |
| `frameGenerationEngine` | `game` | `game` ou `xefg` ; utilise XeFG sur les routes DirectX 12 ou Vulkan prises en charge. DirectX 11 reste expérimental et non qualifié en jeu ici. |
| `xefgMfg4VariablePacing` | `true` | Espacement expérimental des images XeFG au-delà de X2 ; le désactiver peut rendre les facteurs élevés indisponibles. |
| `xefgUiHandling` | `interpolate` | `interpolate` ou `game` ; le traitement séparé de l’interface du jeu exige des entrées compatibles. |
| `xefgUiAlpha` | `unknown` | `unknown`, `straight` ou `premultiplied` ; laissez unknown si le format de l’interface est inconnu. |

La reconstruction peut passer de DLSS à XeSS en cours de jeu dans les jeux compatibles. En Vulkan, installez des runtimes Intel compatibles avant de lancer le jeu ; leur ajout ou remplacement demande un redémarrage. Redémarrez après un changement de moteur de génération d’images, d’espacement XeFG ou de réglage d’interface. Avec XeFG, changer la limite FPS demande aussi un redémarrage. Une limite FPS ne prouve pas la cadence des images source.

[Intel XeSS et XeFG (expérimental)](INTEL-ENGINES.fr.md)

### `frameGeneration`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `mode` | `"game"` | `"game"` (le jeu ou NVIDIA Profile Inspector décide), `"fixed"`, `"dynamic"` |
| `multiplier` | `4` | `2` à `6`, utilisé en mode fixe. `5` et `6` sont expérimentaux. |
| `dynamicTargetFrameRate` | `0` | FPS visés en mode dynamique. `0` suit la fréquence de rafraîchissement de l'écran qui affiche le jeu. |
| `dynamicExperimental56` | `false` | Laisse le mode dynamique monter à X5 et X6. |
| `flipMeteringBypass` | `false` | Lisser X3 / X4 : Streamline espace lui-même les images générées au lieu du pilote NVIDIA. Relancez le jeu pour appliquer. |

La compatibilité des runtimes NVIDIA n’exige plus que le fichier figure dans une liste prédéfinie. Une ancienne entrée `providerPatchPolicy` est ignorée. Les contrôles Intel restent inchangés. La compatibilité dépend toujours des fonctions disponibles dans le runtime et le jeu.

### `dlssSuperResolution`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `dlssRenderScale` | `"game"` | `"game"`, `"dlaa"`, `"quality"`, `"balanced"`, `"performance"`, `"ultra-performance"`, `"custom"` |
| `dlssCustomScale` | `67` | Pourcentage, `50` à `100`, avec `"custom"` |
| `fastSuperResolution` | `false` | RTX 30, DirectX 12 : DLSS Super Resolution plus rapide avec le preset M ou L, environ 16 à 20 % de temps GPU en moins par image upscalée sur le système mesuré, image très proche de l'originale. Inactive sur les autres cartes. Relancez pour appliquer. Voir [Super Resolution](SUPER-RESOLUTION.fr.md#fast-super-resolution-rtx-30). |

### `superResolutionInputs`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `srHdr` | `"game"` | `"game"`, `"on"`, `"off"` |
| `srAutoExposure` | `"game"` | `"game"`, `"on"`, `"off"` |

Ces réglages SR/DLAA communs suivent le moteur de reconstruction sélectionné. Redémarrage requis. Game conserve les indications du jeu. HDR on attend une entrée HDR linéaire, off une entrée LDR. Aucune conversion des couleurs ni activation du HDR de l’écran. Un forçage incorrect peut dégrader l’image. RR et la génération d’images restent inchangés.

L’exposition automatique peut aider le ghosting lié à l’exposition sans corriger tout le ghosting. DLSS peut l’imposer (preset L). XeSS ajoute un coût GPU et la déconseille en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge. Un mode XeSS explicite refusé à l’initialisation conserve le fournisseur original sans lui appliquer le forçage. L’amélioration visuelle des modes forcés reste à valider.

Les anciennes clés DLSS/XeSS sont migrées selon le moteur de reconstruction sélectionné ; les clés communes ont priorité si elles existent.

### `dlssPresets`

Ces choix de modèles sont indépendants et demandent de **relancer le jeu**. SR/RR se trouvent dans
**Image → DLSS Super Resolution**, FG dans **Frames → Frame generation**. Ils ne changent ni la résolution de rendu,
ni le multiplicateur d'images, ni le preset NR.

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `dlssSrPreset` | `"game"` | `"game"`, `"default"`, `"a"`–`"f"` (anciens), `"j"`, `"k"`, `"l"`, `"m"` |
| `dlssRrPreset` | `"game"` | `"game"`, `"default"`, `"a"`–`"c"` (anciens), `"d"`, `"e"` |
| `dlssFgPreset` | `"game"` | `"game"`, `"a"`, `"b"`, `"c"` |
| `dlssFgLite` | `"game"` | Diagnostic uniquement, JSON : `"game"`, `"off"`, `"on"` |
| `dlssNrPreset` | `"b"` | Réseau de Neural Rendering avec `nvngx_dlssnr.dll` 310.9.16 : `"b"` (plus petit, bien plus rapide), `"a"` (le réseau de 310.8.0) ou `"auto"` (le choix du fichier, B). Un seul réseau avec 310.8.0. S'applique en direct. Le `nrPreset` des anciens fichiers est lu sous ce nom, avec sa valeur ; ses valeurs `0` à `3` sont encore lues. |

Pour les clés de preset, `"game"` conserve le choix du jeu. Les choix SR/RR s'appliquent à tous les modes de qualité, DLAA compris ;
`"default"` demande explicitement le choix par défaut de la DLL DLSS. Les modèles disponibles dépendent
de la DLL installée ; les réglages NVIDIA ou un modèle de repli peuvent prendre le dessus.
Choisir un preset RR n'active pas RR.
Avec DLSS Super Resolution **310.9.1.0** et **310.9.16.0**, le preset SR choisi dans Encore s'applique même si
le profil du pilote NVIDIA en impose un autre (310.9.1.0, ainsi que la 310.9.16.0 du SDK Streamline et du GitHub DLSS
de NVIDIA, n'ont pas les presets A à D).

Le choix du preset FG prend en charge les **binaires release NVIDIA DLSS-G** vérifiés individuellement ci-dessous :

- **310.6.0.0, 310.7.0.0, 310.7.128.0, 310.7.129.0**
- **310.8.0.0, 310.9.0.0, 310.9.1.0**
- **310.9.16.0**, le seul qui accepte le preset C

Le preset C est le mode performance plus rapide de NVIDIA pour la génération d’images, avec de légers compromis
sur la qualité d’image. Avec un autre binaire, un `"c"` enregistré est refusé et le choix du jeu est conservé.

**Le preset FG suffit : A/B utilisent Normal ; C active automatiquement Lite.** Fast frame generation reste
indépendant et peut rester activé avec chaque preset. Relancez le jeu après un changement de preset.

`dlssFgLite` est un **forçage réservé au diagnostic, uniquement dans le JSON**, absent des réglages du menu.
Laissez `"game"` pour suivre le preset choisi ou le jeu/pilote. Uniquement pour le diagnostic avec le fichier
310.9.16.0 qualifié individuellement, `"off"` force Normal et `"on"` force Lite, en priorité sur le preset.
Il faut relancer le jeu ; les autres binaires refusent ce forçage. Lite conserve la politique
d'image native de NVIDIA : **Anti-tearing / anti-ghosting** et **Protection tuning** n'ont aucun effet dans ce mode, et la recomposition de l'interface est indisponible.
Fast frame generation utilise les optimisations adaptées au mode actif. Les ajouts pour 310.9.16 restent à valider
en jeu ; aucun gain de performance n'est établi pour Lite.

Cette liste concerne le forçage des presets ; la compatibilité générale de la génération d’images est plus large.
Le numéro de version seul ne garantit pas l’acceptation, et les variantes de développement sont exclues.
Les autres binaires conservent leur comportement et le journal indique le refus. A utilise la FG classique ;
B nécessite des entrées HUD-less/UI compatibles pour la recomposition de l'interface.
Le mod n'écrit pas dans les profils du pilote NVIDIA. Les détails de session indiquent les choix enregistrés,
ceux du lancement et si la demande FG a été acceptée ; ils ne prouvent pas quel modèle a été exécuté.
Laissez `"game"` pour conserver le comportement actuel.

### `neuralRendering`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `nrEnabled` | `false` | Active Neural Rendering. |
| `nrEngine` | `"nvidia"` | `"nvidia"` ou `"opendlss"` (très expérimental). Relancez pour appliquer. |
| `nrPasses` | `1` | `1` à `4` |
| `nrLaterPassLocalTone` | `0` | Ton appliqué après la première passe, `0` à `2` |
| `nrResolution` | `"render"` | `"render"`, `"output"`, `"quality"`, `"balanced"`, `"performance"`, `"ultra-performance"`, `"custom"` |
| `nrResolutionScale` | `67` | Pourcentage de la taille de sortie, `33` à `100`, avec `"custom"` |
| `nrIntensity` | `1` | Intensité, `0` à `2` |
| `nrStyle` | `0` | `0` par défaut, `1` naturel, `2` cinématique |
| `nrPass1Style` … `nrPass4Style` | `"inherit"` | `"inherit"` (suit `nrStyle`), `"default"`, `"natural"`, `"cinematic"` |
| `nrLocalTone`, `nrLocalStructure` | `1` | `0` à `2` |
| `nrAutoMask` | `false` | Masque des personnages ; nécessaire pour `nrSkinStructure` |
| `nrSkinStructure` | `1` | `0` à `2` |
| `nrHdrExposure` | `1` | Jeux HDR : luminosité donnée à NR |
| `nrPrecision` | `"exact"` | `"exact"` ou `"fast"` (RTX 20 et RTX 30). Relancez pour appliquer. |

#### Options de performance NR

Ces réglages se trouvent aussi dans **Image → Neural Rendering → Performance**. Voir
[Neural Rendering](NEURAL-RENDERING.fr.md#réglages-de-performance-sur-rtx-30) pour les mesures et les compromis
d'image. Les options propres à un moteur ne s'appliquent qu'au moteur indiqué ci-dessous ; changer de moteur ou
d'option lue au démarrage demande de relancer le jeu.

| Clé | Défaut | Gain et compromis |
| :--- | :---: | :--- |
| `nrPaddingAware` | `false` | Autorise une taille NR voisine plus petite, au plus 2 % de moins par axe, sans changer la taille de rendu DLSS. Peut changer l'image. S'applique en direct. |
| `nrMaxInFlight` | `0` | NVIDIA, DirectX 12 avec Reflex : `0` sans limite, `1` ou `2` images NR en attente ; la génération d'images active utilise au moins `2`. S'applique en direct ; effet sur les performances en jeu à vérifier. |

### `openExperimental`

Les clés de performance Open sont dans cette section distincte. Choisissez d'abord le moteur et le backend adapté
à votre carte depuis le menu ; ces clés n'activent pas Open à elles seules.

| Clé | Défaut | Gain et compromis |
| :--- | :---: | :--- |
| `nrOpenFast` | `false` | Open sur RTX 30 : précision Fast, temps NR plus court avec de petites différences d'image et sans VRAM supplémentaire. Redémarrage requis. |
| `nrOpenVramForSpeed` | `false` | Open sur RTX 30 : temps NR plus court, à image identique, contre environ 160 Mo de VRAM en plus en 1440p DLSS Balanced. Redémarrage requis. |
| `nrOpenFastProjection` | `true` | Open sur RTX 30 : traitement plus rapide, avec de petites différences d'image. Redémarrage requis. |
| `nrOpenUltraFast` | `false` | Open, une passe NR : **Ultra-fast mode**, environ moitié moins de temps NR dans la comparaison documentée. Couche NR plus ancienne, défauts possibles en mouvement, quelques Mo de VRAM en plus. Redémarrage requis. |
| `nrOpenUltraFastGhostTolerance` | `0.03` | `0` à `0.3` ; plus bas : moins de fantômes, davantage de surfaces en attente temporaire de NR. S'applique en direct. |
| `nrOpenUltraFastFillTolerance` | `0.03` | `0` à `0.3` ; règle le remplissage des surfaces qui apparaissent avec le NR de surfaces semblables. `0` désactive le remplissage. S'applique en direct. |

`nrResolution: "ultra-performance"` règle NR à 33 % de la largeur et de la hauteur de sortie. Ce choix est
indépendant de `nrOpenUltraFast` et de `dlssRenderScale` ; ces réglages ne s'activent pas mutuellement.

### `overlay`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `showOverlay` | `false` | Affiche l'overlay. |
| `overlayPosition` | `"top-left"` | `"top-left"`, `"top-right"`, `"bottom-left"`, `"bottom-right"` |
| `overlayFontSize` | `14` | Taille du texte en pixels, `10` à `32` |
| `overlayShowNr`, `overlayShowFramePacing`, `overlayShowGpu`, `overlayShowVram`, `overlayShowVersions` | `false` | Lignes optionnelles |

### `hudUi`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `autoUiRecomposition` | `true` | Traitement séparé de l'interface dès que le jeu fournit ce qu'il faut |
| `uiAssist` | `true` | DirectX 12 : construit la scène sans interface et la couche d'interface quand le jeu ne les fournit pas |
| `forceUiRecomposition` | `false` | Demande le traitement séparé même quand le jeu ne le demande pas |

### `nativeMenuFeatures`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `reflexFrameLimit` | `0` | `0` restaure les réglages Reflex du jeu ; `1` à `1000` demande un plafond FPS. Le comptage de FG dépend du jeu et du pilote |
| `vsyncOff` | `false` | Demande aux jeux DirectX de présenter sans V-Sync |
| `hairEnabled` | `true` | Cheveux en ray tracing compatibles, dans les jeux qui en ont. Relancez pour appliquer. |

### `compatibility`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `disableInGameMenu` | `false` | `true` n'attache pas le menu RTX Encore, pour les jeux qui le refusent (Star Wars Outlaws). La génération d'images continue de fonctionner ; changez les réglages dans ce fichier ou avec RTX Encore Hub. Redémarrez le jeu pour l'appliquer. |

### `diagnostics`

| Clé | Défaut | Valeurs |
| :--- | :---: | :--- |
| `logPerformance` | `false` | Écrit les temps d’image en CSV et les températures, la puissance et les raisons de limitation GPU dans le journal de session de `rtx-encore-logs` |
| `logFilesKept` | `3` | Journaux normaux conservés, plus autant de journaux d’exception, `1` à `100` |
| `logDxgiWrappers` | `false` | Enregistre chaque appel du jeu sur la swapchain du menu, avec le module appelant, dans les logs de session et d'exception. Pour les plantages après l'attache du menu. Redémarrez pour appliquer |
| `crashDetails` | `false` | Les logs d'exception ajoutent la mémoire des registres et la pile, et un plantage dans le jeu ou dans RTX Encore laisse un instantané mémoire `.dmp` (souvent des dizaines de Mo) dans `rtx-encore-logs`. Pour les rapports de plantage ; désactivez-le ensuite. Redémarrez pour appliquer |

### `keyboardShortcuts`

Chaque clé `hotkey…` prend une ou plusieurs combinaisons séparées par des virgules, par exemple
`"hotkeyFixed4": "Ctrl+Alt+4, Ctrl+Alt+Num4"`. Une valeur vide `""` désactive l'action.
`"disableKeybinds": true` désactive tous les raccourcis.

## Autres clés

Le fichier contient plus de clés que cette page n'en liste : Smooth Motion, les interrupteurs de qualité d'image et
de compatibilité, et les options expérimentales du moteur Open. Chacune a son commentaire dans le fichier et une
commande libellée dans le menu, qui est l'endroit recommandé pour les changer.

### `neuralRendering.passTuning`

Chacune des quatre passes NR possibles peut remplacer l’intensité, le ton local, la structure locale, la structure de peau et le masque de personnage, ou suivre la valeur globale. Les valeurs numériques vont de 0 à 2 ; le masque prend `inherit`, `true` ou `false`. Après la première passe, le ton hérité vient de `nrLaterPassLocalTone`. Une intensité à zéro laisse la passe tourner et ne supprime pas son coût GPU/VRAM.

| Clés | Défaut | Valeurs |
| :--- | :---: | :--- |
| `nrPass1Intensity` … `nrPass4Intensity` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalStructure` … `nrPass4LocalStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1LocalTone` … `nrPass4LocalTone` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1SkinStructure` … `nrPass4SkinStructure` | `"inherit"` | `"inherit"`, 0–2 |
| `nrPass1AutoMask` … `nrPass4AutoMask` | `"inherit"` | `"inherit"`, `true`, `false` |
