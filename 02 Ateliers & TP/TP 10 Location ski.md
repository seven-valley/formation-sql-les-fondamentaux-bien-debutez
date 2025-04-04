# TP 10 - Location ski
<img src="../img/num/ten.webp" width="100">  <img src="../img/tp/tp10/jeanpaul.webp" width="100">

<img src="../img/db-svg/11-ski.png" width="800">



## Informations 
- Lorsqu’un article est rendu, le champ retour de la table lignesFic est valorisé à la date du jour. 
- L’état initial d’une nouvelle fiche est En Cours (EC). 
- Lorsque tous les articles d’une fiche ont été rendus, la fiche passe en mode Rendu (RE). 
- Lorsque le client règle sa fiche, le champ datePaye de la table fiches est valorisé à la date du jour et la 
fiche passe en état « soldée » (SO).
# les tables
```sql
DROP DATABASE IF EXISTS location_ski;

CREATE DATABASE location_ski CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE location_ski;

CREATE TABLE clients (
    noCli INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    prenom VARCHAR(30),
    adresse VARCHAR(120),
    cpo VARCHAR(5) NOT NULL,
    ville VARCHAR(80) NOT NULL,
    CONSTRAINT pk_clients PRIMARY KEY (noCli)
) ENGINE=InnoDB;

CREATE TABLE fiches (
    noFic INT NOT NULL AUTO_INCREMENT,
    noCli INT NOT NULL,
    dateCrea DATE NOT NULL,
    datePaiement DATE,
    etat ENUM( "EC", "RE", "SO") NOT NULL DEFAULT "EC",
    CONSTRAINT pk_fiches PRIMARY KEY (noFic)
) ENGINE=InnoDB;

CREATE TABLE lignesFic(
    noLig INT NOT NULL AUTO_INCREMENT,
    noFic INT NOT NULL,
    refart CHAR(8) NOT NULL,
    depart DATE NOT NULL,
    retour DATE,
    CONSTRAINT pk_lignesFic PRIMARY KEY (noLig, noFic)
) ENGINE=InnoDB;

CREATE TABLE articles (
    refart CHAR(8) NOT NULL UNIQUE,
    designation VARCHAR(80) NOT NULL,
    codeGam CHAR(5),
    codeCate CHAR(5),
    CONSTRAINT pk_articles PRIMARY KEY (refart)
) ENGINE=InnoDB;

CREATE TABLE grilleTarifs (
    codeGam CHAR(5) NOT NULL,
    codeCate CHAR(5) NOT NULL,
    codeTarif CHAR(5),
    CONSTRAINT pk_grilleTarifs PRIMARY KEY (codeGam, codeCate)
) ENGINE=InnoDB;

CREATE TABLE gammes (
    codeGam CHAR(5) NOT NULL UNIQUE,
    libelle VARCHAR(45) NOT NULL,
    CONSTRAINT pk_gammes PRIMARY KEY (codeGam)
) ENGINE=InnoDB;

CREATE TABLE categories (
    codeCate CHAR(5) NOT NULL UNIQUE,
    libelle VARCHAR(30) NOT NULL,
    CONSTRAINT pk_categories PRIMARY KEY (codeCate)
) ENGINE=InnoDB;

CREATE TABLE tarifs (
    codeTarif CHAR(5) NOT NULL UNIQUE,
    libelle CHAR(30) NOT NULL,
    prixJour DECIMAL(5) NOT NULL,
    CONSTRAINT pk_tarifs PRIMARY KEY (codeTarif)
) ENGINE=InnoDB;

ALTER TABLE grilleTarifs ADD (
    CONSTRAINT fk_grilleTarifs_gammes FOREIGN KEY (codeGam) REFERENCES gammes(codeGam),
    CONSTRAINT fk_grilleTarifs_categories FOREIGN KEY (codeCate) REFERENCES categories(codeCate),
    CONSTRAINT fk_grilleTarifs_tarifs FOREIGN KEY (codeTarif) REFERENCES tarifs(codeTarif)
);

ALTER TABLE articles ADD (
    CONSTRAINT fk_articles_grilleTarifs_codeCate FOREIGN KEY (codeCate) REFERENCES categories(codeCate),
    CONSTRAINT fk_articles_grilleTarifs_codeGam FOREIGN KEY (codeGam) REFERENCES gammes(codeGam)
);

ALTER TABLE lignesFic ADD  (
    CONSTRAINT fk_lignesFic_articles FOREIGN KEY (refart) REFERENCES articles(refart),
    CONSTRAINT fk_lignesFic_fiches FOREIGN KEY (noFic) REFERENCES fiches(noFic)
);

ALTER TABLE fiches ADD
CONSTRAINT fk_fiches_clients FOREIGN KEY (noCli) REFERENCES clients(noCli);
```

