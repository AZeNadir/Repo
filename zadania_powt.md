# Lab 3 część 1
## Zadanie 1
SELECT imie, nazwisko, data_urodzenie FROM pracownik;

## Zadanie 2
SELECT imie, nazwisko, DATEDIFF(year, GETDATE(), data_urodzenia);

## Zadanie 3
SELECT nazwa, COUNT(id_pracownika) FROM dzial INNER JOIN pracownik ON dzial.id_dzialu=pracownik.dzial GROUP BY id_dzialu;

## Zadanie 4
SELECT nazwa_kategorii, COUNT(id_towaru) FROM kategoria INNER JOIN towar ON kategoria.id_kategorii=towar.kategoria GROUP BY id_kategorii;

## Zadanie 5
SELECT nazwa_kategorii, GROUP_CONCAT(nazwa_towaru) FROM kategoria INNER JOIN towar ON kategoria.id_kategorii=towar.kategoria;

## Zadanie 6
SELECT ROUND(AVG(pensja), 2) FROM pracownik;

## Zadanie 7
SELECT ROUND(AVG(pensja), 2) FROM pracownik WHERE data_zatrudnienia <= '2019-01,11';

## Zadanie 8
SELECT nazwa_towaru, towar, SUM(ilosc) AS suma FROM towar INNER JOIN pozycja_zamowienia ON towar.id_towaru=pozycja_zamowienia.towar GROUP BY towar ORDER BY suma DESC LIMIT 10; 

## Zadanie 9
SELECT id_zamowienia, numer_zamowienia, SUM(ilosc*cena) FROM zamowienie INNER JOIN pozycja_zamowienia ON zamowienie.id_zamowienia=pozycja_zamowienia.zamowienie WHERE QUARTER(zamowienie.data_zamowienia) = 1 AND YEAR(zamowienie.data_zamowienia) = 2017 GROUP BY zamowienie.id_zamowienia;

## Zadanie 10
SELECT imie, nazwisko, SUM(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) AS suma FROM zamowienie INNER JOIN pozycja_zamowienia ON zamowienie.id_zamowienia=pozycja_zamowienia.zamowienie INNER JOIN pracownik ON pracownik.id_pracownika=zamowienia.pracownik_id_pracownika GROUP BY zamowienie.pracownik_id_pracownika ORDER BY suma DESC;

# Lab 3 część 2
## Zadanie 1
SELECT p.dzial, MIN(p.pensja), MAX(p.pensja), AVG(p.pensja) FROM pracownik p INNER JOIN dzial d ON p.dzial=d.id_dzialu GROUP BY d.id_dzialu;

## Zadanie 2
SELECT z.numer_zamowienia, SUM(pz.ilosc*pz.cena) AS wartosc, z.klient, k.pelna_nazwa FROM zamowienie z INNER JOIN pozycja_zamowienia pz ON z.id_zamowienia=pz.zamowienie INNER JOIN klient k ON k.id_klienta=z.klient GROUP BY z.id_zamowienia ORDER BY wartosc DESC LIMIT 10;

## Zadanie 2*
SELECT DISTINCT k.id_klienta, z.klient FROM klient k INNER JOIN zamowienie z ON k.id_klienta=z.klient;
