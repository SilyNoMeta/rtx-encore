# Intel XeSS et XeFG (expérimental)

NVIDIA 621.14 : Smooth Motion accepte désormais les fichiers exacts du pilote sur RTX 20 (DirectX 12) et RTX 30 ; les RTX 40/50 conservent le chemin natif du pilote. Les Tensor Cores expérimentaux XeSS/XeFG admettent aussi ce pilote. Les vérifications de compatibilité portent sur les fichiers du pilote, sans installation. La validation en jeu sur 621.14 reste à faire ; cela ne couvre pas les pilotes inconnus.

[English](INTEL-ENGINES.md) · [Français](INTEL-ENGINES.fr.md) · [简体中文](INTEL-ENGINES.zh-CN.md)

Pour RTX Encore 1.5.0. Les versions disponibles figurent sur la [page des publications](https://github.com/SilyNoMeta/rtx-encore/releases).

RTX Encore 1.2.0 permet de choisir les moteurs de reconstruction et de génération d’images. Laissez **Game** pour conserver le moteur habituel du jeu. Les modes Intel sont expérimentaux, désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d’un moteur ne prouve pas son activation.

Utilisez **RTX Encore Hub 1.2.0 ou ultérieur** pour installer ou mettre à jour ensemble le mod et ses dépendances Intel. Le Hub vérifie les fichiers, conserve la licence Intel qui les accompagne et permet de défaire l’opération. Désactiver les téléchargements NVIDIA ne supprime pas les dépendances Intel requises par ce mod. Les fichiers Intel fournis par le jeu restent distincts.

## Tensor Cores : deux commandes indépendantes

Sélectionnez la reconstruction XeSS pour **XeSS Tensor Cores**, et la génération XeFG pour **XeFG Tensor Cores**. Les interrupteurs sont indépendants : activer l’un n’active pas l’autre et ne change pas le moteur. Utilisez le Hub 1.5.0 pour installer les runtimes compatibles et accéder aux nouvelles commandes. Fermez le jeu pour installer la nouvelle DLL ; après ce lancement, les deux interrupteurs et les multiplicateurs fixes XeFG qualifiés se changent à chaud. Changer de moteur FG demande toujours de redémarrer le jeu.

Sur un build de test antérieur dans The Witcher 3, RTX 4090 / 621.01 en 4K, le temps GPU XeSS est passé de 1,243 ms à 0,861 ms (environ −30,7 %), pour environ +2,8 à +3,2 % de FPS source rendus. Ce n’est **pas un gain global de 31 % de FPS**. L’optimisation finale ultérieure a été validée par replays déterministes, dont exposition automatique et dimensions d’entrée fractionnaires ; ce dernier build n’a pas de retest distinct en jeu. Aucun gain général de FPS ou de latence XeFG n’est établi.

| Réglage | Défaut | Effet |
| :--- | :---: | :--- |
| `reconstructionEngine` | `game` | `game` ou `xess` ; remplace le traitement Super Resolution/DLAA compatible par XeSS. Ray Reconstruction garde son moteur original. |
| `frameGenerationEngine` | `game` | `game`, `dlss-fg` ou `xefg`. Jeu conserve le FG natif ; DLSS-FG remplace le FG Intel natif qualifié par le DLSS FG NVIDIA ; XeFG remplace le DLSS FG compatible par Intel XeFG. Redémarrage requis. |
| `xessTensorCores` | `false` | Tensor Cores NVIDIA expérimentaux pour XeSS Super Resolution ; bascule à chaud au prochain calcul XeSS. Qualification initiale : RTX 4090, NVIDIA 621.01, DirectX 12 et runtime Intel fourni par le Hub. Les autres RTX 40, les RTX 30 et les RTX 50, avec NVIDIA 621.01, 621.12 ou 621.14, sont admises avec la même accélération compilée pour leur architecture, sans validation en jeu à ce jour. La prise en charge DirectX 11 et Vulkan est expérimentale et reste à valider en jeu. Le remplacement du DLSS dans un jeu Vulkan utilise le XeSS standard. Une préparation refusée conserve le XeSS classique. |
| `xefgTensorCores` | `false` | Tensor Cores NVIDIA expérimentaux pour XeFG ; facteurs fixes X2–X6 dans la capacité initialisée. Bascule à chaud au renouvellement des commandes. Qualification initiale : RTX 4090, NVIDIA 621.01, DirectX 12 et runtime Intel fourni par le Hub. Les autres RTX 40, les RTX 30 et les RTX 50, avec NVIDIA 621.01, 621.12 ou 621.14, sont admises avec la même accélération compilée pour leur architecture, sans validation en jeu à ce jour. Une préparation refusée conserve le XeFG classique ; une erreur d’exécution impose un redémarrage. |
| `xefgMfg4VariablePacing` | `true` | Espacement expérimental des images XeFG au-delà de X2 ; le désactiver peut rendre les facteurs élevés indisponibles. |
| `xefgSourcePacing` | `true` | XeFG à partir de X3 : observe la cadence source sans limiter les images régulières. Essaie une correction uniquement en cas d'alternance courte/longue persistante et ne la conserve que si la régularité s'améliore avec une faible perte de FPS. Relâche progressivement la contrainte et restaure la cadence efficace si l'alternance revient ; s'applique à chaud. |
| `xefgSourcePacingMode` | `"auto"` | `auto` conserve la correction conditionnelle actuelle et son budget de perte mesurée de 4% des FPS de base. `manual` affiche le budget et les vérifications ci-dessous. Les changements s’appliquent à chaud et relancent la calibration. |
| `xefgSourcePacingLossPercent` | `4` | Budget manuel de perte des FPS de base, 0–30%. Avec les ajustements automatiques, c’est un maximum, pas une perte imposée. Sans eux, la valeur entière fixe une période à partir de la cadence mesurée sans régulation. |
| `xefgSourcePacingManualDetection` | `true` | Manuel uniquement : attendre une cadence durablement irrégulière avant d’intervenir. Désactivé : appliquer après calibration, même sur une cadence régulière. |
| `xefgSourcePacingManualAdjustments` | `true` | Manuel uniquement : conserver les vérifications du gain et le relâchement progressif. Désactivé : tenir le budget entier jusqu’à un changement des réglages ou du contexte. Les erreurs de présentation arrêtent toujours la correction. |
| `xefgUiHandling` | `interpolate` | `interpolate` ou `game` ; le traitement séparé de l’interface du jeu exige des entrées compatibles. |
| `xefgUiAlpha` | `unknown` | `unknown`, `straight` ou `premultiplied` ; laissez unknown si le format de l’interface est inconnu. |

La reconstruction peut passer de DLSS à XeSS en cours de jeu dans les jeux compatibles. En Vulkan, installez des runtimes Intel compatibles avant de lancer le jeu ; leur ajout ou remplacement demande un redémarrage. Redémarrez après un changement de moteur de génération d’images, d’espacement XeFG ou de réglage d’interface. Avec XeFG, changer la limite FPS demande aussi un redémarrage. Une limite FPS ne prouve pas la cadence des images source.

L’entrée HDR et l’exposition automatique utilisent les réglages communs `srHdr` et `srAutoExposure`, pour le moteur de reconstruction sélectionné. Ils suivent le jeu par défaut (`game`) et demandent un redémarrage. Ils n’activent pas le HDR de l’écran et ne convertissent pas une image SDR en HDR. Un format d’entrée incorrect peut dégrader l’image. Certains modèles DLSS imposent l’exposition automatique ; celle de XeSS a un coût GPU et est déconseillée en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge. RR et la génération d’images ne changent pas.

Les jeux sans DLSS peuvent aussi utiliser XeSS : choisissez **XeSS** comme moteur de **Reconstruction pour les jeux sans DLSS** (DirectX 11, DirectX 12 et Vulkan). Les Tensor Cores sont expérimentaux dans les trois API quand le chemin est proposé ; remplacer le DLSS d'un jeu Vulkan conserve le XeSS standard. Voir [Jeux sans DLSS](GAMES-WITHOUT-DLSS.fr.md). Là, `srHdr` et `srAutoExposure` ne s’appliquent pas : RTX Encore sait si l’image affichée est SDR ou HDR.

Depuis la 2.0.0, **XeFG peut aussi générer des images dans les jeux qui n’ont aucune génération d’images** : choisissez **XeFG** comme moteur de génération d’images dans **Génération d’images → Moteur**, puis relancez. X2 à X6 en DirectX 12, Vulkan (SDR et HDR10) et DirectX 11, multiplicateur changé à chaud. Le Hub installe les runtimes Intel avec le mod dans ces jeux. Voir [Jeux sans DLSS](GAMES-WITHOUT-DLSS.fr.md#génération-dimages). Depuis la 2.1.0, XeFG Tensor Cores est aussi expérimental en DirectX 11 et Vulkan ; la 2.2.0 ajoute l'admission expérimentale des RTX 20.

XeFG ne permet pas d’ajouter la génération d’images à tous les jeux. Les facteurs élevés dépendent du runtime Intel accepté et de toute la configuration du jeu ; X5/X6 restent expérimentaux et cette bêta ne qualifie pas X6 en jeu. Un multiplicateur demandé, une dépendance installée ou un compteur d’images générées ne mesure ni le contenu réellement affiché ni la latence physique. Activer NR ou Smooth Motion pendant un essai XeFG ne rend pas cette combinaison qualifiée.

Des essais Vulkan locaux limités dans No Man’s Sky et DOOM: The Dark Ages étayent la route XeFG X2 ; l’essai DOOM a été accepté par le mainteneur en plein écran fenêtré, NR coupé. Cela ne qualifie ni le plein écran exclusif, ni DirectX 11, ni XeFG avec NR, ni tous les multiplicateurs élevés ou les autres jeux. Le menu et l’overlay peuvent être indisponibles sur une route Intel ; utilisez le Hub pour régler le mod et relire les constats de session.

[Référence des réglages](SETTINGS.fr.md) · [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)

Pour les jeux à FG Intel natif comme Mirage, installez les runtimes compatibles avec le Hub, choisissez **DLSS-FG** dans Encore, redémarrez, puis activez le **FG Intel** du jeu. **Fixed** permet les multiplicateurs qualifiés ; **Game** utilise X2 sur cet adaptateur et Dynamic est indisponible. Le FSR natif et les jeux déjà en DLSS FG conservent leur moteur. Le mainteneur a validé le build de test corrigé dans Mirage ; les binaires finaux n’ont pas de retest distinct en jeu.