# Les données
```sql
USE location_ski;
INSERT INTO clients (noCli, nom, prenom, adresse, cpo, ville) VALUES 
    (1, 'Albert', 'Anatole', 'Rue des accacias', '61000', 'Amiens'),
    (2, 'Bernard', 'Barnabé', 'Rue du bar', '1000', 'Bourg en Bresse'),
    (3, 'Dupond', 'Camille', 'Rue Crébillon', '44000', 'Nantes'),
    (4, 'Desmoulin', 'Daniel', 'Rue descendante', '21000', 'Dijon'),
     (5, 'Ernest', 'Etienne', 'Rue de l’échaffaud', '42000', 'Saint Étienne'),
    (6, 'Ferdinand', 'François', 'Rue de la convention', '44100', 'Nantes'),
    (9, 'Dupond', 'Jean', 'Rue des mimosas', '75018', 'Paris'),
    (14, 'Boutaud', 'Sabine', 'Rue des platanes', '75002', 'Paris');

INSERT INTO fiches (noFic, noCli, dateCrea, datePaiement, etat) VALUES 
    (1001, 14,  DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY),'SO' ),
    (1002, 4,  DATE_SUB(NOW(),INTERVAL  13 DAY), NULL, 'EC'),
    (1003, 1,  DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY),'SO'),
    (1004, 6,  DATE_SUB(NOW(),INTERVAL  11 DAY), NULL, 'EC'),
    (1005, 3,  DATE_SUB(NOW(),INTERVAL  10 DAY), NULL, 'EC'),
    (1006, 9,  DATE_SUB(NOW(),INTERVAL  10 DAY),NULL ,'RE'),
    (1007, 1,  DATE_SUB(NOW(),INTERVAL  3 DAY), NULL, 'EC'),
    (1008, 2,  DATE_SUB(NOW(),INTERVAL  0 DAY), NULL, 'EC');

INSERT INTO tarifs (codeTarif, libelle, prixJour) VALUES
    ('T1', 'Base', 10),
    ('T2', 'Chocolat', 15),
    ('T3', 'Bronze', 20),
    ('T4', 'Argent', 30),
    ('T5', 'Or', 50),
    ('T6', 'Platine', 90);

INSERT INTO gammes (codeGam, libelle) VALUES
    ('PR', 'Matériel Professionnel'),
    ('HG', 'Haut de gamme'),
    ('MG', 'Moyenne gamme'),
    ('EG', 'Entrée de gamme');

INSERT INTO categories (codeCate, libelle) VALUES
    ('MONO', 'Monoski'),
    ('SURF', 'Surf'),
    ('PA', 'Patinette'),
    ('FOA', 'Ski de fond alternatif'),
    ('FOP', 'Ski de fond patineur'),
    ('SA', 'Ski alpin');

INSERT INTO grilleTarifs (codeGam, codeCate, codeTarif) VALUES
    ('EG', 'MONO', 'T1'),
    ('MG', 'MONO', 'T2'),
    ('EG', 'SURF', 'T1'),
    ('MG', 'SURF', 'T2'),
    ('HG', 'SURF', 'T3'),
    ('PR', 'SURF', 'T5'),
    ('EG', 'PA', 'T1'),
    ('MG', 'PA', 'T2'),
    ('EG', 'FOA', 'T1'),
    ('MG', 'FOA', 'T2'),
    ('HG', 'FOA', 'T4'),
    ('PR', 'FOA', 'T6'),
    ('EG', 'FOP', 'T2'),
    ('MG', 'FOP', 'T3'),
    ('HG', 'FOP', 'T4'),
    ('PR', 'FOP', 'T6'),
    ('EG', 'SA', 'T1'),
    ('MG', 'SA', 'T2'),
    ('HG', 'SA', 'T4'),
    ('PR', 'SA', 'T6');

INSERT INTO articles (refart, designation, codeGam, codeCate) VALUES 
    ('F01', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F02', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F03', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F04', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F05', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F10', 'Fischer Sporty Crown', 'MG', 'FOA'),
    ('F20', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F21', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F22', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F23', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F50', 'Fischer SOSSkating VASA', 'HG', 'FOP'),
    ('F60', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F61', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F62', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F63', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F64', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('P01', 'Décathlon Allegre junior 150', 'EG', 'PA'),
    ('P10', 'Fischer mini ski patinette', 'MG', 'PA'),
    ('P11', 'Fischer mini ski patinette', 'MG', 'PA'),
    ('S01', 'Décathlon Apparition', 'EG', 'SURF'),
    ('S02', 'Décathlon Apparition', 'EG', 'SURF'),
    ('S03', 'Décathlon Apparition', 'EG', 'SURF'),
    ('A01', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A02', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A03', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A04', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A05', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A10', 'Salomon Pro Link Equipe 4S', 'PR', 'SA'),
    ('A11', 'Salomon Pro Link Equipe 4S', 'PR', 'SA'),
    ('A21', 'Salomon 3V RACE JR+L10', 'PR', 'SA');

INSERT INTO lignesFic (noFic, noLig,  refart, depart, retour) VALUES 
    (1001, 1, 'F05', DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY)),
    (1001, 2, 'F50', DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  14 DAY)),
    (1001, 3, 'F60', DATE_SUB(NOW(),INTERVAL  13 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY)),
    (1002, 1, 'A03', DATE_SUB(NOW(),INTERVAL  13 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1002, 2, 'A04', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  7 DAY)),
    (1002, 3, 'S03', DATE_SUB(NOW(),INTERVAL  8 DAY), NULL),
    (1003, 1, 'F50', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY)),
    (1003, 2, 'F05', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY)),
    (1004, 1, 'P01', DATE_SUB(NOW(),INTERVAL  6 DAY), NULL),
    (1005, 1, 'F05', DATE_SUB(NOW(),INTERVAL  9 DAY), DATE_SUB(NOW(),INTERVAL  5 DAY)),
    (1005, 2, 'F10', DATE_SUB(NOW(),INTERVAL  4 DAY), NULL),
    (1006, 1, 'S01', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1006, 2, 'S02', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1006, 3, 'S03', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1007, 1, 'F50', DATE_SUB(NOW(),INTERVAL  3 DAY), DATE_SUB(NOW(),INTERVAL  2 DAY)),
    (1007, 3, 'F60', DATE_SUB(NOW(),INTERVAL  1 DAY), NULL),
    (1007, 2, 'F05', DATE_SUB(NOW(),INTERVAL  3 DAY), NULL),
    (1007, 4, 'S02', DATE_SUB(NOW(),INTERVAL  0 DAY), NULL),
    (1008, 1, 'S01', DATE_SUB(NOW(),INTERVAL  0 DAY), NULL);
```

