# Compte Rendu du laboratoire 2 :

## Introduction 🚀
Déscription des étapes relatives au bon deroulement du **laboratoire 2** 

## Matériel et Méthodes🖥️🛠️

### Matériel⚙️
- FME Workbench 
- QGIS
  
### Méthodes 📅

#### Étapes📝
1. Étape 1 :Ouvrir FME et Création d'un nouveau projet : 📂

2. Étape 2 :Lecture du CSV et GeoJson📖

   1- Input : Arbres et Quartiers

   Au lieu de telecharger le fichier, on aura a copier l'adresse lien puis l'ajouter au Reader sur FME

   Puis on suit les étapes suivantes sur Reader-FME,tout en prenant en compte le format CSV ou GeoJson du fichier :
   
![image](https://github.com/user-attachments/assets/4c6546f1-fd1d-4e8d-b613-fe71e1f2f097)

![image](https://github.com/user-attachments/assets/b01db50b-fd02-4573-9c3c-4c931bb3b1d8)


![image](https://github.com/user-attachments/assets/3b1b22db-8371-4911-a459-b0dda066ba30)


HOURAAA,le fichier s'est lié sans erreurs.


⚠️⚠️Mais on peut voir quelques erreurs dans les ecritures (des accents....)
Pour remedier a ce probléme on suit les étapes suivantes :

![image](https://github.com/user-attachments/assets/85198a41-f22a-4aca-a6bd-e8486d352796)


Et voila,pas d'erreur d'ecriture ✔️

![image](https://github.com/user-attachments/assets/d42d0a9e-8fe7-4398-9c7d-1217306bd762)



3. Étape 3: Reprojection :

Reprojeter en **MTM8 (EPSG:32188)** garantit l’alignement des couches SIG, améliore la précision des analyses locales et minimise les distorsions. Ce système est adapté aux régions spécifiques comme le Québec, respectant les standards géospatiaux locaux.

![image](https://github.com/user-attachments/assets/ce14da8c-8bf4-4fd1-8fbc-fcbd792f44eb)

4. Étape 4 : Jointure spatiale

cette partie est necessaire afin de connaitre le nombre de points dans chaque polygone de quartiers

Pour cela on utilise le PointOnAreaOverlayer

![image](https://github.com/user-attachments/assets/e4dd89b1-7697-428f-9e5f-3b62a1ceca46)
 ![image](https://github.com/user-attachments/assets/bc9aacb5-11a3-4fdb-87c8-761c75d1be77)

5. Étape 5 : Nettoyage et modification des attributs :


On utilise l'attribute Manager pour se debarasser des données non necessaire et les noms d'attributs a changer comme suit :
 
 ![image](https://github.com/user-attachments/assets/1ac34dea-91d3-4e95-adea-2bf50558ad8e)

 6. Étape 6 : Calcul de statistique

![image](https://github.com/user-attachments/assets/0caf301d-d385-4443-bf45-9a96043243ac)![image](https://github.com/user-attachments/assets/26829d06-0540-456f-b947-b8c4c9d85a1c)


en utilisant la formule suivante 

![image](https://github.com/user-attachments/assets/018a2c40-cbe2-4cd5-bcd5-01e5f99c53ee)

et on aura comme résultat :

![image](https://github.com/user-attachments/assets/6b5de571-01ef-4ca0-81a1-72f5356a930b)

 
7. Étape 7 : Exportation vers QGIS🌐 :

cela se fait avec un Writer comme suit :

![image](https://github.com/user-attachments/assets/f13cd9ed-5d77-4a07-94e7-76da47194fea)


![image](https://github.com/user-attachments/assets/48b0f3c2-70a6-4f18-b1b1-64afbffa78f5)


8. Étape 8 : Visualisation sur Qgis :

   On slide la couche

![image](https://github.com/user-attachments/assets/0a46be49-d66e-4bcf-9e5d-33f779636998)


9. Étape 9 :Symbologie et ajustements de la carte

cette étape consiste a modifier a faire la symbologie adécquate 

9.1 carte de la quantité d'arbres dans chaque quartier :

![image](https://github.com/user-attachments/assets/21e51728-a790-4d94-8a87-69032d2a9c60)

on ajoute un fond de carte ESRI

![image](https://github.com/user-attachments/assets/4d149750-ef86-4a99-ae7e-25c43339362d)

![image](https://github.com/user-attachments/assets/dcd42af0-ffb4-41c1-8343-eb5dc8eda334)


10. Étape 10 :Résultats :

FME 

![image](https://github.com/user-attachments/assets/72ab105f-eda0-4557-b670-5bb60da9fc73)






## Résultats.

Ce laboratoire m'a permis de familiariser avec les outils FME de base et de visualiser mes données de FME sur QGIS dans une dimension plus avancée que la 1ere a ajouter des attributs et de calculer des statistique.
