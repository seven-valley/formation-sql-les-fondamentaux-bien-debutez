# TP 10 - Location ski
## :warning: La correction

<img src="../img/c.webp" width="100">  <img src="../img/num/ten.webp" width="100">  <img src="../img/tp/tp10/jeanpaul.webp" width="100">

<img src="../img/db-svg/11-ski.png" width="800">


  

# Partie 2
   
:one:  Liste des clients (toutes les informations) dont le nom commence par un <code>D</code> 
|noCli|nom|prenom|adresse|cpo|ville|
|---|---|---|---|---|---|
|3|Dupond|Camille|Rue Crébillon|44000|Nantes|
|4|Desmoulin|Daniel|Rue descendante|21000|Dijon|
|9|Dupond|Jean|Rue des mimosas|75018 |Paris|
  
```sql
USE location_ski;
SELECT *
FROM clients
WHERE nom LIKE 'D%';
```
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

Nom et prénom de tous les clients
```sql
USE location_ski;
SELECT nom, prenom
FROM clients;
```
:three:  Liste des fiches (n°, état) pour les clients (nom, prénom) qui habitent en Loire Atlantique (44)   
|noFic|etat|nom|prenom|
|---|---|---|---|
|1004|EC|Ferdinand|François|
|1005|EC|Dupond|Camille|

```sql
USE location_ski;
SELECT noFic, etat, nom, prenom
FROM fiches 
INNER JOIN clients USING (noCli)
WHERE cpo LIKE '44%';
```
:four: Détail de la fiche n°1002  
|noFic|nom|prenom|refart|designation|depart|retour|prixJour|montant|
|---|---|---|---|---|---|---|---|---|
|1002|Desmoulin|Daniel|A03|Salomon 24X+Z12|2024-11-18|2024-11-22|10|50|
|1002|Desmoulin|Daniel|A04|Salomon 24X+Z12|2024-11-19|2024-11-24|10|60|
|1002|Desmoulin|Daniel|S03|Décathlon Apparition|2024-11-23|NULL|10|90|

```sql
USE location_ski;
SELECT  f.noFic, nom, prenom, 
		a.refart, designation, depart, retour, prixJour,
        (DATEDIFF(IFNULL(retour, NOW()+1),depart)+1)*prixJour as montant      
FROM 
	fiches f
  INNER JOIN clients c USING (noCli)
  INNER JOIN lignesfic l USING (noFic)
  INNER JOIN articles a ON l.refart=a.refart
  INNER JOIN grilleTarifs g ON a.codeCate = g.codeCate AND a.codeGam = g.codeGam
  INNER JOIN tarifs t	ON g.codeTarif = t.codeTarif
WHERE f.noFic= 1002;
```
:five: Prix journalier moyen de location par gamme
|Gamme|tarif journalier moyen|
|---|---|
|Entrée de gamme|10.833333|
|Haut de gamme|27.500000|
|Moyenne gamme|15.833333|
|Matériel Professionnel|80.000000|

```sql
USE location_ski;

SELECT g.libelle as Gamme, avg(prixJour) as TarifMoyen
FROM grilletarifs gt 
JOIN gammes g USING (codeGam)
JOIN tarifs t USING (codeTarif)
GROUP BY Gamme;
```
:five: Prix journalier moyen de location par gamme
|refart|designation|nombre de locations|
|---|---|---|
|F05|Fischer Cruiser|4|
|F50|Fischer SOSSkating VASA|3|

:six: Détail de la fiche n°1002 avec le **total**
|noFic|nom|prenom|refart|designation|depart|retour|prixJour|Montant|Total|
|---|---|---|---|---|---|---|---|---|---|
|1002|Desmoulin|Daniel|A03|"Salomon 24X+Z12"|2024-11-18|2024-11-22|10|50|200|
|1002|Desmoulin|Daniel|A04|"Salomon 24X+Z12"|2024-11-19|2024-11-24|10|60|200|
|1002|Desmoulin|Daniel|S03|"Décathlon Apparition"|2024-11-23|NULL|10|90|200|

```sql
USE location_ski;
SELECT  f.noFic, nom, prenom, 
	a.refart, designation, depart, retour, prixJour,
(DATEDIFF(IFNULL(retour, NOW()+1),depart)+1)*prixJour as Montant, Total
FROM 
	fiches f
	JOIN clients c USING (noCli)
	JOIN lignesfic l ON f.noFic = l.noFic
	JOIN articles a ON l.refart=a.refart
	JOIN grilletarifs g ON (a.codeGam=g.codeGam AND a.codeCate=g.codeCate)
	JOIN tarifs t ON g.codeTarif = t.codeTarif
    JOIN (
		SELECT  l.noFic,
		SUM( (DATEDIFF(IFNULL(retour, NOW()+1),depart)+1)*prixJour) as total
        FROM 
			lignesfic l
			JOIN articles a ON l.refart=a.refart
			JOIN grilletarifs g ON (a.codeGam=g.codeGam AND a.codeCate=g.codeCate)
			JOIN tarifs t ON g.codeTarif = t.codeTarif
            WHERE l.noFic=1002
            GROUP BY l.noFic ) info ON info.noFic = f.noFic;
```
:seven:  Grille des tarifs
|libelle|Gamme|Tarif|prixJour|
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

```sql
USE location_ski;
SELECT c.libelle, g.libelle, t.libelle, prixJour
FROM grilleTarifs gt
	JOIN gammes g USING (codeGam)
	JOIN categories c USING (codeCate)
	JOIN tarifs t USING (codeTarif);
```

