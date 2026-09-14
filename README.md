# Météo Famille — Qui a le plus chaud ?

Application météo mono-fichier (HTML/CSS/JS, sans backend) qui compare en direct les températures là où vivent les membres de la famille, via l'API [Open-Meteo](https://open-meteo.com/) (gratuite, sans clé API).

## Famille

- Suivi de plusieurs membres de la famille, chacun associé à sa ville et à son avatar (photo circulaire).
- Clic sur un avatar dans le graphique ou dans la légende pour afficher son détail (tooltip dédié à cette personne).

## Graphique de comparaison

- 5 périodes disponibles : aujourd'hui, demain, après-demain, 3 jours, 7 jours.
- Une courbe par personne, plus les seuils canicule et gel en pointillés.
- Clic sur un nom dans la légende pour masquer/afficher sa courbe.
- Phase de lune affichée dans le tooltip du graphique, en plus de l'heure et de la température.

## Cartes par personne

Pour chaque membre de la famille :

- Température actuelle (ou à midi pour demain/après-demain), icône météo, heure locale mise à jour en direct.
- Humidité, vent (vitesse + direction), heure du soleil au plus haut, min/max de la période.
- **AQI moyen (7 jours)** : cliquable, affiché en rouge si supérieur à 60. Le clic déplie :
  - le détail par **polluant** — PM2.5, PM10, Ozone, NO₂, SO₂ — en rouge si le seuil santé OMS (moyenne 24h) est dépassé, avec description complète au survol de chaque puce ;
  - le détail par **pollen** — bouleau, graminées, olivier, ambroisie, aulne, armoise — en rouge au-delà d'un repère indicatif de risque allergique (données disponibles pour l'Europe uniquement).
- **Extrêmes sur 365 jours** (archives Open-Meteo, mesurées à l'emplacement exact de la ville) : nombre de jours de canicule, de gel, de vent fort et de mauvaise qualité d'air.

## Bulletins

- **Bulletin de la période** : qui a le plus chaud et qui a le plus froid en ce moment (ou pour la période affichée), avec alertes automatiques canicule/gel.
- **Bulletin annuel** : qui, dans la famille, bat le record de jours de canicule, de gel ou de pollution sur les 365 derniers jours, avec alertes si un seuil est franchi.
- Historique de pollution des 7 derniers jours complets, par personne (puces journalières, en évidence si AQI ≥60).

## Confort d'usage

- Interface bilingue FR/EN et unité °C/°F (préférences mémorisées).
- Bouton de partage (partage natif ou copie de lien).
- Rafraîchissement automatique des données.
- Message d'astuce rappelant que l'on peut cliquer sur les noms/seuils de la légende pour personnaliser l'affichage.

## Sources de données

- Prévisions horaires : `api.open-meteo.com`
- Qualité de l'air (AQI, polluants, pollens) : `air-quality-api.open-meteo.com`
- Archives 365 jours (extrêmes) : `archive-api.open-meteo.com`

## Notes techniques

- Fichier unique, aucune dépendance serveur — Chart.js chargé depuis un CDN pour le graphique.
- Avatars intégrés en base64 directement dans le HTML (rognage circulaire, bordure colorée par ville).
- Toutes les requêtes API sont mises en cache en mémoire (par ville) pour limiter les appels redondants.
- Le résumé des fonctionnalités ci-dessus est aussi affiché directement dans le site, dans un panneau repliable juste avant le pied de page.
