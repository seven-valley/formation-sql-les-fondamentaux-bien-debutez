# Module 04 - Extraire les données

<a href="../00 Les fichiers PDF - Supports de cours/04 Extraire les données.pdf">
  <img src="../img/mod/m4.webp" width="300">
</a>  
<br>
<a href="../00 Les fichiers PDF - Supports de cours/04 Extraire les données.pdf">
Le PDF : 04 Extraire les données
</a> 

## Pour rappel voici la structure de la table avec les données 
```sql
DROP DATABASE IF EXISTS videotheque;
CREATE DATABASE videotheque CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE videotheque;
CREATE TABLE film (
    id INT NOT NULL AUTO_INCREMENT,
    titre VARCHAR(255) NOT NULL,
    sortie DATE NOT NULL,
    CONSTRAINT pk_film PRIMARY KEY(id)
) ENGINE=InnoDB; 
# les données ou data
INSERT INTO film (titre,sortie) VALUES
('STAR WARS','1977/05/25'),
('THE MATRIX','1999/06/23'),
('PULP FICTION','1994/10/26');
``` 
  
## SELECT
La commande **SELECT** permet d'extraire tous les films avec <code>*</code>
```sql
SELECT * FROM film
```
Cette commande affiche :  
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |
| 2 | THE MATRIX | 1999/06/23 |
| 3 | PULP FICTION | 1994/10/26 |
  
-----------------------------------------------------------------------------------
  
Il préférable de cibler les champs :
Chaque champ est séparé par une virgule <code>,</code>
En effet <code>*</code> est trop gourmande en ressource  

```sql
SELECT id,titre FROM film
```
Cette commande affiche :  
| id | titre |
|---|---|
| 1 | STAR WARS |
| 2 | THE MATRIX |
| 3 | PULP FICTION |

## AS
Je peux aussi définir des alias avec **AS**  
Il est préférable de mettre les champs sur plusieurs lignes pour plus de lisibilité  
```sql
SELECT 
id AS numero,
titre AS nom
FROM film
```
Cette commande affiche :  
| numero | nom |
|---|---|
| 1 | STAR WARS |
| 2 | THE MATRIX |
| 3 | PULP FICTION |
  
## WHERE
Je peux aussi mettre une condition avec **WHERE**
```sql
SELECT 
id,titre,sortie
FROM film
WHERE id=1
```
Cette commande affiche :  
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |


## AND
je peux filtre avec **WHERE** et **AND**  
```sql
SELECT 
id,titre,sortie
FROM film
WHERE titre='STAR WARS'
AND id=1
```
Cette commande affiche :  
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |

## LIMIT
je peux aussi limiter le nombre de résultats avec **LIMIT** 
```sql
SELECT 
id,titre,sortie
FROM film
LIMIT 2
```
Cette commande affiche :  
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |
| 2 | THE MATRIX | 1999/06/23 |

  
## ORDER BY
Pour classer les films par date j'utilise <code>ORDER BY</code>
avec <code>ASC</code> ou <code>DESC</code>
```sql
SELECT 
id,titre,sortie
FROM film
ORDER BY sortie DESC
```

Cette commande affiche :  
| id | titre | sortie |
|---|---|---|
| 2 | THE MATRIX | 1999/06/23 |
| 3 | PULP FICTION | 1994/10/26 |
| 1 | STAR WARS | 1977/05/25 |

## COUNT
Pour compter le nombre de films j'utilise <code>COUNT()</code>

```sql
SELECT 
COUNT(id) AS nb_films
FROM film
```

Cette commande affiche :  
| nb_films |
|---|
| 3 | 

## LIKE
```sql
SELECT 
id,
titre,
sortie
FROM film 
WHERE titre LIKE 'st%'
```
Cette commande affiche : 
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |

# SUM

# AVG

# COUNT GROUP BY