# Partie 1
- Créer la base de données

# Partie 2
Réalisez en priorité les requêtes :one:, :three:, :four:, :six:, :seven:  & :nine:  
  
:warning: La requête :four: est la plus importante à faire !  
    
:one:  Liste des clients (toutes les informations) dont le nom commence par un <code>D</code> 
|noCli|nom|prenom|adresse|cpo|ville|
|---|---|---|---|---|---|
|3|Dupond|Camille|Rue Crébillon|44000|Nantes|
|4|Desmoulin|Daniel|Rue descendante|21000|Dijon|
|9|Dupond|Jean|Rue des mimosas|75018 |Paris| 

:two: Nom et prénom de tous les clients
|prenom|nom|
|---|---|
|Albert|Anatole|
|Bernard|Barnab|
|Dupond|Camille|
|Desmoulin|Daniel|
|Ferdinand|François|
|Albert|Anatole|
|Dupond|Jean|
|Boutaud|Sabine|

:three:  Liste des fiches (n°, état) pour les clients (nom, prénom) qui habitent en Loire Atlantique (44) 
|noFic|etat|nom|prenom|
|---|---|---|---|
|1004|EC|Ferdinand|François|
|1005|EC|Dupond|Camille|

:four: Détail de la fiche n°1002
|noFic|nom|prenom|refart|designation|depart|retour|prixJour|montant|
|---|---|---|---|---|---|---|---|---|
|1002|Desmoulin|Daniel|A03|Salomon 24X+Z12|2024-11-18|2024-11-22|10|50|
|1002|Desmoulin|Daniel|A04|Salomon 24X+Z12|2024-11-19|2024-11-24|10|60|
|1002|Desmoulin|Daniel|S03|Décathlon Apparition|2024-11-23|NULL|10|90|

