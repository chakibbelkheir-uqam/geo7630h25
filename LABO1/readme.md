# Compte Rendu du laboratoire 1 :

## Introduction 🚀
Déscription des étapes relatives au bon deroulement du **laboratoire 1** 

## Matériel et Méthodes🖥️🛠️

### Matériel⚙️
- FME Workbench 
- QGIS
  
### Méthodes 📅

#### Étapes📝
1. Étape 1 :Ouvrir FME et Création d'un nouveau projet : 📂

![alt text][def2]
![alt text][def]

2. Étape 2 :Lecture du CSV 📖

   1- Input : Établissements alimentaires de Montréal
   
Avec les données d'adresse,Longitude et Latitude

Ces données sont extraites du site des données ouvertes de la ville de Montréal

![alt text][def3]
![alt text][def4]

   ![alt text][def5]

   Au lieu de telecharger le fichier, on aura a copier l'adresse lien puis l'ajouter au Reader sur FME

   ![alt text][def6]

Puis on suit les étapes suivantes sur Reader-FME,tout en prenant en compte le format CSV du fichier :

![alt text][def7]

HOURAAA,le fichier s'est lié sans erreurs.

![alt text][def8]

⚠️⚠️Mais on peut voir quelques erreurs dans les ecritures (des accents....)
Pour remedier a ce probléme on suit les étapes suivantes :

![alt text][def9]
![alt text][def11]

Et voila,pas d'erreur d'ecriture ✔️


![alt text][def10]

3. Étape 3: Filtrage 🧹 et  Création de points d'entités 🔍

cette étape se fait par le transformeur vertex creator pour afficher tout les commerces en forme de points,
pour ma part je me suis fait un petit plaisir de filtrer mes commerces pour les boulangeries et patisseries et qui ont le statut ouvert avant de continuer 

![image](https://github.com/user-attachments/assets/35c58bc2-56ca-4f56-a0f3-fb15b997544e)  ![image](https://github.com/user-attachments/assets/f69a1531-95c5-4513-8e6c-d849936f9e3d)

puis parametrer le vertex creator 

![image](https://github.com/user-attachments/assets/b7113dcf-8c00-4846-8a95-b38937a2386f)




4. Étape 4 : Exportation des données pour visualisation 

Pour cette étape d'exportation vers QGIS on utilise le Writer POSTGIS vers notre base de données :

![image](https://github.com/user-attachments/assets/f92b6048-2b7b-495f-aa56-634f22516826)

![image](https://github.com/user-attachments/assets/6d665f7b-be92-4967-a6ea-81a988359935)



5. Étape 5 : Visualisation sur QGIS 🎨🌐 :

on donne accées a notre base de données 


![image](https://github.com/user-attachments/assets/34d70a9f-a4d6-4421-b7fb-907d89e13608)

puis on slide nos données voulues 

![image](https://github.com/user-attachments/assets/d36de937-46a2-4778-87d2-4f4c5fa1f38f)



6. Étape 6 : Symbologie et ajustements de la carte :

  Le premier pas a faire c'est de choisir une symbologie pour nos points afin de les mettre en valeur sur notre carte de fond qu'on choisit grace au plugin QGIS ;

  ![image](https://github.com/user-attachments/assets/058e2121-c4be-41b1-8e41-7600e7149dc1)
  

   La carte de fond est tirée de Google Satellite

  ![image](https://github.com/user-attachments/assets/4484210b-db90-4f78-8757-3f3ff406b726)





7. Étape 7 :Résultats :

Voici toutes les boulangeries et patisseries dans la ville de montréal 

![image](https://github.com/user-attachments/assets/4588bd28-9846-4872-babd-420bb75603be)

On y applique un Zoom sur une petite zone ;

![image](https://github.com/user-attachments/assets/1a421075-ee76-4c4d-a469-90d9f4f559f6)


**Et c'est ma boulangerie préférée ; un magnifique Saint-honoré**



## Résultats.

Ce laboratoire m'a permis de familiariser avec les outils FME de base et de visualiser mes données de FME sur QGIS.



[def]: image-1.png
[def2]: image.png
[def3]: image-4.png
[def4]: image-3.png
[def5]: image-2.png
[def6]: image-5.png
[def7]: image-7.png
[def8]: image-9.png
[def9]: image-10.png
[def10]: image-12.png
[def11]: image-11.png
