# DataBotch — Brief hackaton

**Use case principal.** L'utilisateur sélectionne une zone géographique (n'importe quel contour). Les agents collectent les données OSM de cette zone et estiment le nombre de touristes. Les données Accor servent uniquement à valider l'ordre de grandeur sur les zones où elles sont disponibles.

**Méthode.** Pipeline en 4 étapes : découvrir les données OSM de la zone, extraire, standardiser, estimer la fréquentation. Chaque étape est un script déterministe. Kimi intervient pour la recherche de tags OSM pertinents, le choix de la méthode d'estimation, et la rédaction du rapport.

**Pistes méthodologiques pour estimer les touristes.**

| Méthode | Ce qu'on compte dans OSM | Calcul | Avantage | Inconvénient |
|---|---|---|---|---|
| Capacité hôtelière | Hôtels, campings, auberges, locations saisonnières | Lits × taux d'occupation moyen × durée de séjour | Direct, fiable si on a les capacités | Nécessite de deviner le taux d'occupation |
| Attractivité POI | Musées, monuments, parcs, plages, sites classés | POI × visiteurs estimés par type de POI | Fonctionne même sans hébergement | Très approximatif, dépend du type de POI |
| Densité de services | Restaurants, cafés, bars, commerces touristiques | Corrélation avec la fréquentation observée ailleurs | Indicateur indirect robuste | Nécessite un étalonnage sur des zones connues |
| Parkings | Capacité des parkings (publics, supermarchés, centres commerciaux) | Places × taux de rotation × part touristes | Proxy de trafic réel | Difficile à isoler le touriste du local |

**Combinaison recommandée.** Méthode capacitaire comme base principale. Méthode POI comme complément quand les hébergements sont rares. Données Accor pour calibrer les hypothèses (taux d'occupation, visiteurs par POI) sur les villes couvertes.

**Données OSM à collecter.**
- Hébergements : `tourism=hotel`, `tourism=camp_site`, `tourism=hostel`, `tourism=apartment`, `tourism=guest_house`
- Capacités : `capacity`, `rooms`, `beds`
- POI touristiques : `tourism=attraction`, `tourism=museum`, `historic=monument`, `leisure=park`, `natural=beach`
- Services : `amenity=restaurant`, `amenity=cafe`, `amenity=bar`, `shop=souvenir`
- Transport : `amenity=parking` (capacité)

**Données Accor.** Utilisées en post-traitement pour valider. Si la zone est dans les 700 villes Accor, on compare l'estimation OSM aux nuitées réelles. Si l'écart est grand, on l'explique (tourisme de passage non capturé par les hébergements, saisonnalité, etc.).

**Livrable.** Un rapport pour la zone sélectionnée, lisible sans compétence technique, qui contient :
- la méthode retenue en deux phrases
- le décompte OSM par catégorie (hébergements, POI, services)
- l'estimation du nombre de touristes avec les hypothèses clés
- la comparaison avec Accor si disponible
- les limites de l'analyse

Format au plus simple : HTML autonome ou PDF. Pas de serveur.

**À trancher ensemble.**
1. Quelle méthode d'estimation privilégie-t-on ? Capacitaire seule, ou combinaison ?
2. Comment l'utilisateur fournit la zone ? Adresse + rayon, contour GeoJSON, sélection sur carte ?
3. Quel est le périmètre des 700 villes Accor ? Toute la France ou principalement les grandes villes ?
4. Qui fait quoi dans l'équipe ?

**Principe.** Robuste avant beau. Une seule zone traitée de bout en bout vaut mieux qu'une carte de France inachevée.
