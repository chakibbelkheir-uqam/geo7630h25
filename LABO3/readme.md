# Compte Rendu du laboratoire 3 :

## Introduction
Déscription des étapes relatives au laboratoire 3 

## Matériel et Méthodes
Listez le matériel utilisé et les étapes méthodologiques suivies.

### Matériel
- FME Workbench 
- QGIS

### Étapes
1. Étape 1 :Ouvrir FME et Création d'un nouveau projet :
2. Étape 2 :Lecture du CSV 

   1- Input : Arbres et Parcs de la ville de montréal 
Avec les données d'adresse,Longitude et Latitude

Ces données sont extraites du site des données ouvertes de la ville de Montréal

![image](https://github.com/user-attachments/assets/7333c1ef-98a4-4b92-9c57-a27a9a86f66b)

   Au lieu de telecharger le fichier, on aura a copier l'adresse lien puis l'ajouter au Reader sur FME

   Puis on suit les étapes suivantes sur Reader-FME,tout en prenant en compte le format CSV ou GeoJson du fichier :

   Arbres en CSV ;
   
![image](https://github.com/user-attachments/assets/13d2cf26-b5ce-4d44-8eb4-a3324ff92787)

 Parcs en GeoJson ;

 ![image](https://github.com/user-attachments/assets/d5eca360-0d62-42ea-b4f6-698dfe764bd5)
 

![image](https://github.com/user-attachments/assets/324d9994-5fa7-4186-982a-3946f27c28d5)


3. 🌍 Étape 3 : Reprojection des données

Reprojeter les couches de données (arbres et parcs) en EPSG:32188.
Utiliser le transformer Reprojector dans FME.

![image](https://github.com/user-attachments/assets/352ca51f-caba-4962-adc6-285d28d0ffb5)




4. Étape 4 : Exportation des données pour visualisation 
Pour cette étape d'exportation vers QGIS on utilise le Writer POSTGIS vers notre base de données :



5. Étape 5 : Visualisation sur QGIS :

on donne accées a notre base de données 



puis on slide nos données voulues 




6. Étape 6 : Symbologie et ajustements de la carte :

  le premier pas a faire c'est de choisir une symbologie pour nos points afin de les mettre en valeur sur notre carte de fond qu'on choisit grace au plugin QGIS ;


  

  et la carte de fond est tirée de Google Satellite






7. Étape 7 :Résultats :

Voici toutes les boulangeries et patisseries dans la ville de montréal 


on y applique un Zoom sur une petite zone ;



et c'est ma boulangerie préférée ; un magnifique Saint-honoré

10. Étape 8
11. Étape 9
12. Étape 10
13. Étape 11
14. Étape 12
15. Étape 13
16. Étape 14

## Résultats
Présentez les résultats obtenus sous forme de texte, tableaux ou graphiques.

## Discussion
Analysez les résultats, comparez-les aux attentes et discutez des éventuelles erreurs.

## Conclusion
Résumé des principaux points et des conclusions tirées du TP.

## Références
Listez les sources et références utilisées pour le TP.