:eight:  Liste des locations de la catégorie SURF

|refart|designation|nbLocation|
|---|---|---|
|S01|"Décathlon Apparition"|2|
|S02|"Décathlon Apparition"|2|
|S03|"Décathlon Apparition"|2|
```sql
USE location_ski;
SELECT a.refart, designation, count(*) nbLocation
FROM lignesfic l 
JOIN articles a USING (refart)
WHERE a.codeCate='SURF'
GROUP by a.refart;
```
:nine: Calcul du nombre moyen d’articles loués par fiche de location 

|nb_lignes_moyen_par_fiche|
|---|
|2.375000|

```sql
USE location_ski;

SELECT AVG(nblignesParfiche) as nb_lignes_moyen_par_fiche
FROM (
		SELECT count(noLig) nblignesParfiche
		FROM lignesfic l 
		GROUP BY noFic) info;
```

**10** - Calcul du nombre de fiches de location établies pour les catégories de location Ski alpin, Surf et Patinette

|catégorie|nombre de location|
|---|---|
|Ski alpin|2|
|Patinette|1|
|Surf|6|

```sql
USE location_ski;
SELECT c.libelle, count(*)
FROM lignesfic l 
JOIN articles a USING (refart)
JOIN categories c USING(codeCate)
WHERE c.Libelle IN ('Ski Alpin','Surf','Patinette')
GROUP BY c.libelle;
```
**11** - Calcul du montant moyen des fiches de location
|montant moyen d'une fiche de location|
|---|
|131.8750|

```sql
USE location_ski;
SELECT avg(MontantParFiche)
FROM (
	SELECT noFic, SUM((DATEDIFF(IFNULL(retour, NOW()+1),depart)+1)*prixJour) MontantParfiche
	FROM lignesfic l 
	JOIN articles a USING (refart)
	JOIN grilletarifs g ON (a.codeGam=g.codeGam AND a.codeCate=g.codeCate)
	JOIN tarifs t USING (codeTarif)
	GROUP BY noFic) info ;
```

```
Enum "fiches_etat_enum" {
  "EC"
  "RE"
  "SO"
}
Table "articles" {
  "refart" char(8) [pk, not null]
  "designation" varchar(80) [not null]
  "codeGam" char(5) [default: NULL]
  "codeCate" char(5) [default: NULL]

  Indexes {
    refart [unique, name: "refart"]
    codeCate [name: "fk_articles_grilleTarifs_codeCate"]
    codeGam [name: "fk_articles_grilleTarifs_codeGam"]
  }
}
Table "categories" {
  "codeCate" char(5) [pk, not null]
  "libelle" varchar(30) [not null]

  Indexes {
    codeCate [unique, name: "codeCate"]
  }
}
Table "clients" {
  "noCli" int [pk, not null, increment]
  "nom" varchar(30) [not null]
  "prenom" varchar(30) [default: NULL]
  "adresse" varchar(120) [default: NULL]
  "cpo" varchar(5) [not null]
  "ville" varchar(80) [not null]
}
Table "fiches" {
  "noFic" int [pk, not null, increment]
  "noCli" int [not null]
  "dateCrea" date [not null]
  "datePaiement" date [default: NULL]
  "etat" fiches_etat_enum [not null, default: 'EC']

  Indexes {
    noCli [name: "fk_fiches_clients"]
  }
}
Table "gammes" {
  "codeGam" char(5) [pk, not null]
  "libelle" varchar(45) [not null]

  Indexes {
    codeGam [unique, name: "codeGam"]
  }
}
Table "grilleTarifs" {
  "codeGam" char(5) [not null]
  "codeCate" char(5) [not null]
  "codeTarif" char(5) [default: NULL]

  Indexes {
    (codeGam, codeCate) [pk]
    codeCate [name: "fk_grilleTarifs_categories"]
    codeTarif [name: "fk_grilleTarifs_tarifs"]
  }
}
Table "lignesFic" {
  "noLig" int [not null, increment]
  "noFic" int [not null]
  "refart" char(8) [not null]
  "depart" date [not null]
  "retour" date [default: NULL]

  Indexes {
    (noLig, noFic) [pk]
    refart [name: "fk_lignesFic_articles"]
    noFic [name: "fk_lignesFic_fiches"]
  }
}
Table "tarifs" {
  "codeTarif" char(5) [pk, not null]
  "libelle" char(30) [not null]
  "prixJour" decimal(5,0) [not null]

  Indexes {
    codeTarif [unique, name: "codeTarif"]
  }
}
Ref "fk_articles_grilleTarifs_codeCate":"categories"."codeCate" < "articles"."codeCate"
Ref "fk_articles_grilleTarifs_codeGam":"gammes"."codeGam" < "articles"."codeGam"
Ref "fk_fiches_clients":"clients"."noCli" < "fiches"."noCli"
Ref "fk_grilleTarifs_categories":"categories"."codeCate" < "grilleTarifs"."codeCate"
Ref "fk_grilleTarifs_gammes":"gammes"."codeGam" < "grilleTarifs"."codeGam"
Ref "fk_grilleTarifs_tarifs":"tarifs"."codeTarif" < "grilleTarifs"."codeTarif"
Ref "fk_lignesFic_articles":"articles"."refart" < "lignesFic"."refart"
Ref "fk_lignesFic_fiches":"fiches"."noFic" < "lignesFic"."noFic"
```
