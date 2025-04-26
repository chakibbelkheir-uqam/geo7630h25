# 🌍 Traitement et Diffusion de Rasters avec PostgreSQL/PostGIS

## Objectif du laboratoire

Ce laboratoire a pour but de développer des compétences pratiques autour du traitement et de la diffusion de données raster.  
Les principales étapes couvertes sont :

- 📥 **Lecture et traitement** de différents types de rasters : `TIF`, `GeoTIFF`, `PNG`
- 🛠️ **Extraction de valeurs raster** pour les convertir en données vectorielles
- 🗄️ **Stockage** de rasters non tuilés dans une base de données **PostgreSQL/PostGIS**
- 🗺️ **Génération de tuiles raster** pour faciliter la manipulation et la diffusion des données
- 🏛️ **Création de pyramides de tuiles raster** pour optimiser la visualisation web
- 🔗 **Association des valeurs Z** extraites des rasters aux objets vectoriels

## 📂 Données utilisées

Les données nécessaires pour ce laboratoire sont disponibles ici :  
➡️ [Lien vers les données](https://drive.google.com/drive/folders/1iRcyRWS_JiTciNdonm8leC7Nq03hRY5_?usp=sharing)

# 🛠️ Intégration des données sur FME

## Étapes de cette partie

Dans cette première partie, nous allons :

- 📥 **Intégrer les données raster** de type **TIF** et/ou **CoG** (Cloud Optimized GeoTIFF) en utilisant :
  - Un **Reader de type CoG** pour les fichiers CoG

![image](https://github.com/user-attachments/assets/5af3b415-4ba7-4b91-b6bc-95dbce0d26fc)

  - Un **Reader de type PNG** pour les fichiers PNG

    ![image](https://github.com/user-attachments/assets/71dd9b1f-fcf2-4b1e-82b6-ed31b182c593)


- 🧩 **Créer des bookmarks** pour chaque traitement :
  - Afin de bien organiser le processus dans FME
  - Et de distinguer clairement les données importées des données traitées

    ![image](https://github.com/user-attachments/assets/2a3a2727-f87c-4f94-ac7f-34ac765a240e)


---

✅ Cette structuration permettra une meilleure clarté dans votre espace de travail FME et facilitera la gestion des différentes étapes du traitement.

# 🛠️ 1. Intégration d'image aérienne standard

## a. Reprojection des données

La première étape de ce laboratoire consiste à :

- 🔄 **Reprojeter les données d'images aériennes** (format `PNG`) vers la projection **EPSG:32188**.
- 🛠️ Utiliser le transformateur **Reprojector** dans FME pour effectuer cette transformation.

  ![image](https://github.com/user-attachments/assets/edc0f6f8-bb71-4424-9633-621223209953)
![image](https://github.com/user-attachments/assets/07516fc3-1eaa-4e45-a4c2-a5719b3aeb05)


---

✅ Cette étape est essentielle pour assurer que toutes les données raster soient alignées spatialement dans le bon système de coordonnées.

## b. Extraction des métadonnées du raster

L'étape d'extraction des métadonnées consiste à :

- 🔍 **Extraire des propriétés et métadonnées** d'un raster, telles que :
  - La dimension (largeur, hauteur)
  - Le type de données
  - Le nombre de bandes
  - Et d'autres informations essentielles à son analyse et traitement

- 🛠️ Utiliser le transformateur **RasterPropertyExtractor** dans FME pour obtenir ces métadonnées.

  ![image](https://github.com/user-attachments/assets/3dc9afa4-789f-43b3-ab76-0013f0c6333a)


- 📊 Les propriétés les plus importantes à récupérer sont :
  - Les **pixels**
  - Les **rangées**
  - Les **bandes**

---

✅ Cette étape permet de mieux comprendre la structure interne du raster et de préparer les prochaines étapes de traitement.

## c. Rééchantillonnage de l'image raster

Le rééchantillonnage consiste à :

- 🔄 **Redimensionner ou rééchantillonner** une image raster afin d'ajuster :
  - Sa résolution
  - Sa taille
  - Sa géométrie
- 🛠️ Utiliser le transformateur **RasterResampler** dans FME pour effectuer cette opération.
  
![image](https://github.com/user-attachments/assets/bd795165-194d-4af6-aac3-dad9d56f8489)

Lors de cette étape, nous allons :

- Rééchantillonner les **colonnes** et les **rangées** en divisant leurs nombres par 10.
- Utiliser le calculateur intégré du transformateur :
  - Nouvelle colonne = `Nombre de colonnes / 10`
  - Nouvelle rangée = `Nombre de rangées / 10`

![image](https://github.com/user-attachments/assets/6005c210-0e00-415f-adf9-59b250d2b488)

---

✅ Cette opération est essentielle pour réduire la taille du raster et l'adapter aux besoins d'analyse spatiale ou de performance.

## d. Optimisation de l'affichage par création de pyramides sur le raster

L'optimisation de l'affichage consiste à :

- 🧱 **Créer une série de pyramides raster**, c’est-à-dire des versions de résolution inférieure de l'image raster originale.
- 🎯 Cette technique permet :
  - D'accélérer l'affichage
  - De faciliter l'analyse de grandes images raster sur différents niveaux de zoom

Pour cela, nous allons :

- Utiliser le transformateur **RasterPyramider** dans FME
- Configurer le nombre de niveaux de pyramides à **10** dans les paramètres

  ![image](https://github.com/user-attachments/assets/2bd6342e-88d0-440a-889d-2e3f505b84b1)


---

✅ Cette étape est cruciale pour améliorer les performances d'affichage dans les applications SIG ou web.

## e. Ajout d'un FeatureWriter

Le **FeatureWriter** permet de :

- 🔗 **Chaîner plusieurs actions** immédiatement après l'écriture d'un fichier ou d'une base de données.
- 🛠️ Contrairement au **Writer standard**, il permet d'enchaîner des traitements supplémentaires sur les données sans interrompre le flux de travail.

Dans cette étape, nous allons :

- Utiliser l'outil **FeatureWriter** dans FME pour écrire les rasters traités tout en conservant la possibilité d'appliquer d'autres transformations par la suite.

![image](https://github.com/user-attachments/assets/1c4abfd0-4f6d-4f6e-9e2d-b2d6b446adeb)

![image](https://github.com/user-attachments/assets/4d69d245-be92-43fe-a6b0-39ee1d58a1f9)

---

✅ L'utilisation du FeatureWriter optimise la fluidité et la flexibilité des traitements dans le workspace FME.

## f. Utilisation d'un SQL Executor pour visualiser à la fin

Le **SQL Executor** permet de :

- 🖋️ **Exécuter directement des requêtes SQL** sur une base de données, qu'elle soit spatiale ou non spatiale.
- 📈 **Visualiser ou manipuler** les données finales après traitement.

Dans cette étape, nous allons :

- Utiliser l'outil **SQLExecutor** dans FME pour :
  - Valider les résultats
  - Effectuer des requêtes d'inspection
  - Ou encore mettre à jour des données existantes en base


![image](https://github.com/user-attachments/assets/80907cd2-ad22-4a1e-b0d6-240f39f47e93)


---

✅ Cette étape permet de finaliser le flux de traitement en contrôlant les données directement via des requêtes SQL.


# 🛠️ 2. Intégration de raster analytique - Îlots de chaleur

## Étapes de cette partie

Dans cette deuxième partie, nous allons :

- 🔄 **Répéter les premières étapes** réalisées pour les images aériennes standards :
  - **Reprojeter les données** raster représentant les îlots de chaleur en utilisant la projection **EPSG:32188**.

## a. Conversion des images raster en entités vectorielles polygonales

La conversion consiste à :

- 🛠️ Utiliser l'outil **RasterToPolygonCoercer** dans FME pour :
  - Transformer des informations raster (ex: classifications d'images, MNT) en **formes vectorielles polygonales**.

- 🔧 Important : dans les paramètres du transformateur, il faut :
  - Changer l'attribut **Label Attribute** en **"Classification"**.
    
![image](https://github.com/user-attachments/assets/99f0f4ba-4c69-4430-9702-850b5366d68d)

---

✅ Cette étape permet d'obtenir des polygones vectoriels exploitables pour des analyses spatiales plus avancées.


## b. Réécriture des résultats et visualisation dans la base de données

Après la conversion en polygones, nous allons :

- 🔗 **Connecter la sortie du `RasterToPolygonCoercer`** à un **Writer** de type **PostGIS**.
- 🛠️ Cette fois-ci, **le Writer sera PostGIS** (et non PostGIS Raster), car nous travaillons désormais sur des **couches vectorielles**.
  
![image](https://github.com/user-attachments/assets/50598e2e-4553-4166-95eb-0cd8b0be60dd)


Ensuite :

- 🗺️ **Visualiser les résultats** dans **QGIS** pour valider l'intégration et l'affichage des îlots de chaleur sous forme de polygones.

![image](https://github.com/user-attachments/assets/b74ac6b9-709f-4382-80ca-8be9e20ce10c)

---

✅ Cette étape permet de stocker les données vectorielles directement en base de données et de les consulter facilement via un logiciel SIG.


## c. Lissage et amélioration de l'image

Pour améliorer la qualité visuelle des îlots de chaleur, nous allons :

- 🎨 **Appliquer un lissage** de l'image raster :
  - Utiliser le transformateur **RasterDiffuser**.
  - Garder tous les paramètres par défaut (ne rien modifier).
 
    ![image](https://github.com/user-attachments/assets/c43e764f-69b6-4b8b-8772-b10864f26dd3)


Ensuite :

- 🔢 **Standardiser les valeurs numériques** :
  - Utiliser le transformateur **RasterCellValueRounder**.
  - Modifier l'option **Decimal Places** et la fixer à **1**.

![image](https://github.com/user-attachments/assets/e5db5820-2c89-4e4d-ae1d-d18242e92219)

---

✅ Ces étapes permettent d'améliorer la lisibilité et la précision des couches raster lors de la visualisation ou de l'analyse.

## d. Visualisation finale de l'amélioration

Pour évaluer l'amélioration de la qualité visuelle des îlots de chaleur :

- 🔄 **Transformer à nouveau le raster en polygones**, après les opérations de lissage et d'arrondi.
- 🛠️ Utiliser à nouveau le transformateur **RasterToPolygonCoercer**, comme dans l'étape précédente.

  ![image](https://github.com/user-attachments/assets/5787621d-6fe9-490d-b889-165127ca465d)

![image](https://github.com/user-attachments/assets/0c8cc765-6195-415d-a393-852b948b2f88)

---

✅ Cette dernière conversion permet de visualiser clairement l'effet des traitements appliqués sur les îlots de chaleur, et de générer une couche vectorielle prête pour l'analyse ou l'affichage final.

## d. Conversion de tous les pixels du raster

Afin de convertir tous les pixels du raster en différentes entités géométriques :

- 🔄 Utiliser l'outil **RasterCellCoercer** dans FME.
- 🎯 Cet outil permet de transformer un raster en :
  - **Points**
  - **Lignes**
  - **Polygones**
- Cela permet d'obtenir une représentation plus complète du raster, contrairement à la conversion précédente qui ne produisait que des polygones.

  ![image](https://github.com/user-attachments/assets/1b47ea7d-f7d1-4027-89aa-304611bb2831)


![image](https://github.com/user-attachments/assets/245c61f9-23db-4fe5-8973-ba0f10cc793d)

---

✅ Cette étape ouvre de nouvelles possibilités d'analyse spatiale en travaillant avec plusieurs types de géométries dérivées du raster.


# 🛠️ 3. Intégration de raster (MNS)

## a. Ajout d'un ContourGenerator

Dans cette partie, nous allons :

- 📈 **Générer des courbes de niveau** à partir d'un raster représentant un **Modèle Numérique de Surface (MNS)**.
- 🛠️ Utiliser le transformateur **ContourGenerator** dans FME pour :
  - Créer des lignes de contour correspondant à différents niveaux d'altitude à intervalles réguliers.
 
    ![image](https://github.com/user-attachments/assets/bac70c8e-b2be-4d1f-885b-04065cb4ed31)


⚠️ **Important** : Cette étape peut nécessiter un **temps d'exécution important** en fonction de la taille du raster.

---

✅ Cette opération est essentielle pour visualiser les variations d'altitude de manière vectorielle à partir d'une surface raster.


## b. Ajout d'un Generalizer

Après la génération des courbes de niveau, nous allons :

- ✨ **Simplifier les géométries vectorielles** (lignes ou polygones) obtenues.
- 🛠️ Utiliser le transformateur **Generalizer** dans FME pour :
  - Réduire le nombre de sommets des entités vectorielles
  - Conserver la forme générale de chaque entité
 
    ![image](https://github.com/user-attachments/assets/f8886220-df2c-48c6-92db-d5e3915d95d2)


🎯 Cette étape est essentielle car elle permet :
- De **réduire considérablement la taille des fichiers**
- Tout en **minimisant les pertes d'information géométrique importantes**

---

✅ La généralisation des courbes de niveau optimise la performance des systèmes de visualisation et d'analyse sans sacrifier la qualité spatiale.


## c. Ajout d'un AreaBuilder

Après la simplification des lignes, nous allons :

- 🧩 **Créer des polygones à partir de lignes fermées**.
- 🛠️ Utiliser le transformateur **AreaBuilder** dans FME pour :
  - Transformer des données linéaires (contours, limites, tracés) en **zones fermées**.

![image](https://github.com/user-attachments/assets/14fee9f1-4765-4542-b3c6-79216180bf26)

🎯 Cette étape permet :
- De faciliter les **analyses spatiales** (calculs de surface, intersections, etc.)
- D'améliorer la **visualisation** en représentant des zones plutôt que de simples lignes

⚙️ **Note** : Conserver les **paramètres par défaut** lors de l'exécution de l'AreaBuilder.

---

✅ Cette conversion est essentielle pour travailler avec des surfaces plutôt qu'avec des lignes, ouvrant davantage d'options d'analyse et de cartographie.

## d. Chargement et visualisation finale dans QGIS

Pour finaliser le processus :

- 🗄️ **Charger les polygones** créés dans la **base de données PostGIS**.
- 🖥️ **Visualiser les résultats dans QGIS**, afin d'examiner :
  - L'extrapolation des bâtiments en **2D** réalisée à partir du MNS.

🎯 Cette étape permet :
- De valider visuellement la qualité de la transformation des données
- De préparer les couches pour une utilisation ultérieure dans des analyses spatiales ou cartographiques

---
![image](https://github.com/user-attachments/assets/083e4ea3-a888-43f0-90c4-a109771823c7)

✅ Le chargement dans PostGIS et l'affichage dans QGIS marquent la fin du processus d'intégration et de traitement des rasters pour ce laboratoire.

# 🧩 Conclusion

Pour conclure ce laboratoire :

Tout au long des différentes étapes, nous avons construit un **workflow complet** d'intégration et de traitement de données raster, passant par :

- L'importation, la reprojection et l'extraction de métadonnées
- Le rééchantillonnage et la création de pyramides
- La transformation des rasters en entités vectorielles
- L'amélioration et l'optimisation des données pour une visualisation efficace
- L'intégration finale dans une base de données spatiale (PostGIS) et la validation dans QGIS

🎯 Le workflow ainsi obtenu illustre l'ensemble des bonnes pratiques pour **manipuler efficacement des rasters** dans un environnement SIG professionnel.

---
![image](https://github.com/user-attachments/assets/86a6a0f8-f53f-4864-99a7-88cc5d48c7ea)
