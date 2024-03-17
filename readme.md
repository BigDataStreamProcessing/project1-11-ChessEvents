# Charakterystyka danych
Gracze rozgrywają mecze szachowe na wielu platformach internetowych. 
Organizacja FIDE (*International Chess Federation*) zbiera dane o wszystkich rozgrywanych partiach online. 

W strumieniu pojawiają się zdarzenia zgodne ze schematem `ChessEvent`.

```
create json schema ChessEvent(winner string, loser string, winnersTitle string, losersTitle string, winnersColor string, opening string, matchTimeInSeconds int, numberOfMoves int, ets string, its string);
```

Każde zdarzenie związane z jest z faktem zakończenia partii przez dwóch graczy o konkretnych tytułach szachowych.

Dane uzupełnione są o dwie etykiety czasowe. 
* Pierwsza (`ets`) związana jest z momentem zakończenia partii. 
  Etykieta ta może się losowo spóźniać w stosunku do czasu systemowego maksymalnie do 30 sekund.
* Druga (`its`) związana jest z momentem rejestracji zakończenia partii w systemie.

# Opis atrybutów

Atrybuty w każdym zdarzeniu zgodnym ze schematem `ChessEvent` mają następujące znaczenie:

* `winner` - imię i nazwisko zwycięzcy
* `loser` - imię i nazwisko przegranego
* `winnersTitle` - tytuł szachowy zwycięzcy
* `losersTitle` - tytuł szachowy przegranego
* `winnersColor` - kolor pionków, którym grał zwycięzca (czarne lub białe pionki)
* `opening` - otwarcie, którym rozpoczęła się partia
* `matchTimeInSeconds` - czas trwania partii w sekundach
* `numberOfMoves` - liczba ruchów w partii
* `ets` - czas zakończenia partii
* `its` - czas rejestracji faktu zakończenia partii w systemie

# Zadania
Opracuj rozwiązania poniższych zadań. 
* Opieraj się strumieniu zdarzeń zgodnych ze schematem `ChessEvent`
* W każdym rozwiązaniu możesz skorzystać z jednego lub kilku poleceń EPL.
* Ostatnie polecenie będące ostatecznym rozwiązaniem zadania musi 
  * być poleceniem `select` 
  * posiadającym etykietę `answer`, przykładowo:
  
```aidl
@name('answer') SELECT winner, loser, winnersTitle,
                       losersTitle, winnersColor, opening, 
                       matchTimeInSeconds, numberOfMoves, ets, its
FROM ChessEvent#ext_timed(java.sql.Timestamp.valueOf(its).getTime(), 3 sec);
```

## Zadanie 1
Utrzymuj informację o średniej ilości ruchów potrzebnej do wygrania partii przez graczy z poszczególnymi tytułami zarejestrowanymi w ciągu ostatniej minuty.

Wyniki powinny zawierać następujące kolumny:
- `winnersTitle` - nazwę tytułu zwycięzcy,
- `avgMoves` - średnią liczbę ruchów zarejestrowanych w ciągu ostatniej minuty.

## Zadanie 2
Wykrywaj przypadki zakończenia partii w mniej niż 15 ruchów lub więcej niż 95 ruchów.

Wyniki powinny zawierać wszystkie kolumny dotyczące zdarzenia zakończenia partii.

## Zadanie 3
Wykrywaj przypadki, partii zakończonych w liczbie ruchów co najmniej dwukrotnie mniejszej niż średnia liczba ruchów dla tego samego otwarcia z ostatnich 1000 partii.

Wyniki powinny zawierać następujące kolumny:
- `opening` - nazwa otwarcia,
- `numberOfMoves` - liczba ruchów w wykrytej partii,
- `avgMoves` - średnia liczba ruchów.

## Zadanie 4
Utrzymywane są dwa rankingi 10 najbardziej popularnych zwycięskich otwarć (najczęściej wykorzystywanych) dla: (1) tytułu GM, (2) tytułu WGM, z partii zarejestrowanych w ciągu ostatniej minuty.

Utrzymuj zestawienie porównujące liczby otwarć partii dla obu tytułów 

Wyniki powinny zawierać następujące kolumny:
- `opening` - nazwa otwarcia,
- `gamesGM` - liczba gier o danym typie otwarcia dla tytułu GM,
- `gamesWGM` - liczba gier o danym typie otwarcia dla tytułu WGM.

## Zadanie 5
Znajduj serie trzech partii, które odbyły się w ciągu maksymalnie 5 sekund, zakończonych partią, której zwycięskim tytułem jest WGM oraz wygrana nastąpiła w ponad 70 ruchach. 

Wyniki powinny zawierać następujące kolumny:
- `winner1` - imię i nazwisko zwycięzcy pierwszej partii z serii,
- `winner2` - imię i nazwisko zwycięzcy drugiej partii z serii,
- `winner3` - imię i nazwisko zwycięzcy trzeciej partii z serii.

## Zadanie 6
Wykrywaj trzy kolejne (niekonieczne bezpośrednio po sobie) partie, w których wygrały białe w mniej niż 50 ruchach. Wyklucz przypadki, w trakcie których pomiędzy pierwszą a trzecią partią odbyła się partia wygrana przez czarne. 

Zadbaj o to, aby wykryte serie nie nakładały się na siebie.

Wyniki powinny zawierać następujące kolumny:

- `numberOfMoves1` - liczba ruchów pierwszej z partii,
- `numberOfMoves2` - liczba ruchów drugiej z partii,
- `numberOfMoves3` - liczba ruchów trzeciej z partii.

## Zadanie 7
Znajduj sekwencje partii "normalnych" zakończonych partią, która do "normalnych" nie należy. Partia "normalna" to partia, w której liczba ruchów zawiera się w przedziale (20 - 90) i czas w przedziale (60 - 540).

Wyniki powinny zawierać następujące kolumny:

- `firstWinner` - imię i nazwisko pierwszego zwycięzcy partii "normalnych"
- `firstTitle` - tytuł pierwszego zwycięzcy partii "normalnych",
- `lastWinner` - imię i nazwisko ostatniego zwycięzcy partii "normalnych",
- `lastTitle` - tytuł ostatniego zwycięzcy partii "normalnych",
- `numberOfGames` - liczba gier w sekwencji partii "normalnych".
