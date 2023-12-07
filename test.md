# Zadania lab 04
## Zadanie 1

```sql
CREATE TABLE postac (
    -> id_postaci INTEGER AUTO_INCREMENT PRIMARY KEY,
    -> nazwa VARCHAR(40),
    -> rodzaj ENUM('wiking','ptak','kobieta'),
    -> data_ur DATE,
    -> wiek INTEGER UNSIGNED);

INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Bjorn', 'wiking','1700-11-09', 43);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Drozd', 'ptak','1740-06-25', 3);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Tesciowa', 'kobieta','1655-01-05', 85);

UPDATE postac SET wiek=88 WHERE nazwa='Tesciowa';
```
## Zadanie 2

```sql
CREATE TABLE walizka (
    -> id_walizki INTEGER AUTO_INCREMENT PRIMARY KEY,
    -> pojemnosc INTEGER UNSIGNED,
    -> kolor ENUM('rozowy','czerwony','teczowy','zolty'),
    -> id_wlasciciela INTEGER,
    -> FOREIGN KEY(id_wlasciciela) REFERENCES postac(id_postaci) ON DELETE CASCADE);

ALTER TABLE walizka ALTER kolor SET DEFAULT 'rozowy';

INSERT INTO walizka (pojemnosc, kolor, id_wlasciciela) VALUES (22, 'zolty',1);
INSERT INTO walizka (pojemnosc, kolor, id_wlasciciela) VALUES (45, 'rozowy',3);
```
## Zadanie 3
```sql
CREATE TABLE izba (
    -> adres_budynku VARCHAR(50) NOT NULL,
    -> nazwa_izby VARCHAR(50) NOT NULL,
    -> metraz MEDIUMINT UNSIGNED,
    -> PRIMARY KEY(adres_budynku, nazwa_izby),
    -> FOREIGN KEY (wlasciciel) REFERENCES postac(id_postaci) SET NULL ON DELETE);

ALTER TABLE izba ADD COLUMN
    -> kolor VARCHAR(30) default 'czarny'
    -> AFTER metraz;

INSERT INTO izba VALUES ('Kolejowa 23', 'spiżarnia', 10, default, 1);
```

## Zadanie 4
```sql
CREATE TABLE przetwory (
    -> id_przetworu INT AUTO_INCREMENT PRIMARY KEY,
    -> rok_produkcji SMALLINT default 1654,
    -> id_wykonawcy INT FOREIGN KEY REFERENCES postac(id_postaci),
    -> zawartość VARCHAR(40),
    -> dodatek VARCHAR(30) default 'papryczka chilli',
    -> FOREIGN KEY (id_konsumenta) REFERENCES postac(id_postaci);

INSERT INTO przetwory (rok_produkcji, id_wykonawcy, zawartość, dodatek, id_konsumenta) VALUES (1644, 1, 'bigos', default, 3);
```

## Zadanie 5
```sql
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Wiking1', 'wiking','1705-07-23', 38);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Wiking2', 'wiking','1702-03-14', 41);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Wiking3', 'wiking','1695-04-05', 48);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Wiking4', 'wiking','1705-11-18', 38);
INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Wiking5', 'wiking','1704-05-28', 39);

CREATE TABLE statek (
    -> nazwa_statku VARCHAR(30) PRIMARY KEY,
    -> rodzaj_statku ENUM('wioslowy','zaglowy'),
    -> data_wodowania DATE,
    -> max_ladownosc INT UNSIGNED);

INSERT INTO statek VALUES ('Statek1', 'zaglowy', '1705-08-21', 10000);
INSERT INTO statek VALUES ('Statek2', 'wioslowy', '1704-10-17', 15000);

ALTER TABLE postac ADD COLUMN
    -> funkcja VARCHAR(25)
    -> AFTER wiek;

UPDATE postac SET funkcja='kapitan' WHERE id_postaci=1;

ALTER TABLE postac ADD COLUMN
    -> FOREIGN KEY (statek) REFERENCES statek(nazwa_statku);

UPDATE postac SET statek='Statek1' WHERE nazwa='Bjorn';
UPDATE postac SET statek='Statek1' WHERE nazwa='Wiking1';
UPDATE postac SET statek='Statek1' WHERE nazwa='Wiking2';
UPDATE postac SET statek='Statek1' WHERE nazwa='Wiking3';
UPDATE postac SET statek='Statek2' WHERE nazwa='Wiking4';
UPDATE postac SET statek='Statek2' WHERE nazwa='Wiking5';
UPDATE postac SET statek='Statek2' WHERE nazwa='Drozd';

DELETE FROM izba WHERE nazwa_izby='spiżarnia';

DROP TABLE izba;
```
# Zadania lab 05

## Zadanie 2
```sql
ALTER TABLE ADD COLUMN pesel CHAR(11) FIRST;

UPDATE postac SET pesel='12345678901' + id_postaci;

ALTER TABLE postac ADD PRIMARY KEY(pesel);

ALTER TABLE postac MODIFY rodzaj ENUM('wiking','ptak','kobieta','syrena');

INSERT INTO postac (nazwa, rodzaj, data_ur, wiek) VALUES ('Gertruda Nieszczera', 'Syrena','1723-11-09', 20);
```

