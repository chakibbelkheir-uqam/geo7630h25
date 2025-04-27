# Cartographie interactive de Montréal avec MapLibreGL

## 🎯 Objectif
Élaborer une carte interactive de Montréal intégrant :
- La densité d'arbres par quartier (polygones colorés)
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
