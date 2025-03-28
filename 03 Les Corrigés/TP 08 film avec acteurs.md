# TP 08 - Film avec acteurs - table de jointures
## :warning: La correction

<img src="../img/c.webp" width="100">  <img src="../img/num/eight.webp" width="100">


<img src="../img/tp/td7/titanic.webp" width="80">

## Le modèle relationnel
<img src="../img/db-svg/09-film_has_acteur.png" width="600">

## Les données
<img src="../img/xl/05-jointure.png" width="600">

## Pour rappel voici la structure de la table avec les données
```sql
DROP DATABASE IF EXISTS prime_vdo;
CREATE DATABASE prime_vdo CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE prime_vdo;

CREATE TABLE film (
  id INT  NOT NULL AUTO_INCREMENT,
  nom VARCHAR(100) NOT NULL,
  CONSTRAINT pk_film PRIMARY KEY(id)
)ENGINE=INNODB;

CREATE TABLE acteur (
  id INT NOT NULL AUTO_INCREMENT,
  prenom VARCHAR(100) NOT NULL,
  nom VARCHAR(100) NOT NULL,
   CONSTRAINT pk_acteur PRIMARY KEY(id)
)ENGINE=INNODB;

CREATE TABLE film_has_acteur (
  film_id INT NOT NULL,
  acteur_id INT NOT NULL,
  CONSTRAINT pk_film_has_acteur PRIMARY KEY (film_id, acteur_id)
)ENGINE=INNODB;

ALTER TABLE film_has_acteur ADD CONSTRAINT fk_acteur FOREIGN KEY (acteur_id) REFERENCES acteur (id);
ALTER TABLE film_has_acteur ADD CONSTRAINT fk_film FOREIGN KEY (film_id) REFERENCES film (id);

##############
## Les données
##############

INSERT INTO acteur (id, prenom, nom) VALUES
(1, 'Brad', 'PITT'),
(2, 'Léonardo', 'Dicaprio');

INSERT INTO film (id, nom) VALUES
(1, 'Fight Club'),
(2, 'Once Upon a time in Hollywood');

INSERT INTO film_has_acteur 
(film_id, acteur_id) 
VALUES 
('1', '1'), 
('2', '1'), 
('2', '2');
```


:one: Afficher tous les films de Brad PITT 

| film | acteur_prenom | acteur_nom |
|--- |--- |--- |
|Once Upon the time |  Brad | PITT |
|Fight Club |  Brad | PITT |

```sql
USE prime_vdo;

SELECT 
film.nom AS film,
acteur.prenom,
acteur.nom
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
INNER JOIN acteur ON acteur.id = film_has_acteur.acteur_id
WHERE acteur_id=1
```
:two: Afficher le nombre de films par acteur
| acteur_prenom | acteur_nom |  nb_films | 
|--- |--- |--- |
|  Leonardo | DICAPRIO | 1|
| Brad | PITT | 2 |

```sql
USE prime_vdo;

SELECT 
COUNT(film.id) AS nb_films,
acteur.prenom,
acteur.nom
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
INNER JOIN acteur ON acteur.id = film_has_acteur.acteur_id
GROUP BY (acteur.id);
```

:three: Ajouter un film TITANIC
```sql
USE prime_vdo;
INSERT INTO film (nom) VALUES ('TITANIC');
```
:four: Trouver le film qui n'a pas d'acteur
| film | 
|--- |
|  TITANIC |


```sql
USE prime_vdo;

SELECT 
film.nom, 
acteur_id
FROM film
LEFT JOIN film_has_acteur on film.id=film_has_acteur.film_id
WHERE acteur_id IS NULL
```
:five: Associer Leonardo DICAPRIO dans le film TITANIC
```sql
USE prime_vdo;
INSERT INTO film_has_acteur (film_id,acteur_id) VALUES (3,2);
```
:six: Afficher tous les film avec acteurs 
 | film | acteur_prenom | acteur_nom |