## Zadanie 3
```sql
UPDATE postac SET statek='Statek1' WHERE nazwa LIKE '%a%';

UPDATE statek SET max_ladownosc=max_ladownosc * 0.7 WHERE year(data_wodowania) BETWEEN 1901 AND 2000;

ALTER TABLE postac ADD CHECK(wiek < 1000);
```
## Zadanie 4
```sql
INSERT INTO postac (nazwa, data_ur, wiek) VALUES ('Loko', '','1723-05-18', 20);

CREATE TABLE marynarz LIKE postac;

INSERT INTO marynarz SELECT * FROM postac WHERE statek IS NOT NULL;


```
## Zadanie 5
```sql
ALTER TABLE marynarz ADD PRIMARY KEY(pesel);
ALTER TABLE marynarz ADD PRIMARY KEY(id_postaci);
```

# Zadania lab 06
## Zadanie 1
```sql
CREATE TABLE kreatura AS SELECT * FROM wikingowie.kreatura;
SELECT * FROM zasob;
SELECT * FROM zasob WHERE typ='jedzenie';
SELECT idZasobu, ilosc FROM zasob WHERE idKreatury IN (1, 3, 5);
```

## Zadanie 2
```sql
SELECT * FROM kreatura WHERE NOT rodzaj='wiedźma' AND udzwig >= 50;
SELECT * FROM zasob WHERE waga BETWEEN 2 AND 5;
SELECT * FROM kreatura WHERE nazwa LIKE '%or%' AND udzwig BETWEEN 30 AND 70;
```

## Zadanie 3
```sql
SELECT * FROM zasob WHERE MONTH(dataPozyskania) BETWEEN 7 AND 8;
SELECT * FROM zasob WHERE rodzaj IS NOT NULL ORDER BY waga ASC;
SELECT * FROM kreatura ORDER BY dataUr ASC LIMIT 5;
```

## Zadanie 4
```sql
SELECT DISTINCT rodzaj FROM zasob;
SELECT CONCAT_WS('-',nazwa,rodzaj) AS nazwaRodzaj WHERE rodzaj LIKE 'wi%';
SELECT ilosc*waga AS wagaCalkowita FROM zasob WHERE YEAR(dataPozyskania) BETWEEN 2000 AND 2007;
```

## Zadanie 5
```sql
SELECT () FROM zasob
SELECT * FROM zasob WHERE rodzaj IS NULL;
SELECT DISTINCT FROM zasob WHERE nazwa LIKE 'Ba%' OR nazwa LIKE '%os' ORDER BY nazwa ASC; 
```
# Zadania lab 07
## Zadanie 1
```sql
SELECT AVG(waga) FROM kreatura WHERE rodzaj='wiking';
SELECT AVG(waga), COUNT(waga) FROM kreatura GROUP BY rodzaj;
SELECT AVG(2023 - YEAR(dataUr) AS wiek FROM kreatura GROUP BY rodzaj;
```
## Zadanie 2
```sql
SELECT SUM(waga * ilosc) FROM zasob GROUP BY rodzaj;
SELECT AVG(waga) FROM zasob WHERE ilosc >= 4 GROUP BY nazwa HAVING SUM(waga) > 10;
SELECT COUNT(DISTINCT nazwa) FROM zasob GROUP BY rodzaj HAVING MIN(ilosc) > 1;
```
## Zadanie 3
```sql
SELECT * FROM kreatura k INNER JOIN ekwipunek e ON k.idkreatury=e.idKreatury;
SELECT zasob.nazwa FROM kreatura k INNER JOIN ekwipunek e ON k.idkreatury=e.idKreatury INNER JOIN zasob z ON z.idZasobu=e.idZasobu;
SELECT * FROM kreatura k LEFT JOIN ekwipunek e ON k.idKreatury=e.idKreatury WHERE e.idKreatury IS NULL;
```
## Zadanie 4
```sql
SELECT nazwa.kreatura, nazwa.zasob FROM kreatura NATURAL JOIN zasob WHERE YEAR(dataUr) > 1670;
SELECT zasob.nazwa FROM kreatura k INNER JOIN ekwipunek e ON k.idkreatury=e.idKreatury INNER JOIN zasob z ON z.idZasobu=e.idZasobu WHERE z.rodzaj =
jedzenie ORDER BY k.dataUr DESC LIMIT 5;
SELECT CONCAT(k1.nazwa,' - ',k2.nazwa) FROM kreatura k1 INNER JOIN kreatura k2 WHERE k1.idKreatury - k2.idKreatury = 5;
```
## Zadanie 5
```sql
SELECT k.rodzaj, AVG(e.ilosc * z.waga) FROM kreatura k INNER JOIN ekwipunek e ON k.idKreatury = e.idKreatury INNER JOIN zasob z ON e.idZasobu = z.idZasobu WHERE k.rodzaj NOT IN ('malpa', 'waz') GROUP BY k.rodzaj HAVING SUM(e.ilosc) < 30;
```
