# Journal des versions

*[English](CHANGELOG.md) · [中文](CHANGELOG.zh-CN.md)*

## 1.0.0

La première version stable de **RTX Encore**. Elle fait suite à `v1.4.5.3-rtx20-30-40`, publiée sous le nom DLSSG-Transfusion, et la remplace ; la numérotation repart de 1.0.0. Cette entrée rassemble tout ce qui a changé depuis, y compris ce qu’ont apporté les bêtas et les versions candidates.

### Nouveautés

- **Menu et overlay intégrés.** Appuyez sur **Inser** dans le jeu : tous les réglages, l’état de chaque fonction, et un overlay avec la fréquence d’images, le multiplicateur, le rythme d’affichage, le GPU, la VRAM et les versions de DLSS, DLSS-G et Streamline chargées. Fonctionne dans les jeux DirectX 11, DirectX 12 et Vulkan ; ReShade n’est plus nécessaire. **System → Component compatibility** liste les composants chargés, les fonctions qu’ils permettent et la raison d’un choix indisponible. Voir [Menu et overlay](docs/MENU-AND-OVERLAY.fr.md).
- **[RTX Encore Hub](https://github.com/SilyNoMeta/rtx-encore-hub)**, le compagnon Windows : il installe RTX Encore dans vos jeux avec les derniers fichiers DLSS et Streamline qualifiés et le fichier Neural Rendering de NVIDIA, met à jour tous les jeux en un clic, et peut toujours remettre un jeu comme avant. L’installation à la main fonctionne comme avant ; voir [Installation](docs/INSTALLATION.fr.md#le-plus-simple--rtx-encore-hub).
- **Neural Rendering** sur RTX 20, RTX 30 et RTX 40, appliqué à l’image produite par DLSS : une à quatre passes avec un style pour chacune, prise en charge du HDR, et le choix entre le moteur NVIDIA et le moteur Open expérimental. Désactivé par défaut ; nécessite le `nvngx_dlssnr.dll` 310.8.0 de NVIDIA. Open ne prend pas en charge les RTX 20.
- **Notre travail de performance NR pour Ampere (RTX 30) :** traitement plus rapide à résultat identique dans les tests de référence locaux du moteur NVIDIA ; précision Rapide NVIDIA avec environ 17 % de temps NR en moins mesuré sur RTX 30 ; résolution NR indépendante, Ultra Performance comprise ; précision Rapide d’Open et réglages facultatifs vitesse/VRAM. Notre **mode Ultra-rapide** a fait passer le temps NR de **18,7 à 9,8 ms (−48 %)** et les FPS affichés de **73,8 à 94,9 (+29 %)** dans le benchmark de Wukong sur une RTX 3070 Ti Laptop en FG X3, une mesure par configuration ; une passe NR, artefacts de mouvement possibles, résultats non garantis. Voir [Neural Rendering](docs/NEURAL-RENDERING.fr.md).
- **Plus de versions de Streamline.** Dynamic MFG natif et recomposition de l’interface avec les Streamline de production qualifiés 2.11.1, 2.12.0, 2.14.1 et 2.14.16 ; X2–X4 adaptatif d’Encore pour les anciens Streamline DirectX 12 qualifiés 2.7.2–2.10.3 ; prise en charge des fichiers que télécharge la bêta de l’application NVIDIA du 13 octobre 2026 (Streamline 2.14.16, DLSS-G 310.9.16). Vulkan garde son mode adaptatif ; DirectX 11 garde Smooth Motion X2. Voir [Compatibilité](docs/COMPATIBILITY.fr.md#versions-de-streamline).
- **Presets DLSS :** choix indépendants des presets Super Resolution, Ray Reconstruction et génération d’images, appliqués après un redémarrage, dont le preset de génération d’images **C (Lite)**, le mode performance plus rapide de NVIDIA, avec le DLSS-G 310.9.16.0 qualifié. A/B gardent Normal, et **Fast frame generation** reste indépendante du preset. Par défaut, les choix du jeu sont conservés.
- **Contrôle de la fréquence d’images :** une limite par NVIDIA Reflex avec saisie directe des FPS, **Use game settings** et **For this display** (une marge qui croît avec la fréquence de l’écran : 116 FPS à 120 Hz, 138 à 144 Hz, 224 à 240 Hz), une demande de présentation sans V-Sync, et des cheveux en ray tracing compatibles dans The Witcher 3.
- **Smooth Motion sur RTX 30**, réglé depuis le menu, accepte les pilotes NVIDIA 620.12, 617.42, 617.14, 616.92 et 616.64 (617.14 vérifié en jeu). **Force Smooth Motion** (désactivé par défaut) couvre les jeux Vulkan dont le profil NVIDIA le laisse éteint, et l’overlay et le menu signalent quand Smooth Motion ne tourne pas réellement. Voir [Smooth Motion](docs/SMOOTH-MOTION.fr.md).
- **Un seul fichier universel :** `rtx-encore.dll` accepte dix-neuf noms de fichier, pour cohabiter avec les mods qui utilisent déjà les noms habituels.
- **Panneau facultatif pour Cyber Engine Tweaks** dans Cyberpunk 2077 (expérimental).
- **`disableInGameMenu`** laisse le menu de côté, pour les jeux qui le refusent (Star Wars Outlaws) ; la génération d’images continue de fonctionner.
- **Diagnostics :** journaux de session dans un dossier `rtx-encore-logs` ; un relevé automatique du système, des versions des composants chargés, de la qualité et des résolutions DLSS, et des messages normaux de Streamline/NGX avec les motifs de refus ; des logs d’exception séparés pour chaque processus, avec le thread qui a terminé le processus. En option : `logPerformance` (temps d’image, température, puissance, fréquences et limites du GPU), `logGraphics`, `logStreamline`, `logNr`, `profileNrGpu`, `profileFrameGpu` (temps GPU de chaque passe DLSS sur DirectX 12), et pour les rapports de plantage `logDxgiWrappers` et `crashDetails` (détails mémoire et instantané `.dmp`). Voir [Paramètres](docs/SETTINGS.fr.md).
- Un avertissement quand l’ancien module dlssg_for_sm86 est encore installé à côté de RTX Encore : avec les deux actifs, la génération d’images peut ne présenter aucune image générée.

### Changements

- Les fichiers s’appellent `rtx-encore.*`. Réglages, raccourcis et un ancien plugin sont repris automatiquement : voir [Mise à niveau depuis un ancien nom](docs/INSTALLATION.fr.md#mise-à-niveau-depuis-un-ancien-nom).
- Le fichier de réglages est `rtx-encore.jsonc`, organisé en sections, avec un commentaire sur chaque clé ; plusieurs réglages et entrées du menu ont des noms plus clairs, et un ancien fichier de réglages est repris avec ses valeurs.
- La génération d’images coûte 2 à 3 % de temps GPU en moins sur les RTX 40.
- Le texte de l’overlay est plus compact, avec un réglage de taille ; quand les plugins Streamline du jeu sont plus récents que son cœur Streamline, l’overlay affiche les deux versions.
- L’ancien add-on ReShade n’est plus fourni.

### Corrections

- **Star Wars Outlaws** ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)) : le jeu complet (Ubisoft Connect) se ferme au lancement quand le menu y est attaché ; mettez `"disableInGameMenu": true`, ou appliquez le profil communautaire du jeu dans le Hub, et le jeu démarre avec la génération d’images jusqu’à X4, comme l’ont confirmé deux utilisateurs avec des builds de test. Corrigés aussi : un plantage quand les réglages vidéo affichent les options de génération d’images, et un plantage après un alt-tab depuis le menu principal.
- **Starfield :** un plantage à l’activation de la génération d’images avec le menu présent ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), un problème de fermeture avec Streamline 2.12 ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21)), et les changements de recomposition de l’interface ignorés pendant la génération d’images ([#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- **Forza Horizon 6** qui se fermait sur ses logos de démarrage avec le menu ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- **Final Fantasy VII Rebirth** qui masquait la génération d’images sur les RTX 20 et 30 ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- **Where Winds Meet** qui ne démarrait pas (0xc0000142) avec `version.dll` ou `winmm.dll` ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)) ; utilisez `version.dll` dans ce jeu. Corrigé aussi : un plantage du nom `dinput8.dll` avec l’overlay Epic.
- **Euro Truck Simulator 2 :** le menu n’apparaissait pas ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)) ; installez RTX Encore sous le nom `winmm.dll` dans ce jeu.
- Les problèmes de démarrage de **Cyberpunk 2077** liés au plugin Reflex lors des redémarrages de Streamline ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), et **The Witcher 3** avec la génération d’images dynamique ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4)) : confirmation en jeu en attente.
- Le plantage au démarrage de **The Last of Us** avec OptiScaler et l’ancien Streamline du jeu ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)) : confirmation en jeu en attente.
- Le menu qui refusait de s’attacher en DirectX 12 avec **Special K** ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- L’activité de DLSS **Ray Reconstruction** non détectée ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)) ; Neural Rendering signale maintenant quand son entrée de profondeur est indisponible.
- Les changements de limite Reflex appliqués en retard ou non restaurés selon les threads du jeu.
- Le premier lancement après une mise à jour depuis un ancien fichier de réglages, où certains réglages de démarrage ne prenaient effet qu’au deuxième lancement ; et l’outil de rapport de plantage d’un jeu qui écrivait ses propres journaux de session à la place des vrais.

