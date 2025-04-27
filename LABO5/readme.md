# 🛠️ Intégration et visualisation de données 3D Lidar et tuiles 3D vectorielles

# 1. Importation et nettoyage des données Lidar

## a. Importation des données

La première étape consiste à :

- 📥 **Importer les données LIDAR** dans FME.
- 🖱️ Pour cela, deux méthodes sont possibles :
  - Effectuer un **drag and drop** direct des **6 fichiers** Lidar disponibles
  - Ou les ajouter manuellement en utilisant plusieurs **Readers** adaptés

![image](https://github.com/user-attachments/assets/e6fccef0-2b95-492e-bcd8-a6a7b279439f)

---

✅ Cette étape permet de charger l'ensemble des jeux de données nécessaires au traitement Lidar.

## b. Nettoyage et reprojection

Dans cette étape, nous allons :

- 🧹 **Nettoyer et alléger le nuage de points Lidar** pour faciliter son traitement.
- 🛠️ Utiliser le transformateur **PointCloudThinner** dans FME pour :
  - **Réduire la densité** des points tout en conservant la forme générale de la surface ou de l'objet représenté.

🔧 Paramétrage important :
- Modifier le paramètre **Filter Distance** et le fixer à **30**.

![image](https://github.com/user-attachments/assets/a6db4585-9ce2-49c5-8b71-3d2700c4a2fc)

---

✅ Cette réduction de la densité permet d'améliorer les performances d'affichage et de traitement tout en préservant la qualité générale du modèle 3D.

## c. Combinaison des nuages de points

Après le nettoyage individuel des jeux de données, nous allons :

- 🔗 **Combiner les 6 nuages de points Lidar** en un seul ensemble cohérent.
- 🛠️ Utiliser le transformateur **PointCloudCombiner** dans FME pour fusionner tous les fichiers en un seul nuage de points global.
  
![image](https://github.com/user-attachments/assets/6bde13c7-7633-4d83-b64d-c363f0869071)

⚙️ Paramétrage :
- Conserver les **paramètres par défaut** du transformateur.

---

✅ Cette combinaison facilite les traitements ultérieurs et permet d'obtenir une vue d'ensemble uniforme pour la création de tuiles ou l'analyse spatiale.


## d. Reprojection finale de la couche combinée

Après la combinaison des nuages de points, nous allons :

- 🔄 **Reprojeter le nuage de points combiné** dans un nouveau système de coordonnées.
- 🛠️ Utiliser le transformateur **EsriReprojector** dans FME, qui est une variante du Reprojector classique mais spécialement optimisée pour :
  - Les systèmes de coordonnées définis par Esri (ArcGIS, etc.)
 
    ![image](https://github.com/user-attachments/assets/2badc868-11e1-49f8-8df1-f17050cc4021)


🎯 Objectif de la reprojection :
- Passer du **SCR 2950** (**NAD83 / MTM Zone 5**) ➔ vers **SCR 3857** (**WGS 84 / Pseudo-Mercator**).

---

✅ Cette reprojection permettra une meilleure compatibilité avec les outils de visualisation web (comme les tuiles 3D ou les cartes en ligne).

# 📈 Workflow de la première étape

À l'issue de cette première phase d'importation, de nettoyage, de combinaison et de reprojection des données Lidar,  
nous obtenons le **workflow suivant** :

- Importation des 6 fichiers Lidar
- Nettoyage des nuages de points avec `PointCloudThinner`
- Combinaison de tous les nuages avec `PointCloudCombiner`
- Reprojection de la couche combinée du SCR 2950 vers le SCR 3857 via `EsriReprojector`

![image](https://github.com/user-attachments/assets/63c941ff-b50a-4fbd-897c-36b851f1050c)

---

✅ Ce flux de travail assure une base de données Lidar propre, cohérente et correctement géoréférencée pour la suite du traitement et la création des tuiles 3D.


# 2. Importation des limites terrestres et découpage du nuage de points

## a. Ajout d'un GeoJSON et reprojection

Dans cette étape, nous allons :

- 📥 **Importer une couche de limites terrestres** au format **GeoJSON**.
- 🔄 **Reprojeter** cette couche du **SCR 4326** (**WGS84**) vers **SCR 3857** (**WGS 84 / Pseudo-Mercator**),  
  comme nous l'avons fait précédemment avec les données Lidar.

📚 Source des données :  
➡️ [Limites terrestres de Montréal (GeoJSON)](https://data.montreal.ca/dataset/b628f1da-9dc3-4bb1-9875-1470f891afb1/resource/92cb062a-11be-4222-9ea5-867e7e64c5ff/download/limites-terrestres.geojson)


![image](https://github.com/user-attachments/assets/785b6969-02d0-4cc0-a26e-2bd9c2b3f7f0) ![image](https://github.com/user-attachments/assets/0a8ed0f8-d5ac-4909-9b85-545417047c5c)


---

✅ Cette étape est indispensable pour assurer la compatibilité spatiale entre la couche de limites terrestres et le nuage de points reprojeté.

## b. Découpage avec les limites terrestres

Pour limiter notre nuage de points à la zone d'intérêt :

- ✂️ **Utiliser le transformateur Clipper** dans FME, qui permet :
  - De découper des données spatiales en fonction d'une zone définie.

    ![image](https://github.com/user-attachments/assets/d422a5b3-5515-45da-a372-d319500a8bd1)

Concrètement :

- Connecter les **nuages de points** (obtenus dans la première étape) à l'entrée **Candidate** du Clipper.
- Connecter les **limites terrestres** (importées en GeoJSON) à l'entrée **Clipper** du Clipper.

  ![image](https://github.com/user-attachments/assets/efa2a3ce-e6cb-470e-875f-764e2db1944c)


---

✅ Ce découpage permet d'extraire uniquement la portion du nuage de points correspondant à notre territoire cible, optimisant ainsi les traitements ultérieurs.

# 3. Simplification du nuage de points

Dans cette étape, nous allons :

- 🔄 **Simplifier davantage le nuage de points** issu du découpage précédent.
- 🛠️ Utiliser de nouveau le transformateur **PointCloudThinner** dans FME pour :
  - Réduire encore la densité du nuage de points.

🔧 Paramétrage important :
- Fixer le **Filter Distance** à **5** dans les paramètres du PointCloudThinner.

🎯 Note méthodologique :
- Normalement, l'outil **PointCloudSimplifier** serait utilisé pour alléger le nuage tout en préservant mieux sa forme.
- Cependant, ce processus est **très gourmand en ressources**.
- ➡️ Pour les besoins de cet exercice, nous utilisons à la place un **PointCloudThinner**, qui offre un compromis acceptable.

![image](https://github.com/user-attachments/assets/be0d900e-93a6-4739-b073-d7e40f00d691)

![image](https://github.com/user-attachments/assets/d47a57f6-5cbc-4feb-9316-7d672f32c3c2)


---

✅ Cette simplification permet de réduire la taille

# 4. Ajout de rasters géoréférencés

Dans cette étape, nous allons ajouter une **composante couleur** à notre nuage de points :

## Étapes :

- 📥 **Ajouter les 4 fichiers rasters** dans le projet FME :
  - Effectuer un **drag and drop** direct dans l'espace de travail.
 
    ![image](https://github.com/user-attachments/assets/c01f2a01-8e63-45aa-b220-d6bc8f0a6186)


- 🔄 **Reprojeter les rasters** :
  - Reprojeter chaque fichier depuis **EPSG:3857** (**Pseudo-Mercator**) vers **EPSG:32188** (**NAD83 / MTM Zone 8**).
    
    ![image](https://github.com/user-attachments/assets/2952a9d9-1b05-42ce-a127-4637d1d1d662)

    ![image](https://github.com/user-attachments/assets/fe406ed4-0bdc-4810-9c0b-e90e70e5de6e)


- 🧩 **Assembler les rasters** :
  - Utiliser le transformateur **RasterMosaicker** pour :
    - Combiner les 4 fichiers raster
    - Créer une **image unique et homogène** couvrant toute la zone d'étude.
      
![image](https://github.com/user-attachments/assets/49058c26-5a5d-4445-b6f8-a2831e215ab6)
   
      ![image](https://github.com/user-attachments/assets/a9d41c04-0626-4c3e-b02a-e063d9c16c6f)


---

✅ Cette opération est essentielle pour pouvoir **associer la couleur du raster** aux points du nuage, et ainsi améliorer le réalisme visuel lors de la création des tuiles 3D.

## Suite : Sélection des bandes RGB et suppression de la bande alpha

Après avoir assemblé les rasters, nous allons affiner les informations de couleur :

- 🎨 **Utiliser le transformateur RasterSelector** pour :
  - Sélectionner uniquement les **3 bandes RGB** (Rouge, Vert, Bleu)
  - Supprimer la **bande alpha** (transparence), non nécessaire pour notre traitement

    ![image](https://github.com/user-attachments/assets/2969519a-c9d6-4684-92fe-e4d12921a571)

![image](https://github.com/user-attachments/assets/aae2e4bb-701f-4b76-b6ae-8ab488bb5dcc)

🛠️ Pour bien identifier la correspondance entre les bandes et les couleurs :

- Utiliser le transformateur **RasterPropertyExtractor** :
  - Pour extraire les propriétés détaillées du raster, en particulier celles liées aux **bandes**.
- Puis utiliser un **ListExploder** :
  - Pour explorer en détail la liste des propriétés extraites, notamment la correspondance des **bandes**.

![image](https://github.com/user-attachments/assets/87eabc69-deb9-437a-acaa-89af095bf5e5)


🎯 Objectif :
- Identifier correctement quelle bande correspond à quelle couleur (Rouge, Vert, Bleu)
- S'assurer de travailler uniquement avec les bonnes informations spectrales pour coloriser le nuage de points.

---

✅ Cette étape est essentielle pour garantir une bonne correspondance entre les valeurs raster et la coloration précise des points 3D.

## Finalisation : Association des couleurs au nuage de points

Après la sélection des bonnes bandes RGB :

- 🔄 **Reprojeter à nouveau** :
  - Utiliser **EsriReprojector** pour projeter le raster de **SCR 2950** vers **SCR 3857**,  
    afin d'assurer la compatibilité avec le nuage de points.

    ![image](https://github.com/user-attachments/assets/b45fae39-9a90-47a3-b07c-e82ef0b25dbc) ![image](https://github.com/user-attachments/assets/2516761c-ac3d-425a-bab7-66ac0f543fcb)



- 🎯 **Associer la couleur aux points** :
  - Utiliser le transformateur **PointCloudOnRasterComponentSetter** dans FME.
  - Cet outil permet d'**enrichir un nuage de points** en ajoutant des valeurs provenant d'un raster :
    - Comme l'**élévation**, **l'intensité**, ou ici les **valeurs de couleur RGB**.
      
![image](https://github.com/user-attachments/assets/4698b02d-1fb6-4472-b046-1d990c08a633)

### Connexions :

- Connecter les résultats du **nuage de points simplifié** (étape 3) à l'entrée du **PointCloudOnRasterComponentSetter**.
- Connecter également le **raster mosaïqué et filtré** (avec les bonnes bandes RGB).

⚙️ Paramètres :
- **Conserver les paramètres par défaut** dans le PointCloudOnRasterComponentSetter.

---

✅ Cette étape finalise la **colorisation du nuage de points**, essentielle pour une visualisation 3D réaliste et riche en informations.

## Fusion et filtrage du nuage de points colorisé

### Fusion des nuages de points :

- 🔗 **Utiliser le transformateur PointCloudCombiner** pour :
  - **Combiner** les différentes parties du nuage de points enrichi en **un seul ensemble** complet.

![image](https://github.com/user-attachments/assets/064c81c0-5f8a-4219-9c25-8c640bcb0e98)

---

### Filtrage des valeurs du nuage de points :

- 🔍 **Utiliser le transformateur PointCloudFilter** pour :
  - **Filtrer** les points du nuage de points en fonction de leur valeur de couleur.

📜 Expression de filtrage à utiliser :

```
@Component(color_red)!=0 && @Component(color_blue)!=0 && @Component(color_green)=0
```

![image](https://github.com/user-attachments/assets/a563ccc6-4721-43b6-b49c-afc0bcf2d42c)

## Transformation finale du nuage de points en vecteurs ponctuels

Pour finaliser cette étape :

- 🎯 **Transformer le nuage de points** en une couche de **vecteurs ponctuels simples**.
- 🛠️ Utiliser le transformateur **PointCloudCoercer** dans FME.

  ![image](https://github.com/user-attachments/assets/6f3107a0-bd77-4e77-abc2-c2d1d0434873)


### Points importants :

- **S'assurer** de conserver uniquement les composantes nécessaires :
  - `Z` (élévation)
  - `Rouge (color_red)`
  - `Vert (color_green)`
  - `Bleu (color_blue)`

---

✅ Cette opération permet d'obtenir une couche vectorielle propre et optimisée,  
idéale pour les traitements d'affichage 3D, d'export ou pour générer les tuiles vectorielles finales.

# 5. Ajout des emprunts et détails de bâtiments

Maintenant que notre nuage de points est nettoyé et enrichi, nous allons **assigner la hauteur (Z) et les couleurs RGB** aux polygones de bâtiments.

## a. Ajout et reprojection des données des bâtiments

Dans cette étape :

- 📥 **Ajouter les données des bâtiments**, fournies sous forme de **Shapefile (.shp)**.
- 🔄 **Reprojeter** ces données du **SCR 2950** vers **SCR 3857**, afin de les aligner spatialement avec notre nuage de points.

🛠️ Utiliser le transformateur **EsriReprojector** dans FME pour effectuer cette transformation.

![image](https://github.com/user-attachments/assets/a9682b4f-086a-4224-9bbe-1b5abdf7daca)

---

✅ Cette étape assure que les couches de bâtiments sont dans le même système de coordonnées que les données Lidar colorisées,  
permettant ainsi une association correcte des altitudes et des couleurs par la suite.


## b. Calcul de l'entité spatiale

Pour analyser la couverture spatiale de nos données raster, nous allons :

- 📦 **Utiliser le transformateur BoundingBoxAccumulator** dans FME :
  - Cet outil permet de calculer la **boîte englobante** d'un ensemble d'objets géographiques (points, lignes, polygones ou rasters).
  - Cela permet de délimiter efficacement une **zone d’intérêt** globale.
 
    ![image](https://github.com/user-attachments/assets/30a5588b-8919-48d1-8a96-83a0c9621905)


### Connexions à établir :

- Connecter les **4 rasters filtrés** (issus de l'étape 4) à l'entrée du **BoundingBoxAccumulator**.

---

✅ Cette étape est essentielle pour obtenir une étendue spatiale précise,  
utile pour encadrer les traitements d'assignation des couleurs et hauteurs aux bâtiments.

## c. Découpage et calcul avec BoundingBox

Pour travailler uniquement sur la **zone d'intérêt** définie par la bounding box :

- ✂️ **Utiliser le transformateur Clipper** pour :
  - Découper chacun des **Shapefiles** de bâtiments ajoutés au projet.

### Connexions à établir :

- Connecter chaque **Shapefile de bâtiments** à l'entrée **Candidate** de son propre Clipper.
- Connecter les **résultats du BoundingBoxAccumulator** à l'entrée **Clipper** de chacun des Clippers.

  ![image](https://github.com/user-attachments/assets/e76aae6c-e3de-4dbe-83c2-a7fa6dbfef57)


🎯 Objectif :
- Restreindre les entités géographiques aux limites calculées par la boîte englobante,
- Travailler uniquement sur la portion nécessaire pour l'assignation des attributs.

---

✅ Ce découpage spatial garantit que seules les entités pertinentes sont conservées pour l’étape suivante : l’assignation du Z et des couleurs issues du nuage de points.


## d. Découpage final des polygones et lignes

Après avoir restreint les bâtiments à la zone d'intérêt avec les Clippers, nous allons affiner encore plus les géométries :

- ✂️ **Utiliser le transformateur PolygonCutter** dans FME :
  - Pour **découper précisément** les **polygones** et **lignes** en fonction des limites définies.
 
    ![image](https://github.com/user-attachments/assets/88a6ba1e-d30d-458b-b82c-3dfbc359880d)

### Connexions à établir :

- Connecter les **Shapefiles concernés** (bâtiments ou autres entités découpées précédemment) à l'entrée du **PolygonCutter**.

🎯 Objectif :
- Obtenir des entités géographiques parfaitement adaptées à notre zone d'intérêt,
- Préparer les bâtiments pour l’assignation des valeurs de hauteur (Z) et des couleurs (RGB) issues du nuage de points.

---

✅ Cette étape finalise la préparation géométrique des polygones avant leur enrichissement en attributs spatiaux.


# 6. Jointure des propriétés du nuage de points dans les polygones

Pour enrichir les polygones de bâtiments avec les informations issues du nuage de points :

- 🔗 **Utiliser le transformateur PointOnAreaOverlayer** dans FME :
  - Cet outil permet de **joindre** les polygones détaillés avec les **points du nuage de points colorisé** (issus de l’étape 4).
  - L'objectif est **d'injecter les valeurs de Z et de couleurs** (Rouge, Vert, Bleu) dans chaque bâtiment.
 
    ![image](https://github.com/user-attachments/assets/4f80e4f7-9193-4eb4-8cb1-67c7658524c9)

    ![image](https://github.com/user-attachments/assets/61bc41b2-b4c6-4f6d-b571-45e075c37700)

### Paramétrage spécifique :

- Dans les paramètres du **PointOnAreaOverlayer** :
  - Définir **Area List Name** à `"Hauteur_Batim"`.

🎯 Objectif :
- **Accumuler les valeurs** de Z et des couleurs pour chaque bâtiment.
- Préparer les données pour **calculer les moyennes** par bâtiment (hauteur moyenne, couleur moyenne).

---

✅ Cette étape est essentielle pour générer des bâtiments 3D ayant une élévation réaliste et une coloration fidèle basée sur les données du nuage de points.

## Suite : Somme des hauteurs Z

Après avoir accumulé les informations de Z pour chaque bâtiment, nous allons :

- ➕ **Calculer la somme des valeurs de Z** pour chaque polygone.

🛠️ Utiliser le transformateur **ListSummer** dans FME pour :

- Sélectionner comme attribut à sommer : **`z_accumulation`** (issu de l'étape précédente).
- Nommer le résultat de la somme dans un nouvel attribut appelé **`sum`**.

  ![image](https://github.com/user-attachments/assets/385ca86b-dd6c-486e-b696-5eb7545e1552)


🎯 Objectif :
- Obtenir la **somme totale des hauteurs** capturées pour chaque bâtiment,
- Étape préalable au calcul de la **moyenne d'élévation**.

---

✅ Cette opération prépare les données pour normaliser les hauteurs des bâtiments et leur attribuer une valeur réaliste basée sur le nuage de points.

## Finalisation : Création de l'attribut de moyenne Z

Après avoir obtenu la somme des hauteurs (Z) pour chaque bâtiment :

- 🧮 **Créer un nouvel attribut** représentant la **moyenne d'altitude (Z)**.

🛠️ Utiliser le transformateur **AttributeCreator** dans FME pour :

![image](https://github.com/user-attachments/assets/27bff4c2-5258-488d-b88f-8b4046843c52)


- Créer un attribut nommé **`z`**.
- Utiliser l'expression suivante pour calculer la moyenne arrondie à 4 décimales :

```plaintext
@Evaluate(@round(@Value(_sum)/@Value(_overlaps),4))
```

## Nettoyage final des attributs

Pour finaliser la préparation des bâtiments enrichis :

- 🧹 **Épurer les attributs** pour ne conserver que les informations essentielles.

🛠️ Utiliser le transformateur **AttributeManager** dans FME pour :

- **Supprimer tous les attributs inutiles**,
- **Conserver uniquement** :
  - `z` (moyenne des hauteurs)
  - `z_accumulation` (liste initiale des valeurs de hauteur)
    
![image](https://github.com/user-attachments/assets/f17344d1-4e19-4844-8c64-2ee2ed4e8215)

🎯 Objectif :
- Simplifier les couches de sortie pour un usage optimisé dans des applications de visualisation 3D ou SIG.

---

✅ Cette dernière étape garantit que seules les données nécessaires sont conservées,  
facilitant la génération finale des tuiles 3D ou l'intégration dans des systèmes d'information géographique.

## Conversion des couleurs pour la sortie finale

Pour rendre les couleurs compatibles avec les formats standards du web ou des applications SIG :

- 🎨 **Utiliser le transformateur ColorConverter** dans FME.

### Objectif :

- **Convertir les couleurs** du format natif FME en :
  - **RGB** classique
  - ou **WebRGB** (hexadécimal), selon l'usage souhaité.

🛠️ Paramétrage :
- Choisir le mode de conversion vers **RGB** ou **WebRGB** dans les options du ColorConverter.

🎯 Cela permettra :
- Une compatibilité optimale avec les standards d'affichage sur le web
- Ou une intégration directe dans des viewers 3D ou des outils de cartographie moderne.

---

✅ Cette conversion standardise les couleurs pour assurer une visualisation cohérente sur toutes les plateformes cibles.

![image](https://github.com/user-attachments/assets/5e287878-6d60-4d43-9ff1-2cb16839cf1c)

## Exportation finale pour visualisation

Pour terminer le processus :

- 📤 **Exporter les bâtiments enrichis** dans un fichier GeoJSON.

🛠️ Utiliser un **Writer de type GeoJSON** dans FME pour :

- Sauvegarder le résultat final dans un format léger et standardisé.

### Utilisation du résultat :

- 📍 **Ouvrir le fichier GeoJSON** généré dans votre **fichier `Maplibre.html`**.
- Cela permettra une **visualisation dynamique et interactive** directement sur une carte web basée sur MapLibre.

🎯 Objectif :
- Visualiser les bâtiments en 3D avec leurs **hauteurs** et **couleurs** correctement appliquées,
- Valider le rendu final dans un environnement cartographique moderne.

![image](https://github.com/user-attachments/assets/02cf5b9b-7468-4276-a683-655348800558)

![image](https://github.com/user-attachments/assets/ad7b457c-9d90-4f48-b264-98e62f4f516e)

![image](https://github.com/user-attachments/assets/51f3fa3f-26cb-40a6-904a-c2c2d02d240d)




---

✅ Cette dernière étape rend votre projet exploitable sur le web et prêt pour une présentation ou une intégration dans des systèmes de visualisation en ligne.
