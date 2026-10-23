# Neural Rendering

*[English](NEURAL-RENDERING.md) · [中文](NEURAL-RENDERING.zh-CN.md)*

DLSS Neural Rendering (NR) enrichit l'image produite par DLSS Super Resolution ou DLAA. RTX Encore le fait tourner
sur les cartes RTX 20, RTX 30 et RTX 40, dans les jeux DirectX 11, DirectX 12 et Vulkan.

> [!NOTE]
> Neural Rendering est expérimental et désactivé par défaut. Il coûte beaucoup de temps GPU : voir [Coût](#coût).

À chaque fonction son rôle : **DLSS Super Resolution** reconstruit l'image du jeu, **Neural Rendering** en change
le rendu, **la génération d'images** ajoute des images intermédiaires. Activer NR n'active pas la génération
d'images.

## Ce que RTX Encore lui apporte

| | |
| :--- | :--- |
| **Trois générations de cartes** | RTX 40, RTX 30 et, à titre expérimental, RTX 20, dans les jeux DirectX 11, DirectX 12 et Vulkan. |
| **Traitement plus rapide, à image identique** | Des optimisations activées par défaut pour le moteur NVIDIA sur RTX 20 et RTX 30, avec une sortie identique dans les tests locaux de référence. Performances mesurées sur RTX 30 ; la validation sur une RTX 20 physique reste à faire. Une partie est propre à 310.8.0 : avec 310.9.16, le réseau A prend un peu plus de temps qu'avec 310.8.0. |
| **Précision Fast** | Moteur NVIDIA : environ 17 % de temps NR en moins mesurés sur RTX 30, avec de petites différences d'image. |
| **Résolution NR indépendante** | NR peut travailler sous la résolution de l'écran, tandis que DLSS conserve sa résolution de rendu et les détails reconstruits. Performance et Ultra Performance réduisent le travail NR sans imposer un réglage DLSS inférieur. |
| **Conçu pour cohabiter avec la génération d'images** | NR s'exécute une fois par image calculée, jamais sur les images générées. |
| **Plusieurs passes, un style pour chacune** | Une à quatre passes, chacune avec son rendu. |
| **Jeux HDR** | Pris en charge, avec un réglage de luminosité. |
| **Préparation en arrière-plan** | Pendant la préparation de NR ou quand des données nécessaires du jeu manquent, l'image reste telle que DLSS l'a produite. |

Les options qui consomment davantage de VRAM sont désactivées par défaut.

## Prérequis

- Le fichier NR de NVIDIA, **`nvngx_dlssnr.dll` version 310.8.0 ou 310.9.16**. Il n'est pas inclus dans RTX Encore :
  fournissez-le vous-même et placez-le à côté du fichier du mod, normalement dans le dossier de l'exécutable du jeu.
  Toute autre version est refusée. [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) télécharge ce fichier, le vérifie et le place là pour vous.
- Un jeu avec **DLSS Super Resolution ou DLAA activé**. NR s'applique à son résultat.
  Avec **Ray Reconstruction**, NR s'applique au résultat de RR, après son débruitage : jamais à l'image brute du
  ray tracing. Le jeu doit fournir l'entrée de profondeur dont NR a besoin ; sinon l'image reste telle que DLSS l'a
  produite et le menu l'indique.
- Avec la bêta 1.2, les entrées de profondeur linéaire compatibles sont également prises en charge après RR sous Direct3D 12. Une profondeur invalide ou non prise en charge laisse NR inactif ; Direct3D 11 et Vulkan ne prennent toujours pas en charge ce cas. Un essai local dans 007 First Light en path tracing confirme que NR fonctionne, sans mesure de qualité d’image ni de coût.
- Une carte RTX 40 ou RTX 30. RTX 20 est expérimental.

## L'activer

1. Placez `nvngx_dlssnr.dll` à côté du fichier du mod.
2. Activez DLSS ou DLAA dans le jeu.
3. Ouvrez le menu (**Inser**), onglet **Image**, et activez **Neural Rendering**.

**Ctrl + Alt + N** active ou désactive NR en cours de jeu, pour comparer l'image et la fréquence d'images avec et
sans. Le choix est enregistré comme celui de l'interrupteur du menu. Le raccourci se change ou se désactive dans le
menu (onglet **System**).

NR se prépare en arrière-plan : les premières images peuvent rester telles que DLSS les a produites. La pastille
**Neural Rendering** en haut du menu indique si NR tourne réellement, et pourquoi quand ce n'est pas le cas.

## Réglages principaux

| Réglage | Défaut | Ce qu'il fait |
| :--- | :---: | :--- |
| **Resolution** | Render | La taille à laquelle NR travaille : la taille de rendu DLSS, la taille de sortie complète, ou Quality (67 %), Balanced (58 %), Performance (50 %), Ultra Performance (33 %) de la sortie, ou une échelle personnalisée. **C'est le principal levier de performance** : plus petit, plus rapide. |
| **Strength** | 100 % | L'intensité de l'effet, de 0 à 200 %. |
| **Style** | Default | Le rendu de l'image : Default, Natural ou Cinematic. |
| **Passes** | 1 | NR peut être appliqué de nouveau à son propre résultat, jusqu'à quatre fois. Chaque passe coûte du temps GPU et de la VRAM, et davantage de passes ne garantit pas une meilleure image. |
| **Precision** | Exact | Moteur NVIDIA, RTX 20 et RTX 30 : *Fast* réduit le temps NR, avec environ 17 % mesurés sur RTX 30 et de petites différences d'image. |
| **NR model preset** | B | Avec `nvngx_dlssnr.dll` 310.9.16 : **B** est un réseau plus petit et bien plus rapide, **A** le plus grand, celui que 310.8.0 exécute, **DLL default** laisse le fichier choisir (B). Avec 310.8.0, chaque choix exécute son unique réseau. Il se trouve à côté des presets de modèle SR et RR (onglet **Image**, Upscaling) et s'applique en direct. |

Dans un test local en 1080p sur une RTX 3070 Ti Laptop, le réseau B a pris environ 15 ms de temps NR et le réseau A
environ 27 ms. B donne une image différente de A. La prise en charge de 310.9.16 est expérimentale et peu testée.

### Réglages de performance sur RTX 30

Le groupe **Image → Neural Rendering → Performance** donne accès aux travaux de RTX Encore pour réduire le coût
NR. Ces options ont des compromis différents ; leurs gains ne sont pas des pourcentages à additionner.

| Réglage | Moteur | Défaut | Gain et compromis |
| :--- | :--- | :---: | :--- |
| **Precision : Fast** | NVIDIA sur RTX 30 | Exact | Traitement plus rapide, avec une image légèrement différente. Les 17 % mesurés ci-dessus concernent NVIDIA, uniquement. Relancez le jeu pour appliquer. |
| **Avoid NR padding** | NVIDIA | Désactivé | Autorise une taille NR voisine plus petite, au plus 2 % de moins par axe. Peut changer l'image NR ; ne change pas la taille de rendu DLSS. S'applique en direct. |
| **NR frames in flight** | NVIDIA, DirectX 12 avec Reflex | 0 | Limite les images NR en attente à 1 ou 2 ; la génération d'images active utilise au moins 2. S'applique en direct ; son effet sur les performances en jeu reste à vérifier. |

### Passes et styles

Avec plus d'une passe, chaque passe peut avoir son style : **Global** (suit le réglage **Style**), **Default**,
**Natural** ou **Cinematic**. Par exemple, Style Cinematic avec la passe un sur Natural et la passe deux sur Global
donne Natural, puis Cinematic. **Tone after pass one** (0 par défaut) évite de réappliquer la correction de ton à
chaque passe.

Commencez avec une passe et gardez-la comme référence pour comparer. Plusieurs passes n'ont pas encore été validées
en jeu.

### Réglages fins

Le ton local, la structure locale, un masque des personnages avec son propre réglage de structure de la peau, et la
luminosité donnée à NR dans les jeux HDR sont dans les groupes qui s'ouvrent sous les réglages principaux. Chacun a sa description dans le menu.

## Moteurs

OpenNR est retiré en 1.3.0. Neural Rendering utilise le runtime NVIDIA. Une configuration OpenNR migre avec NR désactivé ; réactivez-le pour utiliser NVIDIA NR.

## Coût

NR ajoute du travail GPU à chaque image calculée. Mesuré dans le benchmark intégré de Black Myth: Wukong sur un
RTX 3070 Ti Laptop (2560×1440, DLSS à 58 %, génération d'images X3) :

| Configuration | FPS moyens | 5 % les plus bas |
| :--- | ---: | ---: |
| NR désactivé | 146 | 127 |
| NR activé, Precision Exact | 76 | 71 |
| NR activé, Precision Fast | 82 | 76 |
| NR activé, Precision Fast, Resolution Performance | 90 | 83 |

Il s'agit des **FPS affichés avec la génération d'images X3**, avec le moteur NR NVIDIA. Dans ce test, passer des
réglages par défaut à Precision Fast et Resolution Performance augmente les FPS affichés de 18 % (76 à 90 FPS).
Cela ne mesure pas une hausse de 17 % des FPS du jeu : ce chiffre concerne le temps de traitement NR. Avec la
génération d'images, NR s'applique aux images que le jeu calcule, jamais aux images générées.

## Quand quelque chose ne fonctionne pas

| Symptôme | Première vérification |
| :--- | :--- |
| L'interrupteur ne peut pas être activé | `nvngx_dlssnr.dll` manque à côté du fichier du mod, ou n'est ni en version 310.8.0 ni en version 310.9.16. |
| La pastille dit attendre DLSS | Activez DLSS ou DLAA dans le jeu et chargez une scène 3D. |
| Les premières images n'ont pas NR | NR se prépare en arrière-plan ; c'est normal. |
| L'image est dégradée, floue ou délavée | Vérifiez qu'il n'y a qu'une seule copie de RTX Encore dans le jeu : deux copies sous deux noms abîment l'image. |
| L'image reste telle que DLSS l'a produite | Lisez la raison affichée dans le menu : quand le jeu ne fournit pas ce dont NR a besoin, l'image est laissée intacte. |
| Les réglages reviennent aussitôt en arrière | Le fichier de réglages n'a pas pu être enregistré : vérifiez qu'il n'est ni en lecture seule ni verrouillé par un éditeur. |
| La fréquence d'images chute | NR coûte cher. Réduisez **Resolution**, et comparez dans la même scène. |

Quand vous signalez un problème, indiquez le jeu et son API graphique, le moteur choisi, et joignez le rapport du
menu (onglet **System**, **Copy**).

## Réglages par passe (depuis la 1.2.0)

Chacune des quatre passes NR possibles peut remplacer l’intensité, le ton local, la structure locale, la structure de peau et le masque de personnage, ou suivre la valeur globale. Les valeurs numériques vont de 0 à 2 ; le masque prend `inherit`, `true` ou `false`. Après la première passe, le ton hérité vient de `nrLaterPassLocalTone`. Une intensité à zéro laisse la passe tourner et ne supprime pas son coût GPU/VRAM.
