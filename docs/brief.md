# Brief : Pipeline d'estimation de frequentation touristique

## Objectif

Construire une pipeline qui produit un rapport d'estimation de la frequentation touristique d'une zone geographique ou d'une ville, en aggregant exclusivement des donnees librement accessibles.

## Use case principal

L'utilisateur selectionne une ville ou une zone geographique. La pipeline travaille sur cette zone et produit un rapport synthetisant tous les signaux de presence et d'attrait touristique disponibles en open data.

## Entree : selection de la zone

La zone est definie via OpenStreetMap (OSM). L'utilisateur fournit le nom d'une ville. La pipeline resout ce nom en un perimetre administratif precis (contour OSM) et utilise ce perimetre comme filtre spatial pour toutes les collectes de donnees.

## Donnees collectees

La pipeline recupere les donnees librement accessibles suivantes, restreintes au perimetre de la zone :

- **Data Tourisme** : donnees d'offre touristique francaise (hebergements, restaurants, patrimoine, evenements, activites). Source : API diffuseur Data Tourisme (open data, licence ouverte 2.0).
- **Pages Wikipedia des POI** : pour chaque point d'interet touristique identifie dans la zone via OSM (musees, monuments, sites historiques, attractions, etc.), recuperation du nombre de vues mensuelles sur l'annee glissante. Source : Wikimedia REST API (pageviews, open data, CC0).
- **Autres sources de presence** : toute source ouverte pouvant temoigner d'une frequentation reelle ou potentielle (ex. : donnees de transport en open data, capacites d'accueil, evenements recurrents).

OSM sert a la fois a delimiter la zone et a inventorier les POI touristiques qui serviront de pivot pour les autres sources.

## Traitement

La pipeline execute les etapes suivantes :

1. **Resolution** : geocodage du nom de ville en perimetre OSM exact.
2. **Extraction** : collecte des POI touristiques OSM dans le perimetre + collecte des donnees Data Tourisme dans la bounding box + collecte des vues Wikipedia pour les POI disposant d'un article.
3. **Enrichissement** : croisement des POI OSM avec leurs signaux de popularite Wikipedia.
4. **Agregation** : calcul d'indicateurs synthetiques (densite d'offre, notoriete des POI, score composite de frequentation estime).
5. **Rapport** : generation d'un document HTML autonome presentant les resultats.

## Output

Un rapport HTML autonome contenant :

- Une carte interactive des POI touristiques de la zone.
- Un tableau de bord avec les principaux indicateurs.
- Des graphiques synthetiques (repartition des categories de POI, vues Wikipedia, donnees d'offre).
- La methodologie et les sources utilisees.

## Contraintes

- Aucune cle API payante requise pour l'execution.
- Donnees exclusivement en open data ou librement accessibles.
- Respect des rate limits des APIs publiques.
- Code livrable propre, sans placeholders ni traces de developpement assiste.

## Livrable immediat

Ce brief enrichi sert de reference fonctionnelle pour l'implementation. Les specifications techniques et l'architecture seront definies a partir de ce document.
