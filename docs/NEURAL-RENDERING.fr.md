# Neural Rendering

*[English](NEURAL-RENDERING.md) · [中文](NEURAL-RENDERING.zh-CN.md)*

DLSS Neural Rendering (NR) enrichit l'image produite par DLSS Super Resolution ou DLAA. RTX Encore le fait tourner
sur les cartes RTX 20, RTX 30 et RTX 40, dans les jeux DirectX 11, DirectX 12 et Vulkan.
Vulkan accepte aussi les RTX 50 à titre expérimental. La validation Vulkan sur des RTX 20, RTX 40 et RTX 50
physiques avec une build officielle reste à faire ; accepter la carte ne prouve ni la qualité d'image ni les performances.

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
| **Génération d’images** | Dans les configurations Post-SR et Pre-SR compatibles, NR travaille sur les images calculées par le jeu. Voir les limites du [mode Image finale](#mode-present). |
| **Plusieurs passes, un style pour chacune** | Une à quatre passes, chacune avec son rendu. |
| **Jeux HDR** | Pris en charge, avec un réglage de luminosité. |
| **Préparation en arrière-plan** | Pendant la préparation de NR ou quand des données nécessaires du jeu manquent, l'image reste telle que DLSS l'a produite. |

Les options qui consomment davantage de VRAM sont désactivées par défaut.

## Pre-SR expérimental et comparaison A/B

**Post-SR** reste le défaut. **Pre-SR** applique NR à l’entrée avant DLSS Super
Resolution, à la taille de rendu, en une à quatre passes avec profil de modèle
et apparence indépendants. Le ruban indique l’ordre demandé ; vérifier le mode
effectif. Pre-SR exige DLSS SR et une entrée uniforme : RR, DLAA, fovéation,
taille évitant le padding et mouvements jitterés sont indisponibles. Une entrée
incompatible conserve le SR original. La qualification GPU et en jeu reste à faire.

Dans **Atelier image et performances**, gardez une scène et une caméra
reproductibles. Mesurez séparément Post-SR, Pre-SR ou Image finale, puis choisissez
les deux modes à comparer. Chaque série exclut 60 images de chauffe et recueille
240 images source terminées. Capturez chaque mode, puis activez **Examiner les
captures** pour le zoom, la vue côte à côte ou la séparation A/B. Post-SR et
Pre-SR capturent après l’accentuation configurée au niveau de DLSS, avant les
effets ajoutés ensuite par le jeu. Le mode Image finale capture l’image du jeu traitée, avec
le HUD préservé le cas échéant, avant la génération d’images et le menu Encore.
Le point de capture figure dans l’export.

Le panneau sépare temps GPU NR/SR, modèles mesurables, intervalles source et
mémoire en MiB. Les allocations NR suivies, l’usage local du processus sur le
nœud 0 de l’adaptateur de rendu et l’usage global NVIDIA restent distincts.
Le point de capture du mode Image finale ne mesure pas le temps GPU de SR. Les temps GPU
de NR et de ses modèles sont disponibles quand le travail peut être mesuré
dans le même flux de commandes. En mode Image finale, le compteur GPU de NR démarre
après la préparation du mouvement et de la profondeur et exclut les transferts
Vulkan ou DirectX 11 de certains chemins. Il mesure uniquement le traitement NR.
Une mesure absente reste **N/D**. Le détail des modèles du pont D3D11 et son
compte complet d’allocations privées sont indisponibles. Le suivi exclut les
allocations opaques NVIDIA et ne mesure pas la résidence. Une variation globale donne du contexte et ne suffit pas à attribuer une
économie de mémoire à un ordre de traitement.

Exporter hors du dossier du jeu produit JSON, CSV, pixels originaux, aperçus
SDR BMP et `compare.html` pour examiner les pixels à pleine résolution. Le HDR
original garde son format numérique ; son aperçu est tonemappé. Une capture
incompatible indique sa raison. Les captures successives sont des images
différentes ; une image fixe ne prouve pas la stabilité temporelle. Comparer
aussi mouvements de caméra, désocclusions et détails fins en mouvement.
L’annulation reste possible. La restauration du mode initial conserve les
modifications manuelles ultérieures. Aucun gain relatif n’est présenté pour
des séries incompatibles. L’arrêt cesse les nouvelles mesures ; les travaux
GPU déjà engagés attendent leur complétion pour libérer leurs ressources.

<a name="mode-present"></a>

## Mode Image finale

Dans **Qualité d’image → Neural Rendering**, activez **Neural Rendering** et réglez **Ordre de traitement** sur **Image finale**. Ce mode expérimental traite les images compatibles du jeu après le post-traitement, avant la génération d’images, le menu Encore et l’affichage. Si une configuration compatible fournit la scène et le HUD séparément, NR traite la scène et préserve le HUD. Les petits textes et les éléments mobiles du HUD déjà dessinés dans une image traitée d’un seul bloc peuvent changer avec le reste de l’image.

Le mode Image finale accepte les chemins DirectX 11, DirectX 12 et Vulkan compatibles utilisant le DLSS du jeu, en SDR, HDR10 10 bits (PQ) ou scRGB en virgule flottante, avec une à quatre passes NR. **NR fovéal** et **Éviter le padding NR** peuvent être utilisés ensemble. Avec un **NIS** compatible sous DirectX 11, le mode Image finale intervient après NIS et son anticrénelage optionnel, y compris dans les jeux sans DLSS. Ce chemin peut alimenter la génération d’images compatible ajoutée par Encore. NIS conserve ses limites de SDR, de mode fenêtré et de mise à l’échelle. Les autres moteurs de mise à l’échelle ajoutés restent hors de ce mode. Les combinaisons avec DLAA et Ray Reconstruction restent à valider en jeu.

- **DirectX 11.** Les configurations compatibles de XeFG ou de génération d’images DLSS ajoutées par Encore peuvent utiliser l’image source améliorée. DirectX 11 sur DirectX 12 est aussi pris en charge lorsque le jeu crée une chaîne de présentation DirectX 11 compatible dans cette configuration. Si le jeu crée sa chaîne en DirectX 12 et en enveloppe séparément les tampons pour DirectX 11, ce chemin est hors périmètre.
- **DirectX 12.** La génération d’images DLSS du jeu via Streamline peut utiliser l’image entière ou une scène et un HUD prémultiplié correspondants en SDR ou HDR10. En HDR10, l’interface doit utiliser une texture séparée de haute précision. Si l’image ne correspond pas à la composition attendue du HUD, le mode Image finale conserve ses couleurs d’origine. Les configurations FSR natives prises en charge exigent une composition standard compatible de l’interface. Les configurations natives compatibles de génération d’images XeSS acceptent l’image entière ou des couches prémultipliées correspondantes de pleine taille en SDR ou HDR10. Le chemin HDR10 natif XeSS exige une scène, une interface et une sortie au même format RGB10A2. Les images d’origine sont conservées si l’alpha 2 bits de l’interface ne reproduit pas l’image du jeu. Le mode Image finale fonctionne aussi avec les configurations compatibles de génération d’images DLSS ou XeFG ajoutées par Encore. Pour XeFG ajouté par Encore sous DirectX, réglez **Traitement HUD/interface** sur **Interpoler**.
- **Vulkan.** Les configurations compatibles de génération d’images DLSS du jeu via Streamline et de XeFG ajouté par Encore peuvent utiliser l’image entière ou des couches de scène et de HUD séparées compatibles. Ce second chemin exige une interface prémultipliée de pleine taille avec une scène correspondante. La génération d’images DLSS du jeu accepte le SDR compatible, le HDR10 avec une texture d’interface haute précision et le scRGB avec une scène et une interface en virgule flottante. Avec XeFG ajouté par Encore, les couches séparées exigent du SDR et le réglage **Traitement HUD/interface → Couches du jeu**. Les couches partielles et les entrées d’interface ne contenant que l’alpha restent hors périmètre. Sélectionnez **Image finale** avant de lancer un jeu Vulkan. Si vous passez à ce mode en jeu, y compris après avoir utilisé Pre-SR ou Post-SR, redémarrez le jeu pour appliquer le changement.
- **Smooth Motion.** Le mode Image finale accepte certaines configurations DirectX 11 et les chemins DirectX 12 compatibles d’Encore avec le pilote NVIDIA 621.14 exact. Le Smooth Motion natif sous DirectX 12 sur RTX 40/50 et celui sous Vulkan sont hors périmètre.
- **Génération d’images désactivée.** Les jeux utilisant une configuration Streamline DLSS compatible peuvent toujours utiliser le mode Image finale s’ils fournissent des entrées d’interface séparées.

Utilisez **Estimation du mouvement** dans la section NR pour choisir comment estimer le mouvement à partir de l’image soumise à l’affichage. Cela ajoute du travail GPU et peut se comporter différemment du mouvement fourni par le jeu à DLSS. **Résolution → Rendu** prend pour base la taille de rendu du jeu si elle est disponible pour l’image courante, sinon la taille de sortie. Le repli sur la taille de sortie peut coûter plus cher. Les échelles en pourcentage et personnalisées sont relatives à la taille de sortie. Vérifiez les dimensions en direct et le **Mode effectif** dans le menu.

En l’absence d’image compatible, l’image d’origine et le mode demandé sont conservés. Utilisez l’**Atelier image et performances** pour capturer en mode Image finale et le comparer à l’un des deux modes SR. La qualité d’image, les temps de traitement et la génération d’images sur ces chemins restent à valider en jeu.

## Prérequis

- Le fichier NR de NVIDIA, **`nvngx_dlssnr.dll` version 310.8.0 ou 310.9.16**. Il n'est pas inclus dans RTX Encore :
  fournissez-le vous-même et placez-le à côté du fichier du mod, normalement dans le dossier de l'exécutable du jeu.
  Toute autre version est refusée. [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) télécharge ce fichier, le vérifie et le place là pour vous.
- Un jeu avec **DLSS Super Resolution ou DLAA activé**. NR s'applique à son résultat.
  Avec **Ray Reconstruction**, NR s'applique au résultat de RR, après son débruitage : jamais à l'image brute du
  ray tracing. Le jeu doit fournir l'entrée de profondeur dont NR a besoin ; sinon l'image reste telle que DLSS l'a
  produite et le menu l'indique.
  **Les jeux sans DLSS** sont l'exception depuis la 2.0.0 : là, l'interrupteur **Neural Rendering** lance le NR sur
  l'image que le jeu affiche, avec le mouvement estimé par RTX Encore et le depth buffer du jeu quand il en a un, en
  DirectX 11, DirectX 12 et Vulkan. Voir [Jeux sans DLSS](GAMES-WITHOUT-DLSS.fr.md#neural-rendering).
- Avec la bêta 1.2, les entrées de profondeur linéaire compatibles sont également prises en charge après RR sous Direct3D 12. Une profondeur invalide ou non prise en charge laisse NR inactif ; Direct3D 11 et Vulkan ne prennent toujours pas en charge ce cas. Un essai local dans 007 First Light en path tracing confirme que NR fonctionne, sans mesure de qualité d’image ni de coût.
- Une carte RTX 40 ou RTX 30. RTX 20 est expérimental ; RTX 50 est aussi accepté à titre expérimental sous Vulkan.

## L'activer

1. Placez `nvngx_dlssnr.dll` à côté du fichier du mod.
2. Activez DLSS ou DLAA dans le jeu.
3. Ouvrez le menu (**Inser**), onglet **Image quality**, et activez **Neural Rendering**.

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
| **NR model preset** | B | Avec `nvngx_dlssnr.dll` 310.9.16 : **B** est un réseau plus petit et bien plus rapide, **A** le plus grand, celui que 310.8.0 exécute, **DLL default** laisse le fichier choisir (B). Avec 310.8.0, chaque choix exécute son unique réseau. Il se trouve à côté des presets de modèle SR et RR (onglet **Image quality**, Upscaling) et s'applique en direct. |

Dans un test local en 1080p sur une RTX 3070 Ti Laptop, le réseau B a pris environ 15 ms de temps NR et le réseau A
environ 27 ms. B donne une image différente de A. La prise en charge de 310.9.16 est expérimentale et peu testée.

### Réglages de performance sur RTX 30

Le groupe **Image quality → Neural Rendering → Performance** donne accès aux travaux de RTX Encore pour réduire le coût
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

NR ajoute du travail GPU quand il traite une image. Mesuré dans le benchmark intégré de Black Myth: Wukong sur un
RTX 3070 Ti Laptop (2560×1440, DLSS à 58 %, génération d'images X3) :

| Configuration | FPS moyens | 5 % les plus bas |
| :--- | ---: | ---: |
| NR désactivé | 146 | 127 |
| NR activé, Precision Exact | 76 | 71 |
| NR activé, Precision Fast | 82 | 76 |
| NR activé, Precision Fast, Resolution Performance | 90 | 83 |

Il s'agit des **FPS affichés avec la génération d'images X3**, avec le moteur NR NVIDIA. Dans ce test, passer des
réglages par défaut à Precision Fast et Resolution Performance augmente les FPS affichés de 18 % (76 à 90 FPS).
Le chiffre de 17 % concerne le temps de traitement NR. Ces mesures s’appliquent au chemin de reconstruction DLSS testé avec la génération d’images X3.

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


## 2.3.0

### Looks NR et exposition

Sept looks sont disponibles dans le mod et le Hub : Standard, Discret, Naturel, Cinématique, Visages, Peau douce et Référence NVIDIA. Un look modifie l’apparence et remet les surcharges d’apparence par passe sur Global, sans changer le nombre de passes, les modèles, la résolution ni l’exposition. L’exposition NR propose Jeu, Automatique, Mesurée et Manuelle pour les entrées DLSS linéaires scène compatibles. Une nouvelle configuration utilise Automatique ; un fichier existant sans cette clé conserve Jeu.