:five: Prix journalier moyen de location par gamme
|Gamme|tarif journalier moyen|
|---|---|
|Entrée de gamme|10.833333|
|Haut de gamme|27.500000|
|Moyenne gamme|15.833333|
|Matériel Professionnel|80.000000|

:six: Détail de la fiche n°1002 avec le **total**
|noFic|nom|prenom|refart|designation|depart|retour|prixJour|Montant|Total|
|---|---|---|---|---|---|---|---|---|---|
|1002|Desmoulin|Daniel|A03|"Salomon 24X+Z12"|2024-11-18|2024-11-22|10|50|200|
|1002|Desmoulin|Daniel|A04|"Salomon 24X+Z12"|2024-11-19|2024-11-24|10|60|200|
|1002|Desmoulin|Daniel|S03|"Décathlon Apparition"|2024-11-23|NULL|10|90|200|

:seven:  Grille des tarifs
|libelle|libelle|libelle|prixJour|  
|---|---|---|---|
|Ski de fond alternatif|Entrée de gamme|Base|10|
|Ski de fond patineur|Entrée de gamme|Chocolat|15|
|Monoski|Entrée de gamme|Base|10|
|Patinette|Entrée de gamme|Base|10|
|Ski alpin|Entrée de gamme|Base|10|
|Surf|Entrée de gamme|Base|10|
|Ski de fond alternatif|Haut de gamme|Argent|30|
|Ski de fond patineur|Haut de gamme|Argent|30|
|Ski alpin|Haut de gamme|Argent|30|
|Surf|Haut de gamme|Bronze|20|
|Ski de fond alternatif|Moyenne gamme|Chocolat|15|
|Ski de fond patineur|Moyenne gamme|Bronze|20|
|Monoski|Moyenne gamme|Chocolat|15|
|Patinette|Moyenne gamme|Chocolat|15|
|Ski alpin|Moyenne gamme|Chocolat|15|
|Surf|Moyenne gamme|Chocolat|15|
|Ski de fond alternatif|Matériel Professionnel|Platine|90|
|Ski de fond patineur|Matériel Professionnel|Platine|90|
|Ski alpin|Matériel Professionnel|Platine|90|
|Surf|Matériel Professionnel|Or|50|

:eight:  Liste des locations de la catégorie SURF
|refart|designation|nbLocation|
|---|---|---|
|S01|"Décathlon Apparition"|2|
|S02|"Décathlon Apparition"|2|
|S03|"Décathlon Apparition"|2|

:nine: Calcul du nombre moyen d’articles loués par fiche de location 
|nb_lignes_moyen_par_fiche|
|---|
|2.375000|


**10** - Calcul du nombre de fiches de location établies pour les catégories de location Ski alpin, Surf et Patinette
|catégorie|nombre de location|
|---|---|
|Ski alpin|2|
|Patinette|1|
|Surf|6|

**11** Calcul du montant moyen des fiches de location
|montant moyen d'une fiche de location|
|---|
|131.8750|
