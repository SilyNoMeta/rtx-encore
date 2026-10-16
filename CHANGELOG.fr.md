# Journal des versions

*[English](CHANGELOG.md) · [中文](CHANGELOG.zh-CN.md)*

## 1.0.0

La première version stable de RTX Encore, après les bêtas et les versions candidates ci-dessous.

- **Star Wars Outlaws : plantage au lancement** ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)). Le jeu complet (Ubisoft Connect) se ferme au lancement quand le menu RTX Encore y est attaché ; sa démo, non. Le nouveau réglage **`"disableInGameMenu": true`** laisse le menu de côté, et la génération d’images jusqu’à X4 continue de fonctionner, comme l’ont confirmé deux utilisateurs avec des builds de test de cette modification. Le profil communautaire du jeu dans [RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub) l’active. Voir [Compatibilité](docs/COMPATIBILITY.fr.md). Correction des notes de la rc.3 : la correction des réglages vidéo de la rc.3 ne couvrait pas ce plantage au lancement.
- **Rapports de plantage :** deux nouveaux réglages, désactivés par défaut, aident à diagnostiquer un plantage. `logDxgiWrappers` enregistre les appels du jeu sur la swapchain du menu ; `crashDetails` ajoute des détails mémoire aux logs d’exception et laisse un instantané mémoire `.dmp` quand le jeu ou RTX Encore plante. Voir [Paramètres](docs/SETTINGS.fr.md).
- **`profileFrameGpu`** (désactivé par défaut, DirectX 12) : enregistre le temps GPU de chaque passe DLSS Super Resolution, Ray Reconstruction et génération d’images dans `rtx-encore-logs`. Temps de travail GPU seulement, ni FPS affichés ni latence ; pour les mesures.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. Le réglage pour Star Wars Outlaws a été vérifié sur la démo du jeu et par deux utilisateurs avec des builds de test de la même modification ; le build final 1.0.0 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué.

### Remerciements

