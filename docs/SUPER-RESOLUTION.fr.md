# Super Resolution

*[English](SUPER-RESOLUTION.md) · [中文](SUPER-RESOLUTION.zh-CN.md)*

DLSS Super Resolution est le suréchantillonneur du jeu : le jeu calcule l'image à une résolution plus basse et DLSS
reconstruit l'image complète. Lorsque le moteur Game est choisi, RTX Encore le conserve. Il permet de choisir la résolution de rendu et le
preset du modèle, et propose, sur les cartes RTX 30, une option qui rend ce travail moins coûteux pour le GPU.

Pour la résolution de rendu, voir [Menu et overlay](MENU-AND-OVERLAY.fr.md#autres-commandes). Pour le choix du
preset, voir [Réglages](SETTINGS.fr.md#dlsspresets).

## Netteté après reconstruction

Désactivée par défaut. Dans **Image > Super Resolution**, choisissez **Off**, **Game** ou **Custom**. Custom règle une force de 0 à 100 %, 30 % par défaut ; les changements s'appliquent à chaud. Game suit la valeur de netteté envoyée par le jeu et ne fait rien s'il n'en envoie pas. Un réglage invalide désactive l'effet.

La netteté concerne DLSS Super Resolution, DLAA, Ray Reconstruction et le remplacement XeSS, y compris Super Resolution rapide. Si Neural Rendering est actif, la netteté passe après. Elle ne touche pas l'overlay ni le HUD dessiné ensuite. Off et une force nulle n'ajoutent aucune passe de netteté.

Elle se cumule avec la netteté ajoutée ensuite par le jeu. Une force élevée crée des halos et accentue le bruit. Le rendu dépend des traitements ultérieurs du jeu, y compris en HDR. Un format ou un chemin graphique non pris en charge laisse l'image inchangée. Les versions originales de DLSS antérieures à 2.5.1 gardent leur propre netteté ; la passe supplémentaire reste aussi inactive si cette version ne peut pas être identifiée. Vulkan et la qualité d'image en jeu ne sont pas encore validés.

## Intel XeSS et XeFG (expérimental)

RTX Encore 1.2.0 permet de choisir les moteurs de reconstruction et de génération d’images. Laissez **Game** pour conserver le moteur habituel du jeu. Les modes Intel sont expérimentaux, désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d’un moteur ne prouve pas son activation. L’entrée HDR et l’exposition automatique utilisent les réglages communs `srHdr` et `srAutoExposure`, pour le moteur de reconstruction sélectionné. Ils suivent le jeu par défaut (`game`) et demandent un redémarrage. Ils n’activent pas le HDR de l’écran et ne convertissent pas une image SDR en HDR. Un format d’entrée incorrect peut dégrader l’image. Certains modèles DLSS imposent l’exposition automatique ; celle de XeSS a un coût GPU et est déconseillée en LDR. XeSS Vulkan LDR avec exposition manuelle reste non pris en charge. RR et la génération d’images ne changent pas.

[Intel XeSS et XeFG (expérimental)](INTEL-ENGINES.fr.md)

## Fast super resolution (RTX 30)

**Fast super resolution (RTX 30)** réduit le temps GPU consacré à DLSS Super Resolution. Sur les cartes RTX 30
(Ampère), en DirectX 12, quand le jeu utilise DLSS Super Resolution avec le preset **M** ou **L**, RTX Encore
exécute la montée en résolution avec une version plus rapide du modèle, calibrée sur des captures de vrais jeux. La
résolution de rendu, le mode de qualité et le preset ne changent pas.

Elle est **désactivée par défaut**. Activez-la dans le menu : **Image → DLSS Super Resolution → Fast super
resolution (RTX 30)**. Dans le fichier de réglages, la clé est
[`fastSuperResolution`](SETTINGS.fr.md#dlsssuperresolution).
**Relancez le jeu** pour que le changement s'applique, comme pour le preset du modèle.

### Prérequis

- Une carte **RTX 30**. Sur les cartes RTX 20, RTX 40 et RTX 50, l'option reste inactive et ne change rien.
- Un jeu en **DirectX 12**.
- Le fichier DLSS Super Resolution `nvngx_dlss.dll` en version **310.9.x** ; **310.9.16** est recommandée. Voir
  [Compatibilité](COMPATIBILITY.fr.md#versions-de-dlss-super-resolution).
- Le **preset M ou L** en service. Si le jeu en demande un autre, réglez **SR model preset** sur **M** ou **L** dans
  la même section du menu (relancez le jeu après avoir changé un preset, comme pour tous les presets).

### À quoi s'attendre

Mesurée sur un RTX 3070 Ti Laptop, l'option retire environ **16 à 20 %** du temps GPU consacré à chaque image
upscalée, en 1080p comme en 1440p. Dans ces mesures, l'image reste très proche de l'originale et les différences ne
sont pas visibles à l'œil.

Ces chiffres viennent d'un seul système. Ils ne valent pas promesse pour chaque carte, chaque jeu ou chaque scène.

### Limites

- Désactivée par défaut.
- Sans effet avec Ray Reconstruction.
- Le DLAA (résolution de rendu égale à la résolution de sortie) n'est pas couvert : il garde le modèle de NVIDIA.
- Si le profil pilote NVIDIA du jeu force un autre preset (l'override DLSS de l'application NVIDIA), le preset choisi
  dans RTX Encore s'applique quand même avec DLSS 310.9.1 et 310.9.16. Avec d'autres versions le pilote gagne, le jeu
  tourne avec un modèle transformer (presets J et K) et l'option ne change rien ; le journal le dit.
- Si le jeu ou la version du fichier DLSS n'est pas reconnu, RTX Encore laisse la super-résolution d'origine en
  place, sans message d'erreur. Le menu et le jeu se comportent comme si l'option était désactivée.
- L'image n'est pas identique au bit près à celle de NVIDIA : elle en est très proche, sans être la même.
- Mesures faites sur un seul système : les résultats peuvent différer selon la carte, le jeu et le pilote.
