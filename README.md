# Lanceur météo — comparateur-meteo.fr

Page d'accueil mono-fichier (HTML/CSS/JS, sans backend) qui regroupe les quatre outils météo de fef73. Elle s'installe comme une appli sur smartphone et peut transmettre la position GPS du téléphone à chacun des sites.

Site : https://comparateur-meteo.fr/

## Les quatre outils

| Carte | Site | Rôle |
|---|---|---|
| Prévisions Météo famille | [meteo-famille](https://fef73.github.io/meteo-famille/) | Aujourd'hui → 7 jours, là où vit chaque membre de la famille |
| Prévisions Météo villes | [comparateur-temperatures](https://fef73.github.io/comparateur-temperatures/) | Comparateur de températures entre villes, même en altitude |
| Historique Météo ville | [evolution-temperatures](https://fef73.github.io/evolution-temperatures/) | Évolution des températures d'une ville depuis 1940 |
| Historique Météo neige | [meteo-neige](https://fef73.github.io/meteo-neige/) | Chutes de neige et neige restante au sol |

## 📍 Ma position (GPS)

- Le bouton **📍 Ma position** récupère les coordonnées GPS du téléphone (API Geolocation du navigateur, avec l'autorisation de l'utilisateur, en HTTPS).
- La commune est retrouvée automatiquement par géocodage inverse ([Nominatim / OpenStreetMap](https://nominatim.openstreetmap.org/), sans clé).
- Une zone **Nom**, pré-remplie avec la commune, permet d'afficher un autre nom (« Chalet », « Maison »…).
- Les coordonnées et le nom sont ajoutés aux liens des quatre cartes :
  ```
  ?lat=45.5660&lon=5.9200&nom=Chambéry&ville=Chambéry
  ```
  `nom` est lu par meteo-famille, comparateur-temperatures et meteo-neige, et `ville` par evolution-temperatures.
- La position et le nom sont mémorisés sur l'appareil. Le bouton **✕** les efface et remet les liens d'origine.

## 🎂 Mon profil

- Zone repliable, facultative : **pseudo**, **date de naissance**, **heure** et **ville de naissance** (recherche via le géocodage Open-Meteo).
- Gardé uniquement dans ce navigateur, et transmis à **meteo-famille** dans le `#` du lien (`#pseudo=…&naissance=…&heure=…&lieu=lat,lon,Nom`), jamais envoyé à un serveur. Sans profil, le lien porte `#profil=0`, qui l'efface aussi côté météo famille.
- Bouton **Effacer mon profil**.

## Avatar personnel

- Toucher l'avatar en haut à gauche ouvre un choix : avatar d'origine, 📍, ou **photo du téléphone** (recadrée au centre, réduite en 96×96).
- L'avatar choisi remplace celui de l'en-tête et est mémorisé sur l'appareil.
- En mode GPS, il est transmis à **meteo-famille** et devient celui de la carte et de la courbe « Moi ». Il passe dans le `#` de l'adresse (`#avatar=…`), qui n'est jamais envoyé au serveur.

## Appli et hors connexion

- Installable sur l'écran d'accueil (manifest, icônes 192 / 512 / maskable).
- Service worker (`sw.js`) : réseau d'abord, copie locale si hors connexion. Le lanceur s'ouvre donc sans réseau.
- Les quatre sites ont aussi leur propre service worker, et restent utilisables sans réseau après une première visite :
  - historiques ville et neige : les jours déjà consultés sont gardés dans le téléphone (IndexedDB), avec une sauvegarde et une restauration en fichier JSON ;
  - météo famille et comparateur : la dernière prévision reçue est réaffichée, avec un bandeau « 📴 Hors connexion — données du … ».

## Confort d'usage

- Interface bilingue FR/EN (préférence mémorisée).
- Le résumé des fonctionnalités est aussi affiché dans le site, dans un panneau repliable juste avant le pied de page.
- Balises de partage (Open Graph / Twitter) et données structurées pour les moteurs de recherche.
- Statistiques de visite anonymes et sans cookie avec GoatCounter.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | La page du lanceur |
| `sw.js` | Service worker (hors connexion) |
| `manifest.json`, `icon-*.png` | Installation comme appli |
| `og-image.png` | Image de partage |
| `sitemap.xml`, `CNAME` | Référencement et domaine `comparateur-meteo.fr` |

## Contact

Un bug, une idée, une question ? Le lien **✉️ Contact / suggestion** en bas de chaque page ouvre un court formulaire, sans compte à créer : https://forms.gle/EMZtxMBJCUE6HJXp8

## Licence

© 2026 Fernand (fef73) — tous droits réservés. Voir le fichier [LICENSE](LICENSE). Géocodage inverse : © contributeurs OpenStreetMap (ODbL).