### Limites connues

- La prise en charge des RTX 20 est expérimentale et n’a pas été validée sur une vraie carte RTX 20 ; un échec de génération d’images sur RTX 20 ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)) est encore à l’étude.
- Dans Star Wars Outlaws (jeu complet), le menu en jeu n’est pas disponible.
- Neural Rendering à plusieurs passes, le moteur Open, le panneau Cyber Engine Tweaks et les cheveux en ray tracing sur RTX 20 et RTX 30 ont été peu ou pas testés en jeu. X5 et X6 sont expérimentaux et demandent beaucoup de VRAM.
- Plusieurs corrections ci-dessus ont été confirmées avec des builds de test ou des essais locaux, pas avec ce build final ; quand l’auteur d’un signalement n’a pas refait l’essai, c’est indiqué. Les vérifications automatiques ne remplacent pas les tests de chaque jeu, carte et pilote.

Build Windows Release avec Open NR inclus, diagnostics par défaut et les six variantes de distribution, compilé et testé sur le PC du mainteneur : les tests de régression automatisés passent hors jeu. Le build final 1.0.0 n’a pas été lancé en jeu avant publication, et aucun gain de performance n’est revendiqué au-delà des mesures citées.

### Remerciements

- [@ivanmestre](https://github.com/ivanmestre) — le signalement Star Wars Outlaws, les logs, les tests d’isolation et de trace et les instantanés de plantage ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)).
- [@yannduwooz83](https://github.com/yannduwooz83) — les tests de Star Wars Outlaws et la comparaison avec le projet précédent ([#33](https://github.com/SilyNoMeta/rtx-encore/issues/33)), le signalement et les logs de Final Fantasy VII Rebirth ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@Avictuu](https://github.com/Avictuu) — les tests répétés de Starfield, les comparaisons de composants, les logs Reflex et de fermeture ([#21](https://github.com/SilyNoMeta/rtx-encore/issues/21), [#27](https://github.com/SilyNoMeta/rtx-encore/issues/27)).
- [@mennogreg](https://github.com/mennogreg) — les signalements, logs et tests répétés de Starfield ([#13](https://github.com/SilyNoMeta/rtx-encore/issues/13)), le signalement OptiScaler ([#20](https://github.com/SilyNoMeta/rtx-encore/issues/20)), la confirmation sur Ray Reconstruction ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@hunan-NRT](https://github.com/hunan-NRT) — le signalement Ray Reconstruction et les tests de Cyberpunk ([#14](https://github.com/SilyNoMeta/rtx-encore/issues/14)).
- [@naruto490610-alt](https://github.com/naruto490610-alt) — les signalements et logs de Cyberpunk ([#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)), le signalement Euro Truck Simulator 2 ([#22](https://github.com/SilyNoMeta/rtx-encore/issues/22)).
- [@wisecase2](https://github.com/wisecase2) — les logs de session de The Witcher 3 et les éléments sur Cyberpunk ([#4](https://github.com/SilyNoMeta/rtx-encore/issues/4), [#17](https://github.com/SilyNoMeta/rtx-encore/issues/17)).
- [@avikpal693](https://github.com/avikpal693) et [@wer2357666](https://github.com/wer2357666) — les signalements et logs de Forza Horizon 6 ([#11](https://github.com/SilyNoMeta/rtx-encore/issues/11), [#18](https://github.com/SilyNoMeta/rtx-encore/issues/18)).
- [@lovebabyx](https://github.com/lovebabyx) — les logs RTX 2070 Super qui ont montré ce qui manquait au journal de session ([#30](https://github.com/SilyNoMeta/rtx-encore/issues/30)).
- [@wd3dmlys](https://github.com/wd3dmlys) — la piste sur l’architecture GPU ([#12](https://github.com/SilyNoMeta/rtx-encore/issues/12)).
- [@ranmo-1](https://github.com/ranmo-1) — le signalement Where Winds Meet ([#9](https://github.com/SilyNoMeta/rtx-encore/issues/9)).
- [@PhantomGamers](https://github.com/PhantomGamers) — la formule de marge selon la fréquence de l’écran ([#23](https://github.com/SilyNoMeta/rtx-encore/issues/23)) et le signalement Special K ([#24](https://github.com/SilyNoMeta/rtx-encore/issues/24)).
- [@PrincessofCelery](https://github.com/PrincessofCelery) — les signalements qui ont amélioré le diagnostic des gels ([#19](https://github.com/SilyNoMeta/rtx-encore/issues/19)).
- [@CorwinMaximus](https://github.com/CorwinMaximus) — la demande du pilote 620.12 ([#35](https://github.com/SilyNoMeta/rtx-encore/issues/35)).
- [@hasnainb237](https://github.com/hasnainb237) — les logs Star Wars Zero Company qui ont montré l’ancien module resté en place ([#1](https://github.com/SilyNoMeta/rtx-encore/issues/1)).

Merci à toutes celles et tous ceux qui ont partagé leurs logs et testé les bêtas et les versions candidates.

## Versions antérieures

Les versions jusqu'à `v1.4.5.3-rtx20-30-40` ont été publiées sous le nom DLSSG-Transfusion. Elles ne sont plus distribuées.
