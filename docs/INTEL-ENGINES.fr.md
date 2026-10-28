# Intel XeSS et XeFG (expérimental)

[English](INTEL-ENGINES.md) · [Français](INTEL-ENGINES.fr.md) · [简体中文](INTEL-ENGINES.zh-CN.md)

Pour RTX Encore 1.4.6. Les versions disponibles figurent sur la [page des publications](https://github.com/SilyNoMeta/rtx-encore/releases).

RTX Encore 1.2.0 permet de choisir les moteurs de reconstruction et de génération d’images. Laissez **Game** pour conserver le moteur habituel du jeu. Les modes Intel sont expérimentaux, désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d’un moteur ne prouve pas son activation.

Utilisez **RTX Encore Hub 1.2.0 ou ultérieur** pour installer ou mettre à jour ensemble le mod et ses dépendances Intel. Le Hub vérifie les fichiers, conserve la licence Intel qui les accompagne et permet de défaire l’opération. Désactiver les téléchargements NVIDIA ne supprime pas les dépendances Intel requises par ce mod. Les fichiers Intel fournis par le jeu restent distincts.

| Réglage | Défaut | Effet |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` ou `xess` ; remplace le traitement Super Resolution/DLAA compatible par XeSS. Ray Reconstruction garde son moteur original. |
| `frameGenerationEngine` | `game` | `game`, `dlss-fg` ou `xefg`. Jeu conserve le FG natif ; DLSS-FG remplace le FG Intel natif qualifié par le DLSS FG NVIDIA ; XeFG remplace le DLSS FG compatible par Intel XeFG. Redémarrage requis. |
| `xefgMfg4VariablePacing` | `true` | Espacement expérimental des images XeFG au-delà de X2 ; le désactiver peut rendre les facteurs élevés indisponibles. |
| `xefgUiHandling` | `interpolate` | `interpolate` ou `game` ; le traitement séparé de l’interface du jeu exige des entrées compatibles. |
| `xefgUiAlpha` | `unknown` | `unknown`, `straight` ou `premultiplied` ; laissez unknown si le format de l’interface est inconnu. |

La reconstruction peut passer de DLSS à XeSS en cours de jeu dans les jeux compatibles. En Vulkan, installez des runtimes Intel compatibles avant de lancer le jeu ; leur ajout ou remplacement demande un redémarrage. Redémarrez après un changement de moteur de génération d’images, d’espacement XeFG ou de réglage d’interface. Avec XeFG, changer la limite FPS demande aussi un redémarrage. Une limite FPS ne prouve pas la cadence des images source.

L’entrée HDR et l’exposition automatique utilisent les réglages communs `srHdr` et `srAutoExposure`, pour le moteur de reconstruction sélectionné. Ils suivent le jeu par défaut (`game`) et demandent un redémarrage. Ils n’activent pas le HDR de l’écran et ne convertissent pas une image SDR en HDR. Un format d’entrée incorrect peut dégrader l’image. Certains modèles DLSS imposent l’exposition automatique ; celle de XeSS a un coût GPU et est déconseillée en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge. RR et la génération d’images ne changent pas.

XeFG ne permet pas d’ajouter la génération d’images à tous les jeux. Les facteurs élevés dépendent du runtime Intel accepté et de toute la configuration du jeu ; X5/X6 restent expérimentaux et cette bêta ne qualifie pas X6 en jeu. Un multiplicateur demandé, une dépendance installée ou un compteur d’images générées ne mesure ni le contenu réellement affiché ni la latence physique. Activer NR ou Smooth Motion pendant un essai XeFG ne rend pas cette combinaison qualifiée.

Des essais Vulkan locaux limités dans No Man’s Sky et DOOM: The Dark Ages étayent la route XeFG X2 ; l’essai DOOM a été accepté par le mainteneur en plein écran fenêtré, NR coupé. Cela ne qualifie ni le plein écran exclusif, ni DirectX 11, ni XeFG avec NR, ni tous les multiplicateurs élevés ou les autres jeux. Le menu et l’overlay peuvent être indisponibles sur une route Intel ; utilisez le Hub pour régler le mod et relire les constats de session.

[Référence des réglages](SETTINGS.fr.md) · [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)

Pour les jeux à FG Intel natif comme Mirage, installez les runtimes compatibles avec le Hub, choisissez **DLSS-FG** dans Encore, redémarrez, puis activez le **FG Intel** du jeu. **Fixed** permet les multiplicateurs qualifiés ; **Game** utilise X2 sur cet adaptateur et Dynamic est indisponible. Le FSR natif et les jeux déjà en DLSS FG conservent leur moteur. Le mainteneur a validé le build de test corrigé dans Mirage ; les binaires finaux n’ont pas de retest distinct en jeu.
