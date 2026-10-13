# Menu et overlay

*[English](MENU-AND-OVERLAY.md) · [中文](MENU-AND-OVERLAY.zh-CN.md)*

RTX Encore dessine son propre menu et son overlay dans les jeux DirectX 11, DirectX 12 et Vulkan. Rien d'autre n'est
à installer.

## Le menu

Appuyez sur **Inser** dans la fenêtre du jeu pour l'ouvrir ou le fermer. Il s'ouvre une fois de lui-même au premier
lancement. Le raccourci se change dans le menu.

Une rangée de quatre pastilles coiffe chaque page : **Upscaling**, **Frame Generation**, **Smooth Motion** et
**Neural Rendering**. Chacune montre l'état de sa fonction par un point de couleur et un chiffre ; survolez-la pour
le détail, cliquez pour aller à ses réglages. Quand un problème dure, une carte sous la rangée le nomme et dit
comment le corriger.

| Onglet | Ce qu'il contient |
| :--- | :--- |
| **Frames** | La génération d'images avec son preset de modèle (A/B Normal, C Lite), son option Fast indépendante et ses options de qualité et d'interface, Smooth Motion, la limite d'images calculées, la V-Sync |
| **Image** | La résolution de rendu DLSS, les presets de modèles SR/RR, Neural Rendering |
| **Overlay** | Un aperçu de l'overlay, sa position, la taille de son texte et les lignes qu'il affiche |
| **System** | Les raccourcis, la compatibilité, les diagnostics et les détails de la session, avec un bouton **Copy** pour les rapports de bug |

À savoir :

- Chaque changement est enregistré aussitôt dans `rtx-encore.jsonc`, commentaires conservés.
- La plupart des réglages s'appliquent en direct. Un `*` après un nom marque un réglage lu au démarrage du jeu ; il
  passe à l'orange une fois modifié, et une ligne en bas du menu liste les changements en attente d'un redémarrage.
