# Météo famille — Qui a le plus chaud ?

Application météo mono-fichier (HTML/CSS/JS, sans backend) qui compare en direct les températures là où vit chaque membre de la famille, via l'API [Open-Meteo](https://open-meteo.com/) (gratuite, sans clé API).

Site : https://fef73.github.io/meteo-famille/ — accessible aussi depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/).

## Famille

- Cinq lieux suivis, chacun associé à un ou plusieurs membres de la famille et à leur avatar :
  - Narbonne — Benjamin
  - Lyon — Audrey et Marine
  - Fontcouverte-la-Toussuire (1800 m) — Fernand
  - Marseille — Valentin
  - Clermont-Ferrand — Loulou
- L'avatar de chacun est placé sur le pic de sa courbe. Un clic sur un avatar, dans le graphique ou la légende, affiche son détail.

## Graphique de comparaison

- 5 périodes : aujourd'hui, demain, après-demain, 3 jours, 7 jours.
- Une courbe par lieu, plus deux lignes de seuil en pointillés : canicule (35 °C) et gel (0 °C).
- Clic sur un nom dans la légende pour masquer/afficher sa courbe. Le choix est conservé quand on change de période.
- Phase de lune affichée dans le tooltip.

## Cartes par personne

- **Repliables** : la température et l'icône restent visibles ; le reste s'ouvre avec « ▾ voir le détail ». Repliées par défaut, et le choix de chaque carte est mémorisé sur l'appareil.
- Température actuelle (ou à midi pour demain et après-demain), icône météo, heure locale en direct.
- Humidité, vent (vitesse + direction), heure du soleil au zénith, min/max de la période.
- **AQI moyen (7 jours)** : cliquable, en rouge au-delà de 60. Le clic déplie :
  - le détail par **polluant** (PM2.5, PM10, Ozone, NO₂, SO₂), en rouge si le seuil santé OMS sur 24 h est dépassé ;
  - le détail par **pollen** (bouleau, graminées, olivier, ambroisie, aulne, armoise), en rouge au-delà d'un repère de risque allergique.
- **Population exposée** sur la ligne AQI :
  - en France, bassin de l'intercommunalité (EPCI), données INSEE via `geo.api.gouv.fr` ;
  - hors France, population de la ville (Open-Meteo / GeoNames) ;
  - détail de la commune au survol.
- **Éphéméride** : arc solaire avec la position du soleil à l'heure locale de la ville (la nuit : « lever dans … »), lever et coucher, midi solaire, durée du jour et son évolution quotidienne, phase de lune (pourcentage éclairé) et date de la prochaine pleine lune. Sur « Demain » et « Après-demain », les heures de ce jour-là.
- **Extrêmes sur 365 jours**, à l'emplacement exact : nombre de jours de canicule (≥ 35 °C), de gel (≤ 0 °C), de vent fort (rafales ≥ 60 km/h) et de pollution (AQI ≥ 60).

## Bulletins

- **Tous les bulletins sont repliables** : on touche leur titre pour les ouvrir ou les fermer, choix mémorisé sur l'appareil. Le bulletin du jour et le bloc anniversaire sont ouverts par défaut ; les bulletins pollution, annuel et des extrêmes sont fermés par défaut.
- **Saint du jour** (calendrier des saints en France) sur la ligne du bulletin de la période — ou celui de demain / après-demain selon la vue.
- **Bulletin de la période**, placé au-dessus des courbes (suivi du bloc « 🎂 profil » quand un profil est renseigné), avec en tête un **résumé par ville** : temps dominant de la journée (ensoleillé, éclaircies, nuageux, brouillard) ou phénomène notable (pluie, neige, orages, avec leur durée en heures, ou en jours sur 3 et 7 jours), températures min → max, et rafales quand elles dépassent 40 km/h.
- Puis qui a le plus chaud et qui a le plus froid, avec alertes canicule et gel.
- **Bulletin annuel** : qui détient le record familial de jours de canicule, de gel ou de pollution sur les 365 derniers jours, avec alertes.
- **Bulletin pollution** (sous les cartes) des 7 derniers jours par personne, avec la population du bassin.

## Bulletin des extrêmes

