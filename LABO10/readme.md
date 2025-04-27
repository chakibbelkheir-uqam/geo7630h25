# 🛠️ Laboratoire 10 - GEO7630H25  
## Configuration de Geoserver et mise en place de services VTS et WFS

---

# Étape 1 : Configuration et lancement d’une instance de Geoserver

### Instructions :

1. 🌐 **Se connecter à GitHub** :
   - Vérifiez que vous êtes connecté à votre compte GitHub personnel.

2. 🍴 **Accéder au dépôt du cours** :
   - Rendez-vous sur le dépôt GitHub officiel du cours GEO7630H25.

3. 🔀 **Utiliser votre fork** :
   - Lancez un **Codespace** à partir de votre **fork** du dépôt,
   - Assurez-vous d'être sur la branche **`main`**.

4. 🚀 **Démarrer l'environnement** :
   - Le Codespace crée un environnement virtuel dans lequel vous pouvez :
     - Modifier du code,
     - Tester vos projets,
     - Démarrer des services cartographiques comme Geoserver.
    
       ![image](https://github.com/user-attachments/assets/d84bcfa4-c1f1-4c3f-8f2c-77e420f2068f)


🎯 Objectif :
- Configurer une instance de Geoserver en local,
- Préparer l’environnement pour publier et tester des services de type **VTS** (Vector Tile Services) et **WFS** (Web Feature Services).

---

✅ Cette étape pose la base technique pour publier et consommer vos données géospatiales directement depuis Geoserver dans un environnement contrôlé.

# Étape 2 : Configuration de l’environnement

Après avoir démarré le Codespace, il est nécessaire de configurer l’environnement pour lancer Geoserver correctement.

### Instructions détaillées :

1. 📄 **Copier le fichier `.env.example`** :
   - Localisez le fichier **`.env.example`** dans le dossier **Atlas**.

2. ✏️ **Renommer le fichier** :
   - Faites une copie et renommez le fichier en **`.env`** (supprimez simplement `.example`).

3. ⚙️ **Modifier les variables d’environnement** :
   - Ouvrez le fichier `.env` et configurez les variables suivantes avec vos informations personnelles :

```plaintext
DB_USER=CODEPERMANENT
DB_PASSWORD=VOTREMOTDEPASSE
DB_HOST=geo7630h25.cvwywmuc8u6v.us-east-1.rds.amazonaws.com
DB_NAME=geo7630
```
![image](https://github.com/user-attachments/assets/d36bacce-6dd1-49eb-9b89-2d451989940d)



Dans le dossier Atlas, faites un clic droit sur le fichier docker-compose.yml et sélectionnez Compose Up.

![image](https://github.com/user-attachments/assets/b52927d2-9ffe-4e1b-aaaf-c670a107b0a0)


# Étape 3 : Ajout de contrôles de carte dans MapLibre

Après avoir configuré l'environnement, nous allons enrichir notre carte avec plusieurs contrôles interactifs.

### Instructions :

1. 📂 Ouvrir le fichier :
   - Localisez et ouvrez le fichier **`/Atlas/app/app.js`** dans votre projet.

2. ➕ Ajouter les contrôles suivants :

### Contrôle de navigation (boussole + zoom + pitch) :

```javascript
var nav = new maplibregl.NavigationControl({
    showCompass: true,
    showZoom: true,
    visualizePitch: true
});
map.addControl(nav, 'top-right');

var geolocateControl = new maplibregl.GeolocateControl({
    positionOptions: { enableHighAccuracy: true },
    trackUserLocation: true
});
map.addControl(geolocateControl, 'bottom-right');

var scale = new maplibregl.ScaleControl({ unit: 'metric' });
map.addControl(scale);
```

![image](https://github.com/user-attachments/assets/419efa8b-5bae-4da3-b9af-0e8aa7dbf677)



## Étape 4 : Chargement de données depuis un serveur de tuiles vectorielles

### Objectif 🎯
Charger une couche de tuiles vectorielles (.pbf) issue d'un serveur `pg_tileserv` dans MapLibre.



---

### Explication 📚
Une source de tuiles vectorielles est définie par une URL suivant la structure :
où :
- **z** représente le niveau de zoom,
- **x** représente la coordonnée en X,
- **y** représente la coordonnée en Y.

⚡ **Attention** : La source doit impérativement être **déclarée avant** d'ajouter une couche qui l'utilise.

---

### Étapes d'intégration 🚀

#### 1. Accéder au serveur de tuiles
➔ Ouvrir l'interface d'administration du serveur de tuiles vectorielles 
![image](https://github.com/user-attachments/assets/167b1802-8bd7-4f31-abca-98d557cdc0e5)

![image](https://github.com/user-attachments/assets/983b03c3-6453-4ac9-b9d0-9a280734a936)

#### 2. Ajouter la source dans MapLibre

![image](https://github.com/user-attachments/assets/2eb5ddc3-ec2f-45f8-aa7d-a515ef20c3de)

## Étape 3 : Style avancé

- Appliquer un style basé sur une interpolation linéaire de la propriété `qt_arbres`.
- Utiliser l'expression suivante dans `paint` :

```javascript
'paint': {
    'fill-color': [
        'interpolate',
        ['linear'],
        ['get', 'qt_arbres'],
        0, 'rgb(255, 255, 255)',
        100, 'rgba(192, 192, 255, 0.64)',
        1000, 'rgba(46, 46, 255, 0.58)',
        5000, 'rgba(68, 0, 255, 0.66)',
        7000, 'rgba(19, 0, 70, 0.66)'
    ],
    'fill-opacity': 0.7
}
```

![image](https://github.com/user-attachments/assets/74c4fe45-e853-46d1-b222-272b3d9708b6)


## Étape 4 : Ajout d’une couche WFS

Ajout de la couche d'arrondissement 

![image](https://github.com/user-attachments/assets/3fd630d4-e165-4b73-98b4-f1518a863024)
