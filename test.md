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
