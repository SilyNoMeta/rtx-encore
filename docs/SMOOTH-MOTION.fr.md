# Smooth Motion sur RTX 30

*[English](SMOOTH-MOTION.md) · [中文](SMOOTH-MOTION.zh-CN.md)*

Smooth Motion est l'interpolation d'images du pilote NVIDIA : elle ajoute une image entre deux images calculées
dans les jeux DirectX 11, DirectX 12 et Vulkan, **y compris les jeux sans DLSS**. RTX Encore la rend disponible sur
les cartes RTX 30.

> [!NOTE]
> Cette fonction est désactivée par défaut. Elle est distincte de DLSS Frame Generation : les deux
> peuvent servir dans des jeux différents, ou ensemble dans un jeu qui a les deux.

## Prérequis

- Une carte **RTX 30**. Les RTX 40 tiennent Smooth Motion de NVIDIA ; les RTX 20 ne sont pas prises en charge.
- L'une de ces versions du pilote NVIDIA, exactement : **621.01**, **620.12**, **617.42**, **617.14**,
  **616.92**, **616.64** ou **591.86**.
  Les 617.14 et 621.01 ont été vérifiées en jeu. Avec tout autre pilote, la fonction reste désactivée et dit
  pourquoi ; le reste de RTX Encore continue de fonctionner.

## L'activer

1. Ouvrez le menu (**Inser**), onglet **Frames**, section **Smooth Motion**.
2. Activez **Smooth Motion**.
3. Réglez **Graphics API** sur l'API que le jeu utilise réellement : Direct3D 12, Direct3D 11 ou Vulkan. Ce n'est
   pas toujours celle que son moteur laisse supposer ; la page du jeu sur
   [PCGamingWiki](https://www.pcgamingwiki.com/) l'indique.
4. Relancez le jeu.

La pastille **Smooth Motion** en haut du menu montre l'état. « Driver prepared » signifie que le pilote est prêt ;
cela ne confirme pas à soi seul que des images interpolées sont affichées. Vérifiez le résultat avec un outil qui
mesure les images affichées, comme NVIDIA FrameView.

## Ce qui a été observé

| API | Jeu | Résultat rapporté |
| :--- | :--- | :--- |
| DirectX 12 | Manor Lords | 120 FPS affichés pour un jeu limité à 60 FPS |
| DirectX 11 | Shadows of Doubt | 120 FPS affichés pour un jeu limité à 60 FPS |
| Vulkan | Enshrouded | Images interpolées affichées, le profil NVIDIA du jeu étant sur Off |

Ce sont quelques observations d'utilisateurs avec le pilote 617.14 ; Enshrouded a aussi été confirmé avec le pilote
621.01. Elles ne garantissent ni la compatibilité, ni la latence, ni la qualité d'image dans d'autres jeux.

## Dans l'overlay

| Exemple | Signification |
| :--- | :--- |
| `148/74 fps sm` | Sortie estimée avec Smooth Motion / fréquence d'images du jeu |
| `296/74 fps 2x+sm` | DLSS Frame Generation X2, puis Smooth Motion |

Le chiffre de Smooth Motion est une estimation : le pilote peut suspendre l'interpolation, et l'overlay ne mesure
pas ce que l'écran reçoit. Il n'apparaît qu'une fois l'interpolation du pilote démarrée ; d'ici là, l'overlay
affiche la seule fréquence d'images du jeu, avec l'une de ces lignes, et le menu montre le même état avec la marche
à suivre :

| Ligne | Signification |
| :--- | :--- |
| `SM OFF: SET GRAPHICS API TO VULKAN` | Le jeu affiche en Vulkan alors que **Graphics API** est sur Direct3D |
| `SM NOT RUNNING: NO DRIVER MODULE LOADED` | Le pilote est prêt mais n'a pas commencé à interpoler |

## Jeux Vulkan exclus par le pilote

Dans les jeux Vulkan, le pilote NVIDIA décide jeu par jeu si Smooth Motion peut fonctionner. Avec Smooth Motion
activé et **Graphics API** sur Vulkan, RTX Encore lève cette décision : Smooth Motion fonctionne même dans un jeu
que le profil du pilote exclut. Il n'y a plus de réglage séparé pour cela.

> [!WARNING]
> NVIDIA peut exclure un jeu pour une raison. Smooth Motion peut y donner des résultats inattendus : curseur de
> souris dédoublé, artefacts visuels ou plantage. Désactivez Smooth Motion pour ce jeu s'il se comporte mal.

Si Smooth Motion ne démarre toujours pas dans un jeu Vulkan, il ne peut pas fonctionner dans ce jeu.

## Si cela ne fonctionne pas

- **Le menu signale un pilote non pris en charge** : installez l'une des versions ci-dessus.
- **Rien ne change en jeu**, ou le menu indique **Wrong API** ou **Not running** : vérifiez le choix
  **Graphics API**, et relancez le jeu après chaque changement. Les émulateurs sont souvent en Vulkan.
- **Vulkan** : essayez avec Smooth Motion sur Off dans le profil du jeu de l'application NVIDIA. Voir aussi
  [Jeux Vulkan exclus par le pilote](#jeux-vulkan-exclus-par-le-pilote).
- Joignez le rapport du menu (onglet **System**, **Copy**) quand vous ouvrez un ticket.