- [@ivanmestre](https://github.com/ivanmestre) — les tests d’isolation et de trace, leurs logs et instantanés de plantage, qui ont localisé le plantage de Star Wars Outlaws ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).
- [@yannduwooz83](https://github.com/yannduwooz83) — les tests de Star Wars Outlaws et la comparaison avec le projet précédent, qui a montré ce qui avait changé ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).

Merci à toutes celles et tous ceux qui partagent leurs logs et prennent le temps de tester les builds.

## 1.0.0-rc.3 — préversion

- Correction de Star Wars Outlaws qui plantait quand ses réglages vidéo affichaient les options de génération d’images. Le menu de génération d’images du jeu propose maintenant ce que le jeu prend en charge (jusqu’à 4x) ; le multiplicateur choisi dans RTX Encore s’applique toujours. Confirmé en jeu sur la démo de Star Wars Outlaws (RTX 3070 Ti Laptop). Le plantage au lancement signalé dans [#33](https://github.com/SilyNoMeta/rtx-encore/issues/33) semble avoir la même cause ; son auteur n’a pas encore refait l’essai.
- Prise en charge du pilote NVIDIA **620.12** pour Smooth Motion sur RTX 30 ([#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)). Vérifié à partir des fichiers du pilote seulement ; pas encore lancé avec ce pilote. Voir [Smooth Motion](docs/SMOOTH-MOTION.fr.md).
- Correction du menu qui n’apparaissait pas dans Euro Truck Simulator 2 ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)). Dans Euro Truck Simulator 2, installez RTX Encore sous le nom `winmm.dll`. Vérifié d’après le journal de session d’un essai local ; pas encore confirmé en jeu, et l’installation de l’auteur du signalement n’a pas été retestée.
- Avertissement quand l’ancien module dlssg_for_sm86 est encore installé à côté de RTX Encore ([#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)) : avec les deux actifs, la génération d’images peut ne présenter aucune image générée. Le journal de session et la section Compatibilité du menu le signalent ; retirez l’ancien module.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. La correction Star Wars Outlaws a été vérifiée en jeu avec un build de test de la même modification ; le build final rc.3 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué.

### Remerciements

- [@ivanmestre](https://github.com/ivanmestre) — le signalement Star Wars Outlaws et les logs beta.5 et rc.2 qui ont mené à la correction ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).
- [@CorwinMaximus](https://github.com/CorwinMaximus) — la demande du pilote 620.12 ([#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) — le signalement Euro Truck Simulator 2 ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)).
- [@hasnainb237](https://github.com/hasnainb237) — les logs Star Wars Zero Company qui ont montré l’ancien module resté en place ([#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)).

Merci à toutes celles et tous ceux qui partagent leurs logs et prennent le temps de tester les builds.

## 1.0.0-rc.2 — préversion

- Correction de Star Wars Outlaws qui se fermait après un alt-tab depuis son menu principal, pendant que la recomposition automatique de l’interface capturait la scène. Confirmé en jeu sur une RTX 3070 Ti Laptop.
- L’overlay affiche la version des plugins Streamline quand elle diffère du cœur Streamline du jeu : les mises à jour des plugins laissent ce cœur en place, si bien que l’overlay ne montrait que la version d’origine du jeu. Voir [Menu et overlay](docs/MENU-AND-OVERLAY.fr.md).
- Le journal de session inclut automatiquement les messages normaux de Streamline/NGX et les motifs de refus des fonctions qu'ils signalent, sans activer d'option de diagnostic. Leur nombre est limité ; les traces supplémentaires restent facultatives. Merci à [@lovebabyx](https://github.com/lovebabyx) pour les logs de [#30](https://github.com/SilyNoMeta/rtx-encore/issues/30) qui ont révélé ce manque. Cela améliore le diagnostic ; le défaut de génération d'images signalé sur RTX 20 reste en cours d'analyse.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. La correction Star Wars Outlaws a été vérifiée en jeu avec un build de test de la même modification ; le build final rc.2 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué.

### Remerciements

- [@lovebabyx](https://github.com/lovebabyx) — les logs RTX 2070 Super qui ont montré ce qui manquait au journal de session ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)).

Merci à toutes les personnes qui partagent leurs logs et prennent le temps de tester les builds.

## 1.0.0-rc.1 — préversion

- **Prise en charge des derniers fichiers NVIDIA.** Qualification des fichiers téléchargés par la bêta de l’application NVIDIA du 13 octobre 2026 : Streamline 2.14.16 (MFG dynamique, MFG fixe au-delà de X2, limite de fréquence Reflex, recomposition de l’interface) et DLSS-G 310.9.16 (presets, options de qualité et Fast frame generation). Ajout du preset de frame generation **C (Lite)**, le mode performance plus rapide de NVIDIA, uniquement avec le DLSS-G 310.9.16.0 qualifié ; A/B gardent Normal et **Fast frame generation** reste indépendant. Ces ajouts sont expérimentaux et peu testés ; voir [Compatibilité](docs/COMPATIBILITY.fr.md#versions-de-streamline).
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, le nouveau compagnon Windows, est disponible en version candidate. Il installe RTX Encore dans vos jeux, apporte avec lui les derniers fichiers DLSS et Streamline qualifiés ainsi que le fichier Neural Rendering de NVIDIA, met à jour tous les jeux en un clic, et peut toujours remettre un jeu comme il était. L’installation à la main fonctionne exactement comme avant ; voir [Installation](docs/INSTALLATION.fr.md#le-plus-simple--rtx-encore-hub).
- Ajout de **Force Smooth Motion** (`"smoothMotionForce"`, désactivé par défaut) pour les jeux Vulkan dont le profil NVIDIA laisse Smooth Motion inactif. Le forçage peut donner des résultats inattendus (curseur dupliqué, artefacts, plantage). L’overlay et le menu indiquent désormais quand Smooth Motion ne tourne pas réellement, ou quand l’API graphique choisie ne correspond pas au jeu, au lieu d’afficher une fréquence doublée (observé dans RPCS3).
- Correction de Forza Horizon 6 qui se fermait sur ses logos de démarrage lorsque le menu était attaché ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)). Le jeu reste désormais ouvert lors de lancements locaux répétés avec le menu ; les installations des rapporteurs n’ont pas été retestées.
- Correction de Final Fantasy VII Rebirth qui masquait la Frame Generation sur RTX 20 et 30 ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12), [#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)). Confirmé en jeu sur une RTX 3070 Ti avec le Streamline 2.8.0 du jeu ; non testé avec la mise à jour Streamline 2.14.1 dans ce jeu.
- Correction de Where Winds Meet qui ne démarrait pas (0xc0000142) avec `version.dll` ou `winmm.dll` ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)), et d’un plantage du nom `dinput8.dll` avec l’overlay Epic. Utilisez `version.dll` dans ce jeu : chargé en `dinput8.dll`, RTX Encore arrive après que le jeu a déjà abandonné la DLSS Frame Generation. L’installation du rapporteur n’a pas été retestée.
- Correction du menu qui refusait de s’attacher en Direct3D 12 avec Special K ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)). Confirmation en jeu attendue.
- Correction de **For this display** : la limite proposée garde désormais une marge qui augmente avec la fréquence de rafraîchissement (116 FPS à 120 Hz, 138 à 144 Hz, 224 à 240 Hz) ([#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)).
- Correction du premier lancement après une mise à jour depuis un ancien fichier de réglages : Smooth Motion et quelques autres réglages de démarrage ne prenaient effet qu’au second lancement (observé dans Shadows of Doubt).
- RTX Encore reste inactif dans le rapporteur de plantage d’un jeu, qui écrivait ses propres journaux de session et chassait les vrais des journaux conservés (observé dans Shadows of Doubt).
- Le journal des exceptions enregistre désormais quel thread a terminé le processus, pour distinguer la fermeture du jeu d’un arrêt venu de l’extérieur.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. Les corrections Forza Horizon 6, Final Fantasy VII Rebirth et Where Winds Meet ont été vérifiées avec des builds de test antérieurs ; le build final rc.1 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué.

### Remerciements

- [@avikpal693](https://github.com/avikpal693) et [@wer2357666](https://github.com/wer2357666) — rapports et logs Forza Horizon 6 ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- [@yannduwooz83](https://github.com/yannduwooz83) — rapport et logs Final Fantasy VII Rebirth ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)) ; [@wd3dmlys](https://github.com/wd3dmlys) — la piste de l’architecture GPU ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)) ; [@lovebabyx](https://github.com/lovebabyx) — le rapport sur la beta.5 ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)).
- [@ranmo-1](https://github.com/ranmo-1) — rapport Where Winds Meet ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)).
- [@PhantomGamers](https://github.com/PhantomGamers) — la formule de marge selon la fréquence ([#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)) et le rapport et log Special K ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).

Merci à toutes les personnes qui partagent leurs logs et prennent le temps de tester les builds.

## 1.0.0-beta.5 — préversion

- Correction des changements de recomposition de l’interface (UIR) ignorés lorsque la Frame Generation est déjà active ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)). Cela concerne l’activation et la désactivation, y compris **Force UIR** et les demandes du jeu ou du mode automatique, avec les fichiers Streamline de production qualifiés 2.11.1, 2.12.0 et 2.14.1. Désactiver Force UIR laisse toujours le jeu ou le mode automatique demander l’UIR.
- Périmètre de compatibilité identique à la beta.4. Privilégiez les dernières versions qualifiées, Streamline 2.14.1 et DLSS-G 310.9.1.0, pour bénéficier du plus grand nombre de fonctions ; voir [Compatibilité](docs/COMPATIBILITY.fr.md#versions-de-streamline).

La correction UIR passe les vérifications automatisées hors jeu. La confirmation du build final beta.5 dans Starfield et les autres jeux reste attendue ; aucun gain de performance n’est revendiqué.

### Remerciements

- [@Avictuu](https://github.com/Avictuu) — rapport Starfield, comparaisons de composants et logs ayant aidé à identifier le problème de changement UIR en cours de partie ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).

## 1.0.0-beta.4 — préversion

- Extension du DMFG natif et de la recomposition de l’interface aux fichiers de production Streamline 2.11.1 et 2.12.0 qualifiés, en plus de 2.14.1. Ajout de l’adaptation Encore X2–X4 pour les anciens fichiers DX12 Streamline 2.7.2–2.10.3 qualifiés. Vulkan garde son adaptation ; D3D11 reste limité à Smooth Motion X2. Voir [Compatibilité](docs/COMPATIBILITY.fr.md#versions-de-streamline) pour les versions exactes et les limites.
- Ajout de **System → Component compatibility** : versions chargées, fonctions disponibles et explications. Les choix incompatibles sont grisés, tout en permettant de désactiver un ancien forçage. Privilégiez les dernières versions qualifiées Streamline 2.14.1 et DLSS-G 310.9.1.0 pour profiter du plus de fonctions.
- Correction d’un accès mémoire invalide identifié avec le FG dynamique dans The Witcher 3 ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)) et d’un problème de fermeture de Starfield avec Streamline 2.12 ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)). La fermeture de Starfield a été confirmée avec le précédent build de test ; le retest de la beta.4 finale et la résolution du crash Witcher restent à confirmer.
- Correction de la durée de vie du plugin Reflex lors des réinitialisations de Streamline, pour les problèmes de démarrage Cyberpunk étudiés ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)). Confirmation en jeu encore attendue.
- Correction des mises à jour et restaurations tardives du plafond Reflex entre threads. Saisie FPS directe, **Use game settings** et **For this display** (fréquence écran moins 3 FPS). Le comptage avec FG dépend du jeu/pilote ; le DMFG suspend ce forçage. Compatibilité Reflex étendue aux versions de DLL qualifiées 2.1.1–2.14.1 ; aucun SDK officiel 2.13 disponible pour vérification.
- Choix indépendants des presets SR, RR et FG, appliqués au redémarrage. FG A/B prend en charge les fichiers de production DLSS-G qualifiés individuellement 310.6.0.0, 310.7.0.0, 310.7.128.0, 310.7.129.0, 310.8.0.0, 310.9.0.0 et 310.9.1.0. SR/RR dépend des DLL et des réglages du pilote ; le modèle effectif n’est pas garanti. Par défaut, les choix du jeu sont conservés.
- Journaux automatiques des versions système et des composants chargés, de la qualité/résolution DLSS et des presets SR/RR demandés. Les journaux de performance optionnels ajoutent température, puissance, fréquences, limites GPU signalées et pauses de plus d’une seconde. Ces ajouts améliorent le diagnostic ; ils ne prouvent pas une correction de [#12](https://github.com/SilyNoMeta/rtx-encore/issues/12) ou [#19](https://github.com/SilyNoMeta/rtx-encore/issues/19).

La qualification automatisée ne remplace pas les essais de chaque combinaison jeu/GPU/pilote. L’adaptation ajoutée, l’affichage de compatibilité et les presets attendent encore des retours en jeu.

### Remerciements

- [@Avictuu](https://github.com/Avictuu) — essais répétés de Starfield, multiplicateurs, journaux Reflex/fermeture détaillés et confirmation de la fermeture normale du build de test ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)).
- [@wisecase2](https://github.com/wisecase2) — journaux Witcher 3 de la même session et éléments de suivi Cyberpunk ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@hunan-NRT](https://github.com/hunan-NRT) et [@naruto490610-alt](https://github.com/naruto490610-alt) — rapports, journaux et essais de suivi Cyberpunk ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@yannduwooz83](https://github.com/yannduwooz83) et [@PrincessofCelery](https://github.com/PrincessofCelery) — rapports ayant amélioré les informations de support et le diagnostic des blocages ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12), [#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)).

Merci à tous ceux qui partagent leurs journaux et prennent le temps de tester les builds.


## 1.0.0-beta.3 — préversion

- Correction du crash de démarrage identifié dans The Last of Us avec RTX Encore, OptiScaler et l'ancien runtime Streamline du jeu. Confirmation en jeu attendue ; le cas de Dead Space reste à confirmer.
- Détection correcte de l'activité DLSS Ray Reconstruction. NR indique lorsqu'une profondeur compatible manque, sans masquer l'activité DLSS ; sa compatibilité dépend encore des données du jeu.
- Journaux d'exception distincts pour chaque processus et lancement, afin qu'un rapporteur d'erreur ne remplace pas les données du jeu. Cela améliore l'analyse du crash de démarrage signalé dans Cyberpunk, sans en établir la correction.

- Correction d'un crash de Starfield lors de l'activation de la Frame Generation avec le menu intégré.
  Le build de test initial a été confirmé fonctionnel en jeu, y compris FG désactivée/réactivée et ouverture du menu.
  La variante finale ajoute des corrections de durée de vie et de diagnostic et passe les tests automatisés Windows ; elle n'a pas reçu de nouvel essai en jeu.
- Limitation de la trace graphique facultative et suppression des exceptions de diagnostic lors du relevé des modules chargés.

### Remerciements

- [@mennogreg](https://github.com/mennogreg) pour les signalements, les logs et les nombreux essais en jeu sur Starfield ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), ainsi que le signalement de compatibilité OptiScaler et ses logs ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)).
- [@hunan-NRT](https://github.com/hunan-NRT), avec confirmation de @mennogreg, pour le signalement du problème de détection de Ray Reconstruction ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) pour le rapport Cyberpunk et les logs qui ont révélé le défaut d’isolation des journaux d’exception ([#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).

Merci également à toutes les personnes qui partagent leurs rapports et logs pour aider à analyser les problèmes restants.

## 1.0.0-beta.2 — préversion

Première version sous le nom **RTX Encore**. La numérotation repart à 1.0.0 : cette version succède à
`v1.4.5.3-rtx20-30-40`, publiée sous le nom DLSSG-Transfusion, et la remplace.

### Nouveautés

- **Menu et overlay intégrés.** Appuyez sur **Inser** dans le jeu : tous les réglages, l'état de chaque fonction, et
  un overlay avec la fréquence d'images, le multiplicateur, la régularité des images, le GPU et la VRAM. Il
  fonctionne dans les jeux DirectX 11, DirectX 12 et Vulkan. ReShade n'est plus nécessaire, et l'ancien add-on
  ReShade n'est plus fourni.
- **Neural Rendering** sur RTX 20, RTX 30 et RTX 40, appliqué à l'image produite par DLSS : une à quatre passes avec
  un style pour chacune, prise en charge du HDR et choix du moteur NVIDIA ou Open, expérimental. Expérimental,
  désactivé par défaut ; demande le fichier `nvngx_dlssnr.dll` 310.8.0 de NVIDIA. Open ne prend pas en charge RTX 20.
- **Nos travaux de performance NR pour Ampere (RTX 30).** Traitement plus rapide avec une sortie identique dans
  les tests locaux de référence du moteur NVIDIA ; précision Fast NVIDIA avec environ 17 % de temps NR en moins
  mesurés sur RTX 30 ; résolution NR indépendante, dont Ultra Performance ; précision Fast Open et options de
  vitesse/VRAM. Notre mode ultra-rapide **Ultra-fast mode** a réduit le temps NR de **18,7 à 9,8 ms (−48 %)**
  et porté les FPS affichés de **73,8 à 94,9 (+29 %)** dans le benchmark Wukong sur un RTX 3070 Ti Laptop avec FG X3,
  un passage par configuration. Une passe NR, défauts possibles en mouvement ; résultats non garantis.
  [Mesures et compromis](docs/NEURAL-RENDERING.fr.md).
- **Smooth Motion sur RTX 30** accepte maintenant les pilotes NVIDIA 617.42, 616.92 et 616.64 en plus du 617.14, et
  se règle depuis le menu. Seul le 617.14 a été vérifié en jeu.
- **Un seul fichier universel.** `rtx-encore.dll` accepte désormais dix-neuf noms de fichier, et peut donc cohabiter
  avec les mods qui utilisent déjà les noms habituels.
- **Nouvelles commandes** : une limite d'images calculées par NVIDIA Reflex, une demande de présentation sans
  V-Sync, et des cheveux en ray tracing compatibles dans The Witcher 3.
- **Panneau optionnel pour Cyber Engine Tweaks** dans Cyberpunk 2077 (expérimental).
- **Les journaux de session** sont conservés dans un dossier `rtx-encore-logs`, les trois plus récents par défaut.

### Changements

- Les fichiers s'appellent `rtx-encore.*`. Les réglages, les raccourcis et un ancien plugin sont repris
  automatiquement : voir
  [Mise à niveau depuis un ancien nom](docs/INSTALLATION.fr.md#mise-à-niveau-depuis-un-ancien-nom).
- Le fichier de réglages est `rtx-encore.jsonc`, organisé en sections, avec un commentaire sur chaque clé.
- Plusieurs réglages et entrées du menu ont un nom plus clair. Un fichier de réglages d'une version antérieure est
  repris avec ses valeurs.
- La génération d'images coûte 2 à 3 % de temps GPU en moins sur les cartes RTX 40.
- Le texte de l'overlay est plus compact, avec un réglage de taille.

### Limites connues

- La prise en charge des RTX 20 est expérimentale et n'a pas été validée sur une vraie carte RTX 20.
- Neural Rendering avec plusieurs passes, le moteur ouvert, le panneau Cyber Engine Tweaks et les cheveux en ray
  tracing sur RTX 20 et RTX 30 ont été peu ou pas testés en jeu.
- X5 et X6 sont expérimentaux et demandent beaucoup de VRAM.

## Versions antérieures

Les versions jusqu'à `v1.4.5.3-rtx20-30-40` ont été publiées sous le nom DLSSG-Transfusion. Elles ne sont plus distribuées.
