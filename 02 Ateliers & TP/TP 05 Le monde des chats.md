# TP 5 - Le mondes des chats avec une clef étrangère
<img src="../img/num/five.webp" width="100">
<img src="../img/db-svg/05-chat-couleur.png" width="400">
Prise en main des commandes :  
    
<code>INNER JOIN</code>    
<code>LEFT JOIN</code>    

<img src="../img/xl/02-tp-chat.png" width="600">

  
![maincoon](../img/tp/tp1/maincoon.webp)
![siamois](../img/tp/tp1/siamois.webp)
![bengal](../img/tp/tp1/bengal.webp)
![scottish](../img/tp/tp1/scottish.webp)



# Objectifs :
:one: Création de la base de données **spa**  
:two: Création de la table **chat**  
:three: Creation de la table **couleur**
```sql
# 1 Création de la base de données spa
DROP DATABASE IF EXISTS spa;
-- CREATION DE LA DATA BASE
CREATE DATABASE spa CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE spa;

# 2 Création de la table chat  
CREATE TABLE chat (
 id int NOT NULL AUTO_INCREMENT,
 nom VARCHAR(50) NOT NULL,
 age INT NOT NULL,
 couleur_id int NULL, # le champ peut etre null
 CONSTRAINT pk_chat PRIMARY KEY (id)
)ENGINE=INNODB;

# 3 Creation de la table couleur 
CREATE TABLE couleur (
 id int NOT NULL AUTO_INCREMENT,
 nom VARCHAR(50) NOT NULL,
 CONSTRAINT pk_couleur PRIMARY KEY (id)
)ENGINE=INNODB;

ALTER TABLE chat ADD CONSTRAINT fk_couleur FOREIGN KEY (couleur_id) REFERENCES couleur(id);
```
  
:four: Insérer  les données  
:five: Afficher les chats avec les couleurs des yeux avec <code>INNER JOIN</code>
| id | nom | yeux | age |
|---|---|---|---|
| 1 | Maine coon | marron | 20 |
| 2 | Siamois | bleu | 15 |
| 3 | Bengal | marron | 18 |
| 4 | Scottish Fold | marron | 10 | 

:six: Afficher les chats avec les couleurs des yeux avec le chat domestique avec <code>LEFT JOIN</code>
| id | nom | yeux | age |
|---|---|---|---|
| 1 | Maine coon | marron | 20 |
| 2 | Siamois | bleu | 15 |
| 3 | Bengal | marron | 18 |
| 4 | Scottish Fold | marron | 10 | 
| 5 | Domestique | PAS DE COULEURS | 21 | 

:seven: Afficher le chat qui n'a pas de couleur des yeux
| id | nom | age |
|---|---|---|
| 5 | Domestique | 21 | 

:eight: Afficher le nombre de chats par couleur des yeux
| couleur | nb_chat |
|---|---|
| marron | 3 |
| bleu | 1 |

:nine: Afficher le nombre de chats par couleur des yeux avec la couleur "vert"
| couleur | nb_chat |
|---|---|
| marron | 3 |
| bleu | 1 |
| vert | 0 |


**10** Afficher la moyenne de couleur des yeux attribuer par chat :cactus::cactus::cactus::cactus:  
| moyenne_couleur_yeux |
|---|
| 1.3333 |