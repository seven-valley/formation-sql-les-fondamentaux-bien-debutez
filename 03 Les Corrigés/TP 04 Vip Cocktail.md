# :cocktail: TP 4 VIP Cocktail - partie 2
## :warning: La correction
<img src="../img/c.webp" width="100"> <img src="../img/num/four.webp" width="100"> 

  
| id | prenom | nom | age | inscription | etat | statut | cv | salaire |
|---|---|---|---|---|---|---|---|---|
| 1 | Brad | PITT | 60 | 01/01/1970 | 1 | non membre | lorem ipsum | 2 000 000 |
| 2 | George | Cloney | 62 | 01/01/1999 | 1 | membre  | juste beau | 4 000 000 |
| 3 | Jean | DUJARDIN | 51 | 01/01/1994 | 0 | membre | brice de nice | 1 000 000 |
    
![brad](../img/tp/tp2/brad.webp)
![george](../img/tp/tp2/george.webp)
![jean](../img/tp/tp2/jean.webp)
## Pour rappel voici la structure de la table
```sql

DROP DATABASE IF EXISTS invitation;
CREATE DATABASE invitation CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE invitation;

CREATE TABLE inv_personne(
    id int NOT NULL AUTO_INCREMENT, 
    prenom VARCHAR(100) NOT NULL,
    nom VARCHAR(100) NOT NULL,
    age INT NOT NULL,
    inscription DATE NOT NULL,
    etat TINYINT NOT NULL DEFAULT 1, 
    statut ENUM('membre','non membre') NOT NULL DEFAULT 'non membre',
    cv TEXT, 
    salaire INT NOT NULL,
    CONSTRAINT pk_personne PRIMARY KEY(id) 
) ENGINE=InnoDB; 
```

# La correction :
:one: - Ajouter les données 
```sql
-- SELECTIONNER La base de données
USE invitation;
-- vider la table de ses données
TRUNCATE inv_personne;
-- inserer DATA données
INSERT INTO inv_personne 
    (prenom,nom,age,inscription,etat,statut,cv,salaire)  
VALUES
('Brad','PITT',	60,'1970-01-01',1,'non membre','lorem ipsum', 2000000),
('George','CLONEY',62,'1999-01-01',1,'membre','juste beau',4000000),
('Jean','DUJARDIN',51,'1994-01-01',0,'membre','brice de nice',1000000);
```       

:two: - Afficher le plus gros salaire (avec MAX)  [doc max w3](https://www.w3schools.com/sql/func_mysql_max.asp)    
  
| plus_gros_salaire |
|---|
| 4000000 |
```sql
USE invitation;
SELECT 
    MAX(salaire) AS plus_gros_salaire 
FROM inv_personne;
```
  
:three: - Afficher le plus petit salaire (avec MIN)  [doc min w3](https://www.w3schools.com/sql/func_mysql_min.asp) 
   
| plus_petit_salaire |
|---|
| 1000000 |
```sql
USE invitation;
SELECT 
    MIN(salaire) AS plus_petit_salaire 
FROM inv_personne;
```
  
:four: - Afficher le nom de l'acteur (et son salaire) qui a le plus petit salaire avec <code>LIMIT</code> & <code>ORDER BY</code>
    
| prenom | nom | salaire |
|---|---|---|
| Jean | DUJARDIN | 1000000 |
  
```sql
USE invitation;
SELECT 
prenom,nom,salaire
FROM inv_personne 
ORDER BY salaire ASC
LIMIT 1;
```

:five: - Afficher le nom de l'acteur (et son salaire) qui a le plus gros salaire avec <code>LIMIT</code> & <code>ORDER BY</code>
  
| prenom | nom | salaire |
|---|---|---|
| George | CLONEY | 4000000 |
  
```sql
USE invitation;
SELECT 
prenom,nom,salaire
FROM inv_personne 
ORDER BY salaire DESC
LIMIT 1;
```     
:six: - Afficher le salaire moyen
  
| salaire_moyen |
|---|
| 2333333.3333 |   

```sql
USE invitation;
SELECT 
AVG(salaire) AS salaire_moyen 
FROM inv_personne
```

:seven: - Afficher le nombre de personnes  
  
| nb_personnes |
|---|
| 3 | 

```sql
USE invitation;
SELECT 
COUNT(id) AS nb_personnes
FROM inv_personne
```  
:eight: - Afficher les acteurs avec un salaire entre 1 000 000 et 4 000 000 avec <code>BETWEEN</code>
  
| id | prenom | nom | salaire |
|---|---|---|---|
| 1 | Brad | PITT | 2000000 |

```sql
USE invitation;
SELECT
prenom,nom,salaire
FROM inv_personne
WHERE salaire BETWEEN 1000001 AND 3999999;
``` 
**alternative** avec <code>WHERE</code> & <code>AND</code>  

```sql
USE invitation;
SELECT
    prenom,nom
FROM inv_personne
WHERE salaire > 1000000
AND salaire < 4000000;
```  
:nine: Proposer une requete avec  <code>UPPER()</code> & <code>LOWER()</code> 
  
| id | prenom | nom |
|---|---|---|
| 1 | Brad | pitt | 

```sql
USE invitation;
SELECT
    id,
    prenom,
    LOWER(nom) AS nom
FROM inv_personne
WHERE id=1;
```
--------------------------------------------------

| id | prenom | nom |
|---|---|---|
| 1 | BRAD | pitt | 
```sql
USE invitation;
SELECT
    id,
    UPPER(prenom) AS prenom,
    nom
FROM inv_personne
WHERE id=1;
```

**10** - Afficher les personnes dont le prenom contient 'bra'  
  
| id | prenom | nom | salaire |
|---|---|---|---|
| 1 | Brad | PITT | 2 000 000 |

```sql
SELECT nom, prenom ,salaire
FROM personne
WHERE prenom LIKE '%bra%'
```   

**11** - Trier par age les membres 
   
| prenom | nom | age |
|---|---|---|
| Jean | DUJARDIN | 51 |
| George | Cloney | 62 |

 ```sql
SELECT 
prenom,nom,age
FROM inv_personne
WHERE type='membre'
ORDER BY age,nom ASC; # du plus petit au plus grand
``` 
**12** - Afficher le nombre d'acteurs "membre" 
  
| nb_membres |
|---|
| 2 | 

```sql
USE invitation;
SELECT
    COUNT(id) AS nb_membres
FROM inv_personne
WHERE statut ='membre'
GROUP BY statut
```


**13** - Afficher le nombre des membres et  d'acteur "non membre"
   
| membre | nb_acteur| 
|---|---|
| non membre | 1 | 
| membre | 2 | 


```sql
USE invitation;
SELECT
    statut, COUNT(id)
FROM inv_personne
GROUP BY statut
```
