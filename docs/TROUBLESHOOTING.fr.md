# Dépannage

*[English](TROUBLESHOOTING.md) · [中文](TROUBLESHOOTING.zh-CN.md)*

## Intel XeSS et XeFG (expérimental)

RTX Encore 1.2.0 permet de choisir les moteurs de reconstruction et de génération d’images. Laissez **Game** pour conserver le moteur habituel du jeu. Les modes Intel sont expérimentaux, désactivés par défaut et demandent des entrées de jeu compatibles ; le choix d’un moteur ne prouve pas son activation.

[Intel XeSS et XeFG (expérimental)](INTEL-ENGINES.fr.md)

## Premières vérifications

1. **Le mod est-il chargé ?** Un dossier `rtx-encore-logs` apparaît à côté du fichier du mod quand il l'est, et
   **Inser** ouvre le menu. Si ni l'un ni l'autre ne se produit, le jeu n'a pas chargé le fichier.
2. **N'y a-t-il qu'une seule copie ?** Deux copies du mod dans le même jeu, sous deux noms, provoquent des plantages,
   des comportements étranges et une image dégradée avec Neural Rendering.
3. **Que dit le menu ?** Les quatre pastilles du haut donnent l'état de chaque fonction, et une carte nomme tout
   problème durable avec sa correction.

## Le mod ne se charge pas

| Vérification | Quoi faire |
| :--- | :--- |
| Mauvais dossier | Le fichier doit être à côté de l'exécutable qui fait réellement tourner le jeu. Pour Unreal Engine : `<Jeu>\Binaries\Win64\`. |
| Ce jeu ne charge pas ce nom | Essayez le nom suivant : `version.dll`, puis `dinput8.dll`, `winmm.dll`, `dxgi.dll`. |
| Le nom est pris par un autre mod | Laissez son fichier à ce mod et choisissez un autre nom pour RTX Encore. |
| Le fichier renommé est refusé | Utilisez la copie déjà nommée du dossier `alternative-proxies`. |
| Le jeu a un anti-triche | Il peut bloquer le fichier. RTX Encore ne doit pas y être utilisé. |

## Génération d'images

