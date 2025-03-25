# Module 03 - Insertion et modification des données 


<a href="../00 Les fichiers PDF - Supports de cours/03 Insertion et modification des données.pdf">
  <img src="../img/mod/m3.webp" width="300">
</a>  
<br>
<a href="../00 Les fichiers PDF - Supports de cours/03 Insertion et modification des données.pdf">
Le PDF : 03 Insertion des données
</a> 
<br><br>
  
------------------------------------------
Le CRUD :
- **C** comme **C**REATE
- **R** comme **R**EAD
- **U** comme **U**PDATE
- **D** comme **D**ELETE
  
Nous allons utiliser :  
- **CREATE** avec <code>INSERT</code>  
- **UPDATE** avec <code>UPDATE</code>  
- **DELETE** avec <code>DELETE</code> & <code>TRUNCATE</code>  
------------------------------------------

<img src="../img/tp/td2/star.webp" width="80"> <img src="../img/tp/td2/matrix.webp" width="80"> <img src="../img/tp/td2/pulp.webp" width="80">

  
| id | titre | sortie |
|---|---|---|
| 1 | STAR WARS | 1977/05/25 |
| 2 | THE MATRIX | 1999/06/23 |
| 3 | PULP FICTION | 1994/10/26 |

------------------------------------------
  
Pour insérer les données dans une table, voici la structure:
```sql
INSERT INTO film (...) VALUES (...);
```

:one: Je précise les champs :
```sql
INSERT INTO film (id,titre,sortie) VALUES (...);
```
:two: Et puis je rentre les valeurs :
```sql
INSERT INTO film (id,titre,sortie) VALUES (1,'STAR WARS','1977/05/25');
```

Je ne suis pas obligé de mettre l'ID car nous avons ajouté <code>AUTO_INCERMENT</code> lors de la création de la table 
```sql
INSERT INTO film (titre,sortie) VALUES ('STAR WARS','1977/05/25');
```

Il est possible d'enchaîner les commandes comme celà :

```sql
INSERT INTO film (...) VALUES
(...),
(...),
(...);
```
**Cela donne** :
```sql
INSERT INTO film (id,titre,sortie) VALUES
(1,'STAR WARS','1977/05/25'),
(2,'THE MATRIX','1999/06/23'),
(3,'PULP FICTION','1994/10/26');
```
**Ou bien sans les ID** :
```sql
INSERT INTO film (titre,sortie) VALUES
('STAR WARS','1977/05/25'),
('THE MATRIX','1999/06/23'),
('PULP FICTION','1994/10/26');
```
## Effacer un ou des enregistrements

:warning: **Penser à vider la table** :scream:  
  

Pour vider **toute** la table (tous les enregistrements):  

Utiliser <code>DELETE</code> pour effacer un film  
Effacer un film  
Effacer un enregistrement
```sql
DELETE FROM film WHERE id=1;
```

  
Pour vider la table avec <code>TRUNCATE</code> et repartir à l'ID=1:
```sql
TRUNCATE film;
```

## Modifier un enregistrement
Je peux modifier 1 champs :  
```sql
UPDATE  film SET 
titre='STAR WARS 2'
WHERE id = 1;
```
Je peux modifier plusieurs champs :  
```sql
UPDATE  film SET 
titre='STAR WARS 2',
annee = '1988'
 WHERE id = 1;
```
Pensez à mettre une virgule <code>,</code> pour séparer chaque ligne