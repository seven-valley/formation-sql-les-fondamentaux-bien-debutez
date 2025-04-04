Ajoutez des clients et des fiches de location supplémentaires en exécutant le script 
```sql
INSERT Clients VALUES(20, 'Dubosc', 'Frank', '12 avenue des flots bleus', '76140', 'Petit-Quevilly');
INSERT Clients VALUES(21, 'Boon', 'Dany', '22 rue des Ch''tis', '59280', 'Armenti�res');
INSERT Clients VALUES(22, 'Elmaleh', 'Gad', '45 rue du sentier', '75001', 'Paris');
INSERT Clients VALUES(23, 'Dujardin', 'Jean', 'rue Brice', '92500', 'Rueil-Malmaison');
INSERT Clients VALUES(24, 'Marceau', 'Sophie', 'Boulevard de la Boom', '75010', 'Paris');
INSERT Clients VALUES(25, 'Merad', 'Kad', 'Rue du petit nicolas', '91130', 'Ris-Orangis');
INSERT Clients VALUES(26, 'Seigner', 'Mathilde', '357 rue du Camping', '75012', 'Paris');
INSERT Clients VALUES(27, 'Reno', 'Jean', '78 Boulevard de L�on', '51200', 'Montmirail');
INSERT Clients VALUES(28, 'Lanvin', 'G�rard', '84 avenue de l''aile ou la cuisse', '92100', 'Boulogne-Billancourt');
INSERT Clients VALUES(29, 'Tautou', 'Audrey', 'rue de Montmartre', '63110', 'Beaumont');
INSERT Clients VALUES(30, 'Cotillard', 'Marion', '45 rue de la M�me', '13001', 'Marseille');
INSERT Clients VALUES(31, 'Duris', 'Romain', '76 rue de l''arnaqueur', '06000', 'Nice');
INSERT Clients VALUES(32, 'Depardieu', 'G�rard', '57 rue du conte de Mont�-Cristo', '36000', 'Ch�teauroux');
INSERT Clients VALUES(33, 'Youn', 'Micha�l', 'rue de la beuze', '92150', 'Suresnes');
INSERT Clients VALUES(34, 'Poelvoorde', 'Beno�t', '22 rue du Boulet', '22500', 'Paimpol');
INSERT Clients VALUES(35, 'Paradis', 'Vanessa', '12 rue des arnaqueurs', '94100', 'Saint-Maur-des-Foss�s');
INSERT Clients VALUES(36, 'Wilson', 'Lambert', '100 rue de Dieu', '92200', 'Neuilly-sur-Seine');
INSERT Clients VALUES(37, 'Garcia', 'Jos�', '65 rue de la v�rit�', '75001', 'Paris');
INSERT Clients VALUES(38, 'Luchini', 'Fabrice', '73 rue de Beaumarchais', '75016', 'Paris');
INSERT Clients VALUES(39, 'Baye', 'Nathalie', '33 rue de V�nus', '27150', 'Mainneville');
INSERT Clients VALUES(40, 'Magimel', 'Beno�t', '47 rue des petits mouchoirs', '33950', 'L�ge-Cap-Ferret');
INSERT Clients VALUES(41, 'Cluzet', 'Fran�ois', '7 rue des apprentis', '75018', 'Paris');
INSERT Clients VALUES(42, 'Frot', 'Catherine', 'rue Odette', '69110', 'Sainte Foy-l�s-Lyon');
INSERT Clients VALUES(43, 'Dupontel', 'Albert', '11 impasse de Bernie', '78100', 'Saintermain-en-Laye');
INSERT Clients VALUES(44, 'Huppert', 'Isabelle', '8 rue des femmes', '75002', 'Paris');
INSERT Clients VALUES(45, 'Deneuve', 'Catherine', '12 rue de Rochefort', '50100', 'Cherbourg-Octeville');
INSERT Clients VALUES(46, 'de France', 'C�cile', '17 rue de l''auberge espagnole', '08000', 'Charlesville-M�zi�res');
```
:warning: Attention le script est en SQL server de microsoft  
Comment le convertir en MySQL ?  
ChatGPT ?   
    
Ensuite, écrivez les requêtes SQL répondant aux demandes ci-dessous :
   
:one: Liste des clients (nom, prénom, adresse, code postal, ville) ayant au moins une fiche de location en cours. 

:two: Détail de la fiche de location de M. Dupond Jean de Paris (avec la désignation des articles loués, la date 
de départ et de retour). 

:three: Liste de tous les articles (référence, désignation et libellé de la catégorie) dont le libellé de la catégorie 
contient ski.  
:four: Calcul du montant de chaque fiche soldée et du montant total des fiches.   
:five: Calcul du nombre d’articles actuellement en cours de location.  

:six:
 Calcul du nombre d’articles loués, par client.   
:seven: Liste des clients qui ont effectué (ou sont en train d’effectuer) plus de 200€ de location.  
:eight: Liste de tous les articles (loués au moins une fois) et le nombre de fois où ils ont été loués, triés du plus 
loué au moins loué.   

:nine: Liste des fiches (n°, nom, prénom) de moins de 150€. 

**10** : Calcul de la moyenne des recettes de location de surf (combien peut-on espérer gagner pour une location 
d'un surf ?).  

**11** : Calcul de la durée moyenne d'une location d'une paire de skis (en journées entières).