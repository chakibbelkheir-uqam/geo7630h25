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

