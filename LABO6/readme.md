# 🛠️ Laboratoire 6 et 7 : ArcGIS Online, Dashboard et Experience Builder – Intégration de données

Ce laboratoire a pour objectif de :

- 🌐 **Maîtriser l'intégration de données** dans **ArcGIS Online (AGOL)**.
- 📊 **Exploiter les données** via **ArcGIS Dashboard** et **Experience Builder**.
- 🔄 **Automatiser les mises à jour** de données en utilisant **FME**.

---

# 1. Intégration de données dans ArcGIS Online

## a. Préparation des données avec FME

Avant d'intégrer les données sur AGOL, il est nécessaire de les préparer avec FME :

- 📄 **Ouvrir le fichier CSV** des **stations Bixi de Montréal** via un **Reader CSV** dans FME.

  ![image](https://github.com/user-attachments/assets/b1490f74-f9f0-4bc3-ba92-39260c99e308)


🎯 Objectif :
- Importer correctement les données tabulaires,
- Les transformer au besoin pour assurer une intégration optimale dans ArcGIS Online.

---

✅ Cette première étape est cruciale pour garantir que les données sont prêtes à être publiées et utilisées dans des tableaux de bord interactifs et des applications web via Experience Builder.

## b. Filtrage des données

Après l'importation du fichier CSV :

- 🧹 **Filtrer les données** pour exclure les enregistrements incorrects.

🛠️ Utiliser le transformateur **AttributeFilter** dans FME pour :

![image](https://github.com/user-attachments/assets/dca24db3-86da-44cd-a6af-a67108cd6447) ![image](https://github.com/user-attachments/assets/dca15fec-9f10-463f-9147-21b471f10afc)



- **Exclure** toutes les lignes :
  - N'ayant pas de valeur de **longitude** (`NULL` ou vide).
  - Dont la **longitude** ou la **latitude** est égale à **-1** (mauvaises coordonnées).

🎯 Objectif :
- Nettoyer les données pour garantir que seules les stations Bixi valides seront intégrées dans ArcGIS Online.

---

✅ Ce filtrage évite d'importer des points erronés dans AGOL, assurant une qualité de données optimale pour le tableau de bord et l'application web.

## c. Calcul des statistiques sur les données Bixis

Après avoir nettoyé les données, nous allons :

- 📈 **Analyser le flux d'entrée et de sortie** des vélos par station.

🛠️ Dans cette étape :

- Calculer le **nombre de départs** (flux de sortie) par station.
- Utiliser des outils tels que :
  - **StatisticsCalculator** ou **GroupBy Aggregator** dans FME,
  - En regroupant les données par **ID de station** ou **nom de station**.
 
    ![image](https://github.com/user-attachments/assets/beb66f82-e580-49df-ab06-cc4fd9554be3)

    ![image](https://github.com/user-attachments/assets/076cb827-1488-445c-9d8f-9b9e885f2dad)


🎯 Objectif :
- Obtenir le **nombre total de départs** pour chaque station,
- Fournir une statistique clé pour la future intégration dans le **Dashboard ArcGIS**.

---

✅ Ces statistiques permettront de visualiser directement sur la carte et dans les tableaux de bord :
- Les stations les plus actives pour les départs de vélos.


## d. Nettoyage et gestion des attributs

Avant d'intégrer les données dans ArcGIS Online, il est essentiel de simplifier et de normaliser les champs :

- 🧹 **Utiliser le transformateur AttributeManager** dans FME pour :

  ![image](https://github.com/user-attachments/assets/c3db4c00-022c-4b6e-af04-697447c48102)


### Actions à réaliser :

- **Supprimer les attributs inutiles** provenant du fichier CSV initial (ceux qui ne sont pas pertinents pour l'analyse ou l'affichage).
- **Renommer les champs** restants pour :
  - Simplifier leur lecture,
  - Garantir leur compatibilité avec les standards d'intégration AGOL (éviter les espaces, caractères spéciaux, etc.).

🎯 Objectif :
- Faciliter l'intégration et la publication sur ArcGIS Online,
- Assurer la clarté des données dans les Dashboards et applications Experience Builder.

---

✅ Cette étape garantit un modèle de données propre, optimisé pour une intégration sans erreurs dans l'écosystème AGOL.

## e. Jointure des données via FeatureJoiner

Pour garantir la localisation correcte et enrichir les informations des stations Bixi :

- 🔗 **Utiliser le transformateur FeatureJoiner** dans FME pour :

  ![image](https://github.com/user-attachments/assets/9c3a3292-d43a-4f70-8231-6c63976dd000)


### Actions à réaliser :

- **Joindre** :
  - Les coordonnées des stations,
  - Aux statistiques de départs par station.

- **Configurer la clé de jointure** :
  - Utiliser l'**identifiant unique des stations** (généralement un champ ID ou PK - Primary Key).
  
- **Type de jointure** :
  - Sélectionner une **jointure de type "Full"** pour conserver toutes les stations, même si certaines n'ont pas de données de départ.

🎯 Objectif :
- Associer chaque station à son flux de départ tout en préservant l'information spatiale,
- Préparer des données complètes et prêtes à être chargées dans ArcGIS Online.

---

✅ Cette étape assure que chaque station est correctement localisée et enrichie avant son intégration dans les Dashboards et les applications Experience Builder.

## f. Jointure des arrivées par station

Après avoir joint les données de départs :

- ➡️ **Ajouter un second FeatureJoiner** directement enchaîné sur le port **"Joined"** du premier FeatureJoiner.

![image](https://github.com/user-attachments/assets/fef59f5b-18f2-494b-9e31-db6d42d441a0)

### Actions à réaliser :

- **Joindre** :
  - Les statistiques d'**arrivées** par station,
  - Aux stations enrichies déjà avec les statistiques de départs.

- **Configurer la clé de jointure** :
  - Utiliser à nouveau l'**identifiant unique des stations** (Primary Key).

- **Type de jointure** :
  - Utiliser également une **jointure de type "Full"**, pour garantir que toutes les stations soient conservées, même celles sans données d'arrivée.

🎯 Objectif :
- Finaliser l'enrichissement des stations Bixi avec à la fois les **données de départ** et **d'arrivée**,
- Créer une table complète prête à être utilisée dans ArcGIS Online pour des analyses plus avancées.

---

✅ Cette étape garantit que chaque station dispose de toutes les informations nécessaires pour alimenter les dashboards et applications Experience Builder.

## g. Nettoyage final des données

Pour finaliser le traitement des stations Bixi enrichies :

- 🧹 **Utiliser à nouveau un AttributeManager** dans FME pour :

  ![image](https://github.com/user-attachments/assets/d64b989b-9bcb-471e-a1ad-89c98c9bbd14)


### Actions à réaliser :

- **Supprimer** les attributs superflus issus des jointures précédentes,
- **Renommer** les champs de manière cohérente et adaptée à ArcGIS Online :
  - Exemples : `departures`, `arrivals`, `station_id`, `station_name`, `longitude`, `latitude`, etc.

🎯 Objectif :
- Obtenir un fichier propre, normalisé et facilement exploitable dans ArcGIS Online, Dashboard, et Experience Builder.

---

✅ Cette dernière étape termine le processus de préparation et garantit que les données sont prêtes pour la publication et la visualisation.


# 2. Exportation des données vers ArcGIS Online

Après avoir préparé et nettoyé les données, l'étape suivante consiste à :

- 🌐 **Publier les stations Bixi enrichies** sur **ArcGIS Online (AGOL)** pour pouvoir les exploiter dans un tableau de bord interactif.

  ![image](https://github.com/user-attachments/assets/47e58f68-9412-4e0f-a7fc-de2a59d28abc)


### Accès à AGOL :

- Utiliser le lien suivant pour accéder à la plateforme AGOL de l'UQAM :  
  ➡️ [https://uqam.maps.arcgis.com/home/index.html](https://uqam.maps.arcgis.com/home/index.html)

### Étapes à suivre :

1. **Se connecter** à votre compte AGOL.
2. Aller dans **Contenu** → **Mes contenus**.
3. Sur la gauche, cliquer sur **Créer un dossier**.
4. Nommer ce dossier **`GEO7630`**.

![image](https://github.com/user-attachments/assets/94a5b289-f88f-46f0-bdfa-36f29dffe322)

🎯 Objectif :
- Organiser vos données dans un espace dédié avant publication,
- Préparer la base pour publier les Feature Layers nécessaires aux Dashboards et applications.

---

✅ Cette étape permet une gestion propre des fichiers et facilite leur exploitation dans les outils ArcGIS comme Dashboard ou Experience Builder.

## b. Visualisation et configuration du Feature Layer

Après avoir publié les données sur ArcGIS Online :

### Ouvrir et configurer le Feature Layer :

1. 📍 **Ouvrir le Feature Layer** nouvellement publié dans **Map Viewer**.
2. 🖌️ **Appliquer un style d’agrégation** :

   ![image](https://github.com/user-attachments/assets/9683e082-19d6-4114-8ec2-25c345c4d18b)

   - Utiliser l'agrégation pour représenter visuellement :
     - Le **nombre de départs** (`departures`)
     - Le **nombre d’arrivées** (`arrivals`) par station.
   - Choisir un style adapté pour une lecture rapide (ex. : cercles proportionnels, heatmap).
  
     ![image](https://github.com/user-attachments/assets/a9c2640c-a6fe-4908-963e-e99a8e67c54b)


3. 🏷️ **Configurer les étiquettes** :
   - Activer les étiquettes sur les stations.
   - Afficher les **totaux** (`departures` et `arrivals`) directement sur la carte.

🎯 Objectif :
- Permettre une **lecture immédiate** de l’activité des stations Bixi sur la carte,
- Préparer la couche pour une intégration directe dans un **Dashboard interactif**.

---

✅ Cette étape prépare visuellement les données pour une meilleure exploitation dans les outils ArcGIS.

## c. Configuration avancée des styles et étiquettes

Pour enrichir la visualisation du flux de vélos entre départs et arrivées :

### Étapes détaillées :

1. 🖌️ Dans Map Viewer, cliquer sur **Styles**.
2. ➕ **Ajouter un nouveau champ** :
   - Cliquer sur **“+ Champ”**,
   - Sélectionner les champs :
     - **`start_total_count`** (nombre de départs)
     - **`end_total_count`** (nombre d’arrivées).

3. ⚖️ **Choisir une méthode de comparaison** :
   - Sélectionner **“Comparer A à B”** comme mode d'affichage.
  
     ![image](https://github.com/user-attachments/assets/df600f61-7469-457c-ba36-ab5b6a9dbc7f)

![image](https://github.com/user-attachments/assets/3ecdcb05-cf26-4c98-a3a4-9266f4b25406)

4. 🏷️ **Configurer les étiquettes** :
   - Cliquer sur **Options de style**,
   - Aller dans l’onglet **Étiquettes**,
   - Choisir **“Afficher A comme pourcentage de A et B”**.

🎯 Objectif :
- Afficher **le pourcentage des départs par rapport au total** (départs + arrivées) pour chaque station,
- Rendre la lecture des flux beaucoup plus intuitive pour les utilisateurs du Dashboard.

![image](https://github.com/user-attachments/assets/6ba78a02-dd3c-4cdc-ba8b-d0d2eb206d96)

---

✅ Cette configuration rend l'analyse des stations Bixi plus fine et plus facile à interpréter, en mettant en évidence les stations à fort départ ou fort retour.

# 3. Création d'un tableau de bord (Dashboard)

Après avoir configuré votre carte et vos données, il est temps de créer un Dashboard pour visualiser dynamiquement les flux de vélos Bixi.

### Étapes détaillées :

1. 📊 Dans ArcGIS Online, ouvrir le **Dashboard** depuis le menu des applications.
2. ➕ **Créer un nouveau tableau de bord** :
   - Donner un **nom** significatif au Dashboard,
   - **Choisir le bon dossier**, par exemple **GEO7630**, pour l'organisation.
  
     ![image](https://github.com/user-attachments/assets/ecd07003-e76e-4a6c-b2ef-62e316e20fcf)


3. 🗺️ **Ajouter un élément Carte** :
   - Cliquer sur **“Ajouter un élément”** → **Carte**,
     
     ![image](https://github.com/user-attachments/assets/63944dba-8578-44d7-9c17-978fc1b1cadc)

   - Sélectionner la **carte créée précédemment** (celle avec les clusters et le style comparant les départs et arrivées).
  
     ![image](https://github.com/user-attachments/assets/d179c171-c59e-41fd-94da-fe62c7f03edb)

     ![image](https://github.com/user-attachments/assets/3290602e-705d-4c42-90b1-9c146254045f)



4. ⚙️ **Configurer les réglages de la carte** :
   - Ajuster les options selon vos préférences :
     - Activation/désactivation des zooms,
     - Affichage de la légende,
     - Choix du fond de carte,
     - Interaction avec les autres éléments du Dashboard (filtres, sélections, etc.).

5. ✅ Cliquer sur **Terminer** pour finaliser l'ajout de la carte au Dashboard.

🎯 Objectif :
- Visualiser en temps réel les flux de vélos,
- Permettre une exploration interactive des données par station directement dans une application web conviviale.

---

✅ tableau de bord est maintenant prêt pour ajouter d'autres éléments analytiques : graphiques, compteurs, filtres, listes, etc.

## b. Personnalisation du Dashboard : Mise en page et ajout d'indicateurs

Après avoir intégré la carte, nous allons améliorer la présentation générale du Dashboard.

### Étapes détaillées :

1. 🛠️ **Configurer la mise en page** :
   - Aller dans **Mise en page (Version)** dans le Dashboard.
     
     ![image](https://github.com/user-attachments/assets/58a07f59-11f9-4ff2-9a9f-742c71aa76a0)


2. ➕ **Ajouter un en-tête** :
   - Insérer un **En-tête** en haut du Dashboard,
   - Y indiquer un **titre clair**.
  
     ![image](https://github.com/user-attachments/assets/f827a99a-cf7d-494f-aafd-12206a0a5da1)


3. ➕ **Ajouter un élément de type Jauge** :
   
   ![image](https://github.com/user-attachments/assets/51df51e7-da83-4b5f-9e86-0ae5b050136b)

   - Dans la section **Corps**, cliquer sur **Ajouter un élément** → **Jauge**.
   - Configurer la jauge pour afficher :
     - **La somme statistique des départs** (`start_total_count`).

       ![image](https://github.com/user-attachments/assets/1fd01c3b-2664-4319-a43d-1be848d82fa8)


4. ⚙️ **Configurer les options de la jauge** :
   - Définir le minimum et maximum si nécessaire,
   - Personnaliser les couleurs ou seuils pour rendre la lecture plus intuitive.
  
     ![image](https://github.com/user-attachments/assets/83841777-8a8d-4774-b68b-35e6ee07c437)
![image](https://github.com/user-attachments/assets/70433aab-e7c4-4ee0-bac9-018e96c15a23)


🎯 Objectif :
- Offrir une **visualisation rapide et synthétique** du nombre total de départs dans le réseau de stations Bixi,
- Rendre le Dashboard plus informatif et attractif.

![image](https://github.com/user-attachments/assets/3d8eb5bc-9d06-4852-82ef-25b82f45781e)

---

✅ Cette configuration permet de fournir une vue d'ensemble immédiate sur l'intensité de l'activité des vélos partant des stations.

## c. Ajout d'un indicateur personnalisé

Pour enrichir davantage l'interface du Dashboard :

### Étapes détaillées :

1. ➕ **Ajouter un indicateur** :
   - Dans le Dashboard, cliquer sur **Ajouter un élément** → **Indicateur**.

2. ⚙️ **Configurer l'indicateur** :
   - Sélectionner la statistique désirée à afficher, par exemple :
     - Total des départs
     - Total des arrivées
     - Ratio départs/arrivées, etc.

3. 🎨 **Activer la mise en forme avancée** :
   - Dans les paramètres de l'Indicateur, **cliquer sur "Activer" la mise en forme avancée**.
     
     ![image](https://github.com/user-attachments/assets/c78f4019-c72e-4bd9-a44d-041b909e99cf)

     ![image](https://github.com/user-attachments/assets/f577502b-1fff-4b1e-a7f4-0ee1b3acfedc)


   - Cela permet de :
     - Personnaliser les polices,
     - Ajouter des couleurs dynamiques selon les valeurs,
     - Choisir des icônes ou symboles conditionnels.

🎯 Objectif :
- Rendre les indicateurs plus **visuellement attractifs**,
- Faciliter la **lecture rapide** des principales statistiques du réseau Bixi.

---

✅ Cette étape permet de rendre votre Dashboard encore plus dynamique, professionnel et agréable à utiliser.



![image](https://github.com/user-attachments/assets/677b6185-d51d-45a7-9270-400f08c7eaea)