- Un réglage modifié a un nom plus lumineux et un bouton de réinitialisation.
- **FG model preset** demande de relancer le jeu : **A/B utilisent Normal ; C (Lite) active automatiquement Lite**. **Fast frame generation** reste indépendant. Le forçage Lite est réservé au diagnostic dans le JSON, sans réglage séparé dans le menu. Voir les [réglages](SETTINGS.fr.md#dlsspresets).
- Chaque section montre ses réglages principaux ; les autres sont dans des groupes qui s'ouvrent sur place
  (**Fine tuning**, **Performance**, **Diagnostics**, ...).
- Faites glisser la barre de titre pour déplacer le menu, et son coin inférieur droit pour le redimensionner.
  Position et taille sont retenues.
- Si un bouton était enfoncé à l'ouverture du menu, relâchez-le avant de cliquer.

### Quand le menu s'ouvre dans une fenêtre séparée

Dans quelques jeux, le menu ne peut pas être dessiné sans risque dans l'image. Les réglages s'ouvrent alors dans une
fenêtre séparée et l'overlay devient un petit texte posé sur le jeu, qui laisse passer les clics. Le mode fenêtré
sans bordure est recommandé dans ce cas. Les réglages et leur effet sont les mêmes.

### Disponibilité des fonctions

**System → Component compatibility** affiche les versions chargées de Streamline, DLSS-G, SR et RR, les fonctions disponibles et la raison de chaque limite. Les choix indisponibles ou non qualifiés sont grisés. Un composant pas encore chargé est indiqué en attente ; cela peut changer après activation de DLSS/FG dans le jeu. Il reste possible de désactiver un forçage enregistré ou de revenir aux réglages du jeu. Une demande de preset disponible ne prouve pas quel modèle le pilote utilise réellement.

## L'overlay

Activez-le dans l'onglet **Overlay** ou avec `Ctrl + Alt + O` ; `Ctrl + Alt + P` le déplace dans le coin suivant.

| Exemple | Signification |
| :--- | :--- |
| `74 fps` | Fréquence d'images du jeu ; aucune génération d'images observée |
| `74/37 fps 2x` | Fréquence affichée / calculée, avec le multiplicateur observé |
| `148/74 fps sm` | Sortie estimée avec Smooth Motion / fréquence d'images du jeu |
| `296/74 fps 2x+sm` | Génération d'images X2, puis Smooth Motion |

Lignes optionnelles, toutes désactivées par défaut :

| Ligne | Affiche |
| :--- | :--- |
| Neural Rendering | Son état et la taille à laquelle il travaille |
| Régularité des images | Temps d'image : moyenne, 99e centile et gigue |
| GPU | Charge, température, puissance et fréquences |
| VRAM | Utilisée / totale sur la carte, tous programmes confondus |
| Versions | Versions de DLSS, DLSS Frame Generation et Streamline chargées par le jeu |
| Interface | Si le traitement séparé de l'interface est actif, et d'où viennent la scène sans interface et la couche d'interface |

La position (quatre coins) et la taille du texte (10 à 32 px) sont dans le même onglet.

## Autres commandes

| Réglage | Onglet | Ce qu'il fait |
| :--- | :--- | :--- |
| **Render resolution** | Image | Force la résolution à laquelle le jeu calcule l'image avant DLSS : DLAA, Quality, Balanced, Performance, Ultra Performance, ou une échelle personnalisée de 50 à 100 %. Le jeu doit déjà utiliser DLSS. Le menu montre la résolution observée et si le jeu suit la demande ; revenez à *Game* si un jeu se comporte mal. Dans les jeux Unreal Engine 4 et 5, le pourcentage d'écran est ajusté en direct quand c'est possible. |
| **Frame limit (FPS)** | Frames | Demande un plafond via NVIDIA Reflex compatible. 0 restaure les réglages du jeu. Les images générées peuvent compter dans ce plafond selon le jeu et le pilote. Suspendu en mode Dynamic. |
| **Request V-Sync off** | Frames | Demande aux jeux DirectX de présenter sans V-Sync. Un réglage forcé dans le panneau de configuration NVIDIA garde la priorité. Sans effet dans les jeux Vulkan. |
| **Compatible ray-traced hair** | System | The Witcher 3 (DirectX 12, versions du jeu 25575366 et 25646871) : rend les cheveux en ray tracing du jeu disponibles sur les cartes RTX 20, 30 et 40. Les réglages de cheveux du jeu décident toujours s'ils sont affichés. Pas encore validé en jeu sur RTX 20 et RTX 30. Relancez pour appliquer. |

### Plafond Reflex suggéré

Le limiteur Reflex accepte les versions de DLL 2.1.1–2.14.1, et 2.14.16. Avec une autre version, les réglages du jeu sont conservés.
Les SDK publiés dans cette plage ont été examinés hors ligne ; chaque version n'a pas été validée en jeu.
Les versions intermédiaires sans tag public, dont 2.13 et 2.14.0, n'ont pas été vérifiées avec un SDK public.
Son effet dépend toujours de l'intégration Reflex du jeu et du pilote. Aucun limiteur de repli par attente CPU n'est ajouté.

Dans **Frames → Frame pacing**, **For this display** propose `fréquence - fréquence² / 3600`, arrondi à l’entier inférieur :
par exemple **116 FPS à 120 Hz**, 138 à 144 Hz et 224 à 240 Hz, sans division par le multiplicateur FG. Selon le jeu et le pilote, les images
générées peuvent compter dans le plafond. Dans Starfield avec Streamline 2.12, une demande de 90 a donné environ
22,5 FPS rendus avec FG ×4. Comparez la cadence source mesurée et celle incluant FG.
Cliquez sur **Use … FPS** pour enregistrer la suggestion ; changer d’écran ou de multiplicateur n’ajuste jamais le plafond automatiquement.
Vous pouvez aussi saisir directement une valeur dans **Frame limit (FPS)**, puis valider par **Entrée** ou **Apply**.
La plage est de 1 à 1000 FPS. **Use game settings** (ou 0) restaure le plafond du jeu, sans désactiver tous les limiteurs.
Cliquer ailleurs n’enregistre pas une saisie inachevée.

La suggestion est indisponible en mode Dynamic, avec Smooth Motion, ou tant que la fréquence de l’écran ou
l’état actuel de la génération est inconnu. Le réglage manuel reste disponible et nécessite Reflex compatible.
Le menu distingue la demande et son statut Reflex des FPS source mesurés : **Reflex accepted … FPS** confirme la demande,
pas que la cadence mesurée ou celle de l’affichage lui correspond.

La marge grandit avec la fréquence de l’écran. C’est un point de départ, sans détection automatique du VRR ni garantie de rester dans sa plage.
Un plafond plus bas peut réduire charge, consommation et chauffe s’il réduit effectivement la cadence de rendu.
Un plafond supérieur aux FPS déjà atteints peut n’apporter aucun bénéfice ; ajustez-le en comparant la même scène à chaud.

## Raccourcis

Les raccourcis par défaut sont listés dans [Génération d'images](FRAME-GENERATION.fr.md#raccourcis). Dans l'onglet
**System**, chaque action peut recevoir plusieurs combinaisons de touches, ou aucune. Les raccourcis n'agissent que
lorsque la fenêtre du jeu a le focus.

## Journaux

Les journaux de session sont écrits dans le dossier `rtx-encore-logs` à côté du fichier du mod ; les trois plus
récents sont conservés (**Session logs kept**, 1 à 100). **Log performance** écrit la fréquence et les temps d'image
de la session dans un fichier CSV du même dossier.

Les journaux d’exception sont conservés séparément, en même nombre. Chaque processus a son fichier ; les journaux encore utilisés sont conservés.

Avec **Log performance**, le journal de session relève aussi la température, la puissance, les fréquences GPU et les limitations signalées, environ une fois par seconde. Cela fonctionne avec l’overlay masqué et la Frame Generation désactivée. Les mesures indisponibles sont marquées `NA`. Voir [les baisses de performances](TROUBLESHOOTING.fr.md#baisse-de-performances-après-quelques-minutes).
