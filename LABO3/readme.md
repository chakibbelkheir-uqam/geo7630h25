# 📚 Compte Rendu — Laboratoire 3

## 🚀 Introduction
Description des étapes relatives au déroulement du **laboratoire 3**.

---

## 🛠️ Matériel et Méthodes

### ⚙️ Matériel utilisé
- **FME Workbench**
- **QGIS**

---

### 📅 Étapes

### 1. Création d'un projet FME 📂
- Ouvrir **FME Workbench** et créer un **nouveau projet**.

---

### 2. Lecture des fichiers CSV et GeoJSON 📖
- **Sources** :
  - Arbres de Montréal (CSV)
  - Parcs de Montréal (GeoJSON)

![image](https://github.com/user-attachments/assets/7333c1ef-98a4-4b92-9c57-a27a9a86f66b)
![image](https://github.com/user-attachments/assets/13d2cf26-b5ce-4d44-8eb4-a3324ff92787)
![image](https://github.com/user-attachments/assets/d5eca360-0d62-42ea-b4f6-698dfe764bd5)
![image](https://github.com/user-attachments/assets/324d9994-5fa7-4186-982a-3946f27c28d5)

---

### 3. Reprojection EPSG:32188 🌍
- Utilisation du transformer **Reprojector**.

![image](https://github.com/user-attachments/assets/352ca51f-caba-4962-adc6-285d28d0ffb5)

---

### 4. Jointure spatiale (Points dans Polygones) 📌
- Associer chaque arbre au parc qui le contient.

![image](https://github.com/user-attachments/assets/a6d0f559-aaef-4766-9eee-1f151971caf9)
![image](https://github.com/user-attachments/assets/28be6df4-758e-4065-ad82-36a2a5b8dd67)
![image](https://github.com/user-attachments/assets/460e7369-5fb1-4723-b05d-ee64cc153939)

---

### 5. Nettoyage des données 🧹
- Utilisation de **AttributeKeeper**.

![image](https://github.com/user-attachments/assets/142ab431-9eb1-4ff4-be7d-f62e13f525bb)

---

### 6. Calcul de la densité d'arbres 📈
- Utilisation de **AttributeCreator** et **AttributeManager**.

![image](https://github.com/user-attachments/assets/0fdfdad7-66dc-4f7a-8d6a-542906cd2379)
![image](https://github.com/user-attachments/assets/27143600-4a67-490a-82cb-b45e84222914)
![image](https://github.com/user-attachments/assets/c79bfaef-8d24-4fcc-b3bf-119e199a993d)

- Calcul de la médiane avec **StatisticsCalculator** :

![image](https://github.com/user-attachments/assets/7c039578-32be-4ae8-e763398a3ee0)

- Calcul de l'index de densité :

![image](https://github.com/user-attachments/assets/746b4828-c598-4d5c-a295-19eaeba8a6ce)
![image](https://github.com/user-attachments/assets/41c41f8a-95a3-4185-8c6f-28427281f291)

---

### 7. Nettoyage final 🧼
- Nettoyer les valeurs vides avec **NullAttributeMapper**.

![image](https://github.com/user-attachments/assets/ca668ab5-78ff-40ab-aa9e-f6f67b6d20a5)

---

### 8. Exportation vers QGIS 🌐
- Utilisation du **Writer PostGIS**.

![image](https://github.com/user-attachments/assets/cea1e5a4-b9da-448b-a2b0-a208c1a8171e)

---

### 9. Visualisation sur QGIS 🎨
- Chargement et affichage des résultats.

![image](https://github.com/user-attachments/assets/89d25f0e-a6b1-4b36-ab33-55505f4a5973)

---

## 🧩 Partie 2 — Analyse par H3 Hexagonal

### 10. Création de la grille hexagonale 🔷
- Génération avec **H3HexagonalIndexer**.

![image](https://github.com/user-attachments/assets/f51df78a-2fbd-4c48-83ab-d4989128e5a5)

ℹ️ **Attention** : Ne pas oublier le **Reprojector**.

---

### 11. Jointure spatiale avec les arbres 🌳
- Utilisation de **PointOnAreaOverlayer**.

![image](https://github.com/user-attachments/assets/89d11642-420f-4602-8578-822a673ec2e6)

---

### 12. Sélection et renommage d'attributs ✍️
- Sélection avec **AttributeKeeper**.
- Renommage avec **AttributeRenamer**.

![image](https://github.com/user-attachments/assets/f84eac56-7e6f-46bf-886e-b714314b652e)
![image](https://github.com/user-attachments/assets/0396d7b0-4404-495f-8a49-f99ff463643a)

---

### 13. Exportation vers QGIS 🚀
- Writer PostGIS pour hexagones.

![image](https://github.com/user-attachments/assets/79deeba4-7335-4602-9eec-77d2f66c324b)

---

### 14. Visualisation avancée dans QGIS 🎯
- Application d'une palette de couleurs sur les hexagones.

![image](https://github.com/user-attachments/assets/2fca22b4-4a55-481b-8c3e-df8c28756f2a)

---

## 📈 Résultats

- Comparaison entre **analyse classique** (par parc) et **analyse par grille hexagonale H3**.
- Analyse plus fine et visualisation spatiale détaillée des densités d'arbres.

---

