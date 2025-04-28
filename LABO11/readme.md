# Cartographie interactive de Montréal avec MapLibreGL

## 🎯 Objectif
Élaborer une carte interactive de Montréal intégrant :
- Les commerces
- Les contours administratifs des arrondissements

---

## 🛠️ Préparation de l'environnement

1. **Créer un fichier `index.html`** pour intégrer la carte MapLibreGL et les boutons de chargement WFS.
   
 ![image](https://github.com/user-attachments/assets/530722be-1710-4047-9a7d-bed44efd2f56)

 
  
2. **Créer un fichier `app.js`** pour configurer les couches, les contrôles et les interactions.

   
3. **Configurer un Docker Compose** pour utiliser pg_tileserv et pg_featureserv si nécessaire.

![image](https://github.com/user-attachments/assets/be0c6bba-1e27-4fba-85d7-84e9c8fa3faf)

---

## 🗺️ Création de la carte MapLibreGL

- Fond de carte : MapTiler (clé API gratuite utilisée)
- Centre : **Montréal** `[Longitude : -73.55, Latitude : 45.55]`
- Zoom initial : `9`

Ajout des contrôles :
- **Navigation** (Zoom + Boussole)
- **Échelle**
- **Hash** pour garder l'état de la carte dans l'URL

```javascript
var map = new maplibregl.Map({
    container: 'map',
    style: 'https://api.maptiler.com/maps/dataviz/style.json?key=JhO9AmIPH59xnAn5GiSj',
    center: [-73.55, 45.55],
    zoom: 9,
    hash: true
});

map.addControl(new maplibregl.NavigationControl(), 'top-right');
map.addControl(new maplibregl.ScaleControl({ unit: 'metric' }));
```

## Ajout des couches de données
Créer le fichier : map-layers.js Créer un fichier map-layers.js et créer les layers sous forme de variable objet en commençant par la couche des commerces de Montréal.

Source GeoJSON dynamique depuis Montréal Source GeoJSON via données ouvertes ou pgfeatureserv ou pgtileserv ex : https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson

Aussi, avec le style de la couche comme variable de couche.

    var commercesSource = {
type: 'geojson', data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson' };

var commercesLayer = { id: 'commerces', type: 'circle', source: 'commerces_source', paint: { 'circle-radius': [ 'match', ['get', 'type'], 'Épicerie', 5, 'Pâtisserie/Boulangerie', 7, 'Distributrice automatique', 4, 'Pharmacie', 6, 'Restaurant', 5, 3 ], 'circle-color': [ 'match', ['get', 'type'], 'Épicerie', 'orange', 'Pâtisserie/Boulangerie', 'yellow', 'Distributrice automatique', 'blue', 'Pharmacie', 'green', 'Restaurant', 'purple', 'grey' ], 'circle-stroke-color': '#fff', 'circle-stroke-width': 1 }, filter: ['==', ['get', 'statut'], 'Ouvert'] };

Filtrage pour ne garder que ceux au statut "Ouvert" ajouté le à la suite du "paint" configuration sur la dernière lihne de ce code.

![image](https://github.com/user-attachments/assets/93a2dc7b-d926-4d7b-b5a9-47b662b8a778)

## Chargement des couches dans la carte

Fichier : app.js Créer un fichier app.js et injecter les layers précédement créer dans le map-layers.js Ajout des sources et des couches : commerces_source → commerces arrondissements-source → arrondissements, arrondissements-labels
```
    map.on('load', function () {
// Ajouter sources map.addSource('commerces_source', { type: 'geojson', data: 'https://donnees.montreal.ca/dataset/c1d65779-d3cb-44e8-af0a-b9f2c5f7766d/resource/ece728c7-6f2d-4a51-a36d-21cd70e0ddc7/download/businesses.geojson' }); map.addSource('arrondissements_source', { type: 'geojson', data: 'https://donnees.montreal.ca/dataset/9797a946-9da8-41ec-8815-f6b276dec7e9/resource/e18bfd07-edc8-4ce8-8a5a-3b617662a794/download/limites-administratives-agglomeration.geojson' });

// Ajouter couches map.addLayer({ id: 'commerces', type: 'circle', source: 'commerces_source', paint: { 'circle-radius': [ 'match', ['get', 'type'], 'Épicerie', 5, 'Pâtisserie/Boulangerie', 7, 'Distributrice automatique', 4, 'Pharmacie', 6, 'Restaurant', 5, 3 ], 'circle-color': [ 'match', ['get', 'type'], 'Épicerie', 'orange', 'Pâtisserie/Boulangerie', 'yellow', 'Distributrice automatique', 'blue', 'Pharmacie', 'green', 'Restaurant', 'purple', 'grey' ], 'circle-stroke-color': '#fff', 'circle-stroke-width': 1 }, filter: ['==', ['get', 'statut'], 'Ouvert'] }); map.addLayer(arrondissementsLayer); map.addLayer({ id: 'arrondissements-labels', type: 'symbol', source: 'arrondissements_source', layout: { 'text-field': ['get', 'NOM'], 'text-font': ['Open Sans Bold', 'Arial Unicode MS Bold'], 'text-size': 14, 'text-anchor': 'center' }, paint: { 'text-color': '#111', 'text-halo-color': '#fff', 'text-halo-width': 1.5 }, });

});
```

![image](https://github.com/user-attachments/assets/f466a3a7-8079-43a9-98e8-649ac5fd70dd)

## Ajout des interactions souris
Fichier : mouse-controls.js Créer un fichier mouse-controls.js et injecter les controleurs de souris Survol (mouseenter / mouseleave) : changement du curseur Clic sur un commerce : Affiche une popup (nom + type) Effectue un zoom et un recentrage (flyTo)

Voici le code ayant permis de le réaliser ; map.on('mouseenter', 'commerces', () => { // Change le curseur en pointeur (main) pour indiquer que l'élément est interactif map.getCanvas().style.cursor = 'pointer'; });

// Quand la souris quitte la couche "commerces" : on remet le curseur par défaut map.on('mouseleave', 'commerces', () => { map.getCanvas().style.cursor = ''; });

// Événement au clic sur un commerce map.on('click', 'commerces', (e) => { // On récupère la première entité (feature) cliquée const feature = e.features[0];

// Extraction des coordonnées du point (on fait une copie avec slice())
const coordinates = feature.geometry.coordinates.slice();

// Récupération de propriétés pour alimenter la popup
const name = feature.properties.name;
const description = feature.properties.type;

// Création et affichage d'une popup HTML à la position cliquée
new maplibregl.Popup()
    .setLngLat(coordinates)
    .setHTML(`<strong>${name}</strong><br>${description}`)
    .addTo(map);

// Zoom et centrage automatique vers le point sélectionné
map.flyTo({ center: coordinates, zoom: 14 }); // JumpTo
});

![image](https://github.com/user-attachments/assets/691d0366-da57-416a-99d1-6ac6bc04cf82)


