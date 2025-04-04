# Team Builder

## le prompt db diagram
https://dbdiagram.io/
```
Table personne{
  id int [pk ,increment]
  prenom varchar(50)
  nom varchar(50)
}

Table equipe{
  id int [pk ,increment]
  nom varchar(50)
}

Table equipe_has_personne{
  personne_id int [pk]
  equipe_id int [pk]
}

Ref: "equipe"."id" < "equipe_has_personne"."equipe_id"

Ref: "personne"."id" < "equipe_has_personne"."personne_id"
```

## le MySQL généré
ajouter:
```
ENGINE=INNODB;
```
et aussi :
```sql
CREATE DATABASE team_builder CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE team_builder;
```

```sql
CREATE DATABASE team_builder CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE team_builder;
CREATE TABLE `personne` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `prenom` varchar(50),
  `nom` varchar(50)
)ENGINE=INNODB;

CREATE TABLE `equipe` (
  `id` int PRIMARY KEY AUTO_INCREMENT,
  `nom` varchar(50)
);

CREATE TABLE `equipe_has_personne` (
  `personne_id` int,
  `equipe_id` int,
  PRIMARY KEY (`personne_id`, `equipe_id`)
);

ALTER TABLE `equipe_has_personne` ADD FOREIGN KEY (`equipe_id`) REFERENCES `equipe` (`id`);

ALTER TABLE `equipe_has_personne` ADD FOREIGN KEY (`personne_id`) REFERENCES `personne` (`id`);
```

# INSERER les données
```sql
USE team_builder;
INSERT INTO personne (prenom,nom) VALUES
('Jean','DUJARDIN'),
('Brad','PITT'),
('Bruce','WILLIS'),
('Angelina','JOLIE');

INSERT INTO equipe (nom)VALUES
('Team A'),
('Team B');

INSERT INTO equipe_has_personne (equipe_id,personne_id)VALUES
(1,1),
(1,2),
(2,3),
(2,4);
```

# Extraire les données
```sql
SELECT 
e.nom AS equipe,
p.prenom AS prenom,
p.nom AS nom
FROM equipe e
INNER JOIN equipe_has_personne ehp ON e.id = ehp.equipe_id
INNER JOIN personne p ON ehp.personne_id = p.id
```
|equipe|prenom|nom|
|---|---|---|
|Team A|Jean|DUJARDIN|
|Team A|Brad|PITT|
|Team B|Bruce|WILLIS|
|Team B|Angelina|JOLIE|