| Symptôme | Première vérification |
| :--- | :--- |
| Le jeu n'a pas d'option Frame Generation | RTX Encore étend DLSS Frame Generation dans les jeux qui l'ont ; il ne peut pas l'ajouter. Voir [Smooth Motion](SMOOTH-MOTION.fr.md) pour les autres jeux. |
| L'option existe mais est grisée | Vérifiez que DLSS est activé, que le jeu tourne en DirectX 12 ou Vulkan, et que le mod est chargé. |
| Le multiplicateur reste à X2 | Désactivez puis réactivez la génération d'images dans le jeu, ou relancez-le. Vérifiez le mode dans le menu : en **Game decides**, c'est le jeu qui choisit. |
| X5 ou X6 n'est pas atteint en mode Dynamique | Activez **Allow 5x and 6x**. |
| Rien ne se passe avec cette version du jeu | Le fichier `nvngx_dlssg.dll` du jeu est peut-être dans une version non prise en charge : voir [Compatibilité](COMPATIBILITY.fr.md#versions-de-dlss-frame-generation). |
| Contours dédoublés ou images déformées en mouvement | Désactivez **Automatic UI recomposition**. |
| Défauts sur les grillages, les feuillages ou les ombres | Gardez **Anti-tearing / anti-ghosting** activé et essayez l'autre **Protection tuning**. |
| Saccades ou plantages aux multiplicateurs élevés | La VRAM est probablement pleine : baissez le multiplicateur ou la qualité des textures. |
| Plantage dans les menus ou les écrans de chargement | Laissez **Disable menu detection** désactivé. |

## Smooth Motion et Neural Rendering

Voir la fin de leurs pages : [Smooth Motion](SMOOTH-MOTION.fr.md#si-cela-ne-fonctionne-pas),
[Neural Rendering](NEURAL-RENDERING.fr.md#quand-quelque-chose-ne-fonctionne-pas).

## Menu et réglages

| Symptôme | Première vérification |
| :--- | :--- |
| **Inser** ne fait rien | Le mod n'est pas chargé (voir plus haut), ou le raccourci a été changé : regardez `menuState` dans `rtx-encore.jsonc`. |
| Le menu s'ouvre dans une fenêtre séparée | Normal dans quelques jeux : voir [Menu et overlay](MENU-AND-OVERLAY.fr.md#quand-le-menu-souvre-dans-une-fenêtre-séparée). |
| Les clics ne sont pas pris en compte | Relâchez tous les boutons de la souris, puis cliquez de nouveau. Le mode fenêtré sans bordure aide. |
| Les réglages reviennent aussitôt en arrière | Le fichier de réglages n'a pas pu être enregistré : vérifiez qu'il n'est ni en lecture seule, ni ouvert et verrouillé dans un éditeur. |
| Un réglage n'a aucun effet | Les réglages marqués `*` sont lus au démarrage du jeu : relancez le jeu. |
| Les raccourcis ne font rien | Ils n'agissent que lorsque la fenêtre du jeu a le focus, et `disableKeybinds` doit valoir `false`. |

## Baisse de performances après quelques minutes

Activez **System → Diagnostics → Log performance**, puis jouez depuis un démarrage à froid jusqu’au ralentissement.
Gardez les mêmes réglages pendant cette capture et notez l’heure approximative de la chute. Envoyez le journal
de session du jeu dans `rtx-encore-logs` ; le CSV des temps d’image, s’il est produit, est également utile.
Désactivez ensuite cette option.

Les lignes `[GPU-PERF]` relèvent températures, puissance, fréquences, limites signalées, charge CPU et état
secteur/batterie avec la cadence du jeu. Elles identifient la carte NVIDIA surveillée ; vérifiez son nom si vous
avez plusieurs GPU NVIDIA. Une mesure absente vaut `NA`, jamais zéro. La température et la puissance CPU,
ainsi que la vitesse des ventilateurs du portable, ne sont pas relevées. Ces mesures ne prouvent pas l’état de la pâte thermique.

Une limitation thermique signalée aide à confirmer un ralentissement lié à la chauffe. Une limitation de
puissance seule ne départage pas un profil du portable, le partage CPU/GPU ou une autre contrainte électrique.
Les événements brefs entre deux relevés peuvent être manqués. Comparer la même scène avant et après la chute
est particulièrement utile.

Pour une comparaison distincte, essayez **Frames → Frame pacing → Suggested Reflex cap** si disponible,
ou une demande Reflex manuelle. La suggestion utilise une limite sous la fréquence écran (224 FPS à 240 Hz) ; ce n’est pas une cible
thermique. Le comptage des images FG dépend du jeu et du pilote. Si l’application ne réduit pas la cadence de
rendu mesurée, essayez une demande plus basse. Comparez les FPS source et les mesures GPU ; `accepted` seul ne
démontre pas un gain. Le plafond enregistré reste fixe jusqu’à votre prochaine modification.
Voir [les commandes du plafond](MENU-AND-OVERLAY.fr.md#plafond-reflex-suggéré).

## Signaler un problème

Les messages normaux de Streamline/NGX, dont les motifs de refus des fonctions qu'ils signalent, sont enregistrés
automatiquement même si les options de diagnostic sont désactivées. Leur nombre est limité pour éviter que des
messages répétés remplissent le journal. Si le support demande davantage de détails, passez `diagnostics.logStreamline`
à `true` dans `rtx-encore.jsonc`, puis relancez le jeu. Désactivez ensuite cette option et redémarrez : les traces
verbeuses peuvent réduire les performances.

Le journal de session relève automatiquement les informations de base, même avec les options de diagnostic
désactivées : version de Windows, CPU, mémoire, cartes graphiques et modes du bureau, versions et chemins
des DLL chargées (jeu, mod, Streamline, DLSS et composants du pilote compris). Les versions indisponibles
sont signalées. Plusieurs copies chargées ne prouvent pas laquelle est utilisée. La collecte peut manquer
un crash très précoce ou une DLL déchargée avant son observation.

Les créations DLSS observées indiquent aussi l’API graphique, le mode SR/RR, la qualité, les résolutions et
les presets demandés. **Un preset demandé ne confirme pas le preset effectivement utilisé** : le pilote
ou une substitution peut choisir un autre modèle. Valeur par défaut, absente ou inconnue restent distinctes.
Les rapports sont bornés et dédupliqués lorsque c’est pertinent ; les performances image par image et les traces
graphiques/NR détaillées demandent toujours leurs options. Compressez tout le dossier `rtx-encore-logs` du
lancement concerné pour inclure le jeu et les éventuels chargeurs/rapporteurs d’erreur. Le rapport **Copy** du
menu est un résumé et ne remplace pas ce dossier.

Ouvrez un [ticket](https://github.com/SilyNoMeta/rtx-encore/issues) avec :

- le jeu, sa version ou sa boutique, et son API graphique ;
- votre carte et la version du pilote NVIDIA ;
- la version de RTX Encore et le nom que vous avez donné au fichier ;
- le rapport copié depuis le menu (onglet **System**, **Copy**) ;
- le journal de la session où le problème s'est produit, pris dans `rtx-encore-logs`. Les journaux peuvent contenir
  des noms de dossiers de votre PC : relisez-les avant de les publier.

Un problème par ticket, avec ce que vous avez fait et ce que vous avez vu.