|--- |--- |--- |
|Once Upon the time |  Leonardo | DICAPRIO |
|Once Upon the time |  Brad | PITT |
|Fight Club |  Brad | PITT |
|TITANIC |  Leonardo | DICAPRIO |
```sql
USE prime_vdo;

SELECT 
film.nom AS film,
acteur.prenom,
acteur.nom
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
INNER JOIN acteur ON acteur.id = film_has_acteur.acteur_id
```

:seven: Ajouter un acteur TOM CRUISE  
```sql
USE prime_vdo;
INSERT INTO acteur (prenom,nom) VALUES ('TOM','CRUISE');
```

:eight: Afficher le nombre de films par acteur en incluant TOM CRUISE
| acteur_prenom | acteur_nom |  nb_films | 
|--- |--- |--- |
| Brad | PITT | 2 |
|  Leonardo | DICAPRIO | 1|
| TOM | CRUISE | 0 |

```sql
USE prime_vdo;
SELECT 
acteur.prenom,
acteur.nom,
COUNT(film.id) AS nb_films
FROM acteur
LEFT JOIN film_has_acteur  ON acteur.id = film_has_acteur.acteur_id
LEFT JOIN film ON film.id = film_has_acteur.film_id
GROUP BY (acteur.id);
```


**Autre possiblité**

```sql
USE prime_vdo;
SELECT 
acteur.prenom,
acteur.nom,
COUNT(film.id) AS nb_films
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
RIGHT JOIN acteur ON acteur.id = film_has_acteur.acteur_id
GROUP BY (acteur.id);
```

**9** - Afficher les acteurs ayant jouer dans 2 films avec <code>HAVING</code>
| acteur_prenom | acteur_nom |  nb_films | 
|--- |--- |--- |
|  Leonardo | DICAPRIO | 2|
| Brad | PITT | 2 |
```sql
USE prime_vdo;
SELECT 
acteur.prenom,
acteur.nom,
COUNT(film.id) AS nb_films
FROM acteur
INNER JOIN film_has_acteur  ON acteur.id = film_has_acteur.acteur_id
INNER JOIN film ON film.id = film_has_acteur.film_id
GROUP BY (acteur.id)
HAVING nb_films >= 2;
```

**10** - En moyenne Combien d'acteurs jouent dans 1 film ?
| acteur_par_film |
|--- |
| 1,3333 |
```sql
USE prime_vdo;

SELECT AVG(info.nb_films) AS acteurs_par_film
FROM (
SELECT 
acteur.prenom,
acteur.nom,
COUNT(film.id) AS nb_films
FROM acteur
LEFT JOIN film_has_acteur  ON acteur.id = film_has_acteur.acteur_id
LEFT JOIN film ON film.id = film_has_acteur.film_id
GROUP BY (acteur.id)) AS info;
```
**Autre possibilé avec création d'un table temporaire**
```sql
USE prime_vdo;
-- CREATE TEMPORARY TABLE IF NOT EXISTS table2 AS (SELECT * FROM table1)
CREATE TEMPORARY TABLE IF NOT EXISTS temp AS
(
  SELECT 
  acteur.prenom,
  acteur.nom,
  COUNT(film.id) AS nb_films
  FROM acteur 
  LEFT JOIN film_has_acteur  ON acteur.id = film_has_acteur.acteur_id
  LEFT JOIN film ON film.id = film_has_acteur.film_id
  GROUP BY (acteur.id) 
);

SELECT AVG(nb_films) AS acteurs_par_film
FROM temp;
DROP TABLE temp;
```

**11** - Effacer les 3 tables avec <code>DROP TABLE</code>  
```sql
DROP TABLE film_has_acteur;
DROP TABLE film;
DROP TABLE acteur;
```  
OU bien 

```sql
USE prime_vdo;

ALTER TABLE film_has_acteur DROP FOREIGN KEY fk_acteur;
ALTER TABLE film_has_acteur DROP FOREIGN KEY fk_film;
DROP table acteur;
DROP table film;
DROP table film_has_acteur;
```
OU bien 
```sql
DROP DATABASE prime_vdo;
```