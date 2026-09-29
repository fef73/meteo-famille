# Comparateur Températures — Prévisions Météo Multisites

Application météo mono-fichier (HTML/CSS/JS, sans backend) permettant de comparer en direct les températures de plusieurs villes du monde, via l'API [Open-Meteo](https://open-meteo.com/) (gratuite, sans clé API).

## Sélection des villes

- Recherche de n'importe quelle ville au monde (géocodage Open-Meteo), plusieurs villes en simultané.
- Altitude précise optionnelle par ville (utile pour une station de ski, un point en montagne) — corrige la température prévue pour ce point exact.
- Une même ville peut être ajoutée à plusieurs altitudes différentes (le panneau de saisie reste ouvert après validation, pour enchaîner les ajouts).
- Villes retirables individuellement via les chips (bouton ×).

## Graphique de comparaison

- 5 périodes disponibles : aujourd'hui, demain, après-demain, 3 jours, 7 jours.
- Une courbe par ville, plus deux lignes de seuil en pointillés : canicule (35 °C) et gel (0 °C).
- Une croix (×) marque le pic de chaque courbe.
- Clic sur une ville dans la légende pour masquer/afficher sa courbe (le bulletin et les cartes se mettent à jour en conséquence).
- Tooltip au survol avec icône météo par ville.

## Cartes par ville

Pour chaque ville sélectionnée :

- Température actuelle (ou à midi pour demain/après-demain), icône météo, heure locale mise à jour en direct.
- Humidité, vent (vitesse + direction), min/max de la période.
- **Éphéméride** : arc solaire avec la position du soleil à l'heure locale de la ville (la nuit : « lever dans … »), lever et coucher, midi solaire, durée du jour et son évolution quotidienne, phase de lune (pourcentage éclairé) et date de la prochaine pleine lune. Sur « Demain » et « Après-demain », les heures de ce jour-là.
- **AQI moyen (7 jours)** : cliquable, affiché en rouge si supérieur à 60. Le clic déplie :
  - le détail par **polluant** — PM2.5, PM10, Ozone, NO₂, SO₂ — en rouge si le seuil santé OMS (moyenne 24h) est dépassé, avec description complète au survol de chaque puce ;
  - le détail par **pollen** — bouleau, graminées, olivier, ambroisie, aulne, armoise — en rouge au-delà d'un repère indicatif de risque allergique (données disponibles pour l'Europe uniquement).
- **Extrêmes sur 365 jours** (archives Open-Meteo, mesurées à l'emplacement exact de la ville) : nombre de jours de canicule (≥35 °C), de gel (≤0 °C), de vent fort (rafales ≥60 km/h) et de mauvaise qualité d'air (AQI ≥60).

## Bulletin de la période

- **Saint du jour** (calendrier des saints en France) sur la ligne du bulletin — ou celui de demain / après-demain selon la vue.
- Placé au-dessus des courbes, et terminé par une invitation « 👇 Fais défiler vers le bas pour plus d'infos » qui, touchée, descend jusqu'au graphique.
- **Résumé par ville** en tête : temps dominant de la journée (ensoleillé, éclaircies, nuageux, brouillard) ou phénomène notable (pluie, neige, orages, avec leur durée en heures, ou en jours sur 3 et 7 jours), températures min → max, et rafales quand elles dépassent 40 km/h. La région, le pays et l'altitude précisée sont rappelés à côté du nom.
- Ville la plus chaude et ville la plus fraîche pour la période affichée.
- Alertes automatiques si une ville dépasse le seuil canicule ou descend sous le seuil de gel.
- Historique de pollution des 7 derniers jours complets, par ville, dans son propre encadré sous les cartes (puces journalières, en évidence si AQI ≥60).

## Extrêmes mondiaux

- Deux bandeaux calculés en direct : le point le plus chaud et le point le plus froid du monde, parmi une sélection de lieux connus pour leurs extrêmes (déserts, régions polaires, stations records) — ce n'est pas une recherche exhaustive de toutes les villes du monde.

## Confort d'usage

- Interface bilingue FR/EN et unité °C/°F (préférences mémorisées).
- Lien de partage qui conserve la sélection de villes (bouton natif de partage ou copie du lien).
- Accès direct par URL avec coordonnées GPS, pour un usage en raccourci ou depuis une appli mobile :
  ```
  ?lat=45.9237&lon=6.8694&alt=1035&nom=Chamonix
  ```
  Plusieurs points d'un coup, séparés par `;` :
  ```
  ?points=45.9237,6.8694,1035,Chamonix;48.8566,2.3522,,Paris
  ```
- Position GPS sans nom (appli mobile, raccourci) : la **commune, la région et le pays** sont retrouvés automatiquement par géocodage inverse (Nominatim / OpenStreetMap). Si le service ne répond pas en 4 secondes, « Position GPS » est affiché.
- Depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/), le bouton **📍 Ma position** transmet la position du téléphone et le nom choisi.
- Rafraîchissement automatique des données toutes les 15 minutes.

## Hors connexion

- Après une première visite, la page s'ouvre sans réseau (service worker `sw.js`).
- La dernière réponse Open-Meteo reçue pour chaque vue est réaffichée, avec un bandeau « 📴 Hors connexion — données du 28/09, 05:26 ». Une vue jamais ouverte avec réseau (par exemple « 7 jours ») n'est pas disponible hors connexion.

## Sources de données

- Prévisions horaires et géocodage : `api.open-meteo.com`, `geocoding-api.open-meteo.com`
- Qualité de l'air (AQI, polluants, pollens) : `air-quality-api.open-meteo.com`
- Archives 365 jours (extrêmes) : `archive-api.open-meteo.com`
- Géocodage inverse (position GPS → commune) : `nominatim.openstreetmap.org`

## Notes techniques

- Fichier unique, aucune dépendance serveur — Chart.js chargé depuis un CDN pour le graphique.
- `sw.js` : service worker (page, Chart.js, polices et dernières réponses Open-Meteo gardés sur l'appareil pour l'usage hors connexion).
- Toutes les requêtes API sont mises en cache en mémoire (par ville/coordonnées) pour limiter les appels redondants.
- Le résumé des fonctionnalités ci-dessus est aussi affiché directement dans le site, dans un panneau repliable juste avant le pied de page.

## Contact

Un bug, une idée, une question ? Le lien **✉️ Contact / suggestion** en bas de chaque page ouvre un court formulaire, sans compte à créer : https://forms.gle/EMZtxMBJCUE6HJXp8

## Licence

© 2026 Fernand (fef73) — tous droits réservés. Voir le fichier [LICENSE](LICENSE). Les données météo restent soumises aux licences de leurs fournisseurs (Open-Meteo CC BY 4.0, INSEE / Etalab, OpenStreetMap ODbL).