Regroupés dans un bloc repliable « 🌍 Bulletin des extrêmes » (replié par défaut, choix mémorisé). Bandeaux calculés en direct, chacun parmi une sélection de lieux (ce n'est pas une recherche exhaustive) :

- point le plus chaud et le plus froid du **monde** (déserts, régions polaires, stations records) ;
- point le plus chaud et le plus froid d'**Europe** (grandes villes européennes) ;
- ville la plus chaude et la plus froide de **France** (grandes villes françaises) ;
- lieu le plus pollué (indice AQI) parmi de grandes villes du monde, d'Europe et de France.

## 📍 Mode GPS (« Moi »)

- Depuis le lanceur [comparateur-meteo.fr](https://comparateur-meteo.fr/), le bouton **📍 Ma position** ouvre le site avec la position du téléphone. Une **6e courbe** et une **carte « Moi »** s'ajoutent à la famille.
- Accès direct par URL, pour un raccourci ou une appli mobile :
  ```
  ?lat=45.5660&lon=5.9200&nom=Chambéry&alt=1035
  ```
  `lat` et `lon` sont obligatoires. `nom` (ou `ville`) donne le nom affiché, « Ma position » par défaut. `alt` (en m) est facultatif.
- **Avatar « Moi »** au choix : 📍 (par défaut), l'un des avatars de la famille, ou une **photo du téléphone** (recadrée au centre et réduite).
  - Se choisit en touchant l'avatar de la carte « Moi », ou dans le lanceur, qui le transmet dans le `#` de l'adresse (`#avatar=…`). Ce `#` n'est jamais envoyé au serveur.
  - Il est mémorisé sur l'appareil, sans rien envoyer en ligne.

## 🎂 Mon profil

Renseigné dans le lanceur (pseudo, date, heure et ville de naissance), transmis dans le `#` de l'adresse puis gardé sur l'appareil. Le `#` est retiré de la barre d'adresse dès la lecture, pour ne pas être partagé par erreur.

- La carte « Moi » porte le **pseudo** (🎂 devant le jour de l'anniversaire).
- **Anniversaire** : compte à rebours, âge atteint, **prévision du jour J** dès 15 jours avant (à ta position, sinon à ta ville natale), **confettis** le jour même.
- **Jours de vie** et prochain millier (« 24 000 jours le … »), **saint** de ton anniversaire.
- **🔭 Carte du ciel de ta naissance**, au-dessus de la ville natale, à l'heure de naissance (22 h si elle n'est pas renseignée) : environ 700 étoiles, tracés et noms des constellations, étoiles les plus brillantes, planètes visibles à l'œil nu et Lune, avec une phrase de résumé (jour, crépuscule ou nuit, planètes au-dessus de l'horizon). Tout est calculé sur le téléphone, sans service en ligne : la carte fonctionne hors connexion. Précision : planètes à 0,1° près, Lune à environ 1°.
- **✨ Thème astral** : Soleil, **ascendant** et milieu du ciel (avec l'heure de naissance), Lune et planètes dans leur signe, au degré près, puis un court portrait pour le signe solaire, l'ascendant et le signe lunaire. Présenté comme la tradition astrologique, pour le plaisir : l'astrologie n'a pas de fondement scientifique, les positions calculées, elles, sont réelles.
- **🔮 Horoscope du jour**, juste sous le thème astral : pour ton signe solaire, avec la **Lune du jour** (sa position réelle, calculée sur le téléphone), trois rubriques notées de 2 à 5 étoiles (cœur, projets, énergie), un chiffre et une couleur porte-bonheur, et un conseil. Le texte est composé sur l'appareil, sans service en ligne : il change chaque jour et reste identique toute la journée. Il s'affiche dès que la date de naissance est renseignée (l'heure et la ville ne sont pas nécessaires). Pour le plaisir, sans valeur scientifique.
- **Le jour de ta naissance** : météo à la ville natale (archives Open-Meteo / ERA5, depuis 1940) — temps, min → max, température à l'heure de naissance, pluie, neige, rafales —, lever et coucher du soleil, durée du jour, phase de lune, **constellation où se trouvait le Soleil** (limites IAU, y compris le Serpentaire) et signe astrologique.

## 📴 Hors connexion

- Après une première visite avec réseau, le site s'ouvre sans réseau (service worker `sw.js`).
- La dernière réponse Open-Meteo reçue pour chaque vue est réaffichée, avec un bandeau « 📴 Hors connexion — données du 28/09, 05:26 ».
- Une vue jamais ouverte avec réseau (par exemple « 7 jours ») n'est pas disponible hors connexion.

## Confort d'usage

- Interface bilingue FR/EN et unité °C/°F (préférences mémorisées).
- Bouton de partage (partage natif ou copie du lien).
- Rafraîchissement automatique : prévisions toutes les 15 minutes, extrêmes du moment toutes les 30 minutes, et bouton **↻ Actualiser**.
- Le résumé des fonctionnalités est aussi affiché dans le site, dans un panneau repliable juste avant le pied de page.

## Sources de données

- Prévisions horaires : `api.open-meteo.com`
- Qualité de l'air (AQI, polluants, pollens) : `air-quality-api.open-meteo.com`
- Archives 365 jours (extrêmes) : `archive-api.open-meteo.com`
- Population hors France : `geocoding-api.open-meteo.com` (GeoNames)
- Population en France : `geo.api.gouv.fr` (INSEE)

## Notes techniques

- Fichier unique, aucune dépendance serveur. Chart.js est chargé depuis un CDN pour le graphique, et les avatars sont intégrés à la page.
- `sw.js` : service worker (page, Chart.js, polices et dernières réponses Open-Meteo gardés sur l'appareil pour l'usage hors connexion).
- Les statistiques sur 365 jours sont calculées une fois au chargement, puis gardées en mémoire.
- Statistiques de visite anonymes et sans cookie avec GoatCounter.

## Contact

Un bug, une idée, une question ? Le lien **✉️ Contact / suggestion** en bas de chaque page ouvre un court formulaire, sans compte à créer : https://forms.gle/EMZtxMBJCUE6HJXp8

## Licence

© 2026 Fernand (fef73) — tous droits réservés. Voir le fichier [LICENSE](LICENSE). Étoiles et constellations : [d3-celestial](https://github.com/ofrohn/d3-celestial) (BSD-3-Clause, d'après Hipparcos). Les données météo restent soumises aux licences de leurs fournisseurs (Open-Meteo CC BY 4.0, INSEE / Etalab). Les avatars de la famille ne peuvent pas être réutilisés.
