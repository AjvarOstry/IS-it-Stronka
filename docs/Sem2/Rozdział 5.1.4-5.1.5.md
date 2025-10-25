## Rozdział 5.1.4

1. Należy wskazać w kodzie ucztujących filozofów instrukcję stanowiącą sekcję wejściową i sekcję wyjściową regionu krytycznego. W jaki sposób instrukcje te odwołują się do pliku widelca i która instrukcja w kodzie zapewnia otwarcie pliku widelca?

**sekcja/region krytyczny** - fragment programu, w którym proces/wątek korzysta ze współdzielonych danych lub zasobów wyłącznych
Sekcja powinna być jak najkrótsza tzn. zawierać jak najmniej operacji niezwiązanych ze wspólnymi zasobami wielu zadań. Program może zawierać wiele sekcji krytycznych, z których każda powinna kontrolować dostęp do innych wspólnych zasobów zadań. Może w niej występować wywłaszczenie.
Nie blokujemy wywłaszczenia, bo gdyby była tylko jedna jednostka i nie byłoby wywłaszczania to wątki nie kontynuowałyby przetwarzania. Nie byłoby postępu
sekcja wejściowa(podnies_widelec)   sekcja wyjsciowa(odloz_widelec)

3 parametr pozycyjny to deskryptor pliku widelca określany za pomocą exec:
```bash
eval "exec ${DESKRYPTOR_WIDELEC_1}>${6}" #Polecenie exec otwiera plik widelca i przypisuje do niego deskryptor widelca                       
eval "exec ${DESKRYPTOR_WIDELEC_2}>${7}"
```

2. Warunki poprawności regionu krytycznego. 
- wykonywana jest w danej chwili przez co najwyżej jedno zadanie,
- Nie można przyjmować żadnych założeń dotyczących szybkości przetwarzania sekcji krytycznej (w tym: priorytetów, poziomów kolejek, polityk szeregowania, polityk NUMA, reguł koligacji), przełączeń kontekstu zadań, lub ich liczby
- Zadanie działające wewnątrz sekcji krytycznej w skończonym czasie musi opuścić sekcję krytyczną
- Żadne zadanie nie powinno nieskończenie długo oczekiwać na dostęp do sekcji krytycznej

Wersja z zajęć:
W sekcji krytycznej przetwarzane są instrukcje - rozkazy

- w danym rejonie może znajdować się jedno zadanie
- zadanie nie może oczekiwać w nieskończoność na wejście
- musi w skończonym czasie opuścić sekcję krytyczną
- nie mogą występować założenia dot. szybkości przetwarzania sekcji krytycznej

3. Przy określonej przez nauczyciela liczbie filozofów , posiłków i jednostkach przetwarzających ile występuje sekcji krytycznych związanych z pobieraniem widelców ze stołu przez filozofów w opracowanym rozwiązaniu?
`uruchom.sh -f <l. filoz> -n <l. posilkow> -k <czas konsumpcji> -r <czas rozmyślania>`
Tyle ile `-f`, ALE jest ona powiązana z zasobem (widelcem), nie filozofem (reszta parametrów to zmyłka)

4. Z ilu sekcji krytycznych korzysta pojedynczy filozof i jaka jest zależność pomiędzy nimi (przenikanie/zagnieżdżenie/rozłączność)?
Z dwóch **zagnieżdżonych**, zewnętrznej i wewnętrznej.
jeden filozof korzysta z dwóch sekcji krytycznych

```bash
    podnies_widelec $1 $6 $DESKRYPTOR_WIDELEC_1 # zagnieżdżenie, bo jedna jest w drugiej
	podnies_widelec $1 $7 $DESKRYPTOR_WIDELEC_2 

	LICZBA_ZJEDZONYCH_POSILKOW=$(($LICZBA_ZJEDZONYCH_POSILKOW+1))
	jedzenie $1 $LICZBA_ZJEDZONYCH_POSILKOW $8

	odloz_widelec $1 $7 $DESKRYPTOR_WIDELEC_2 # żeby było przenikanie można np
	odloz_widelec $1 $6 $DESKRYPTOR_WIDELEC_1 #zamienić te dwie linijki
```

5. Jaki mechanizm synchronizacji został zastosowany do zbudowania sekcji krytycznej i z jakich elementów jest złożony oraz ile udostępnia operacji?
Mechanizm **blokady wyłącznej**.
Jest złożony z elementów:
właściciel
kolejka (algorytm FIFO) - gdyby był LIFO proces mógłby zostać zagłodzony

Udostępnia 3 operacje:
unlock odblokowanie - tylko wtedy, kiedy zadanie zajmuje blokadę i inne czeka w kolejce do blokady
zwolnić blokadę może tylko aktualny właściciel
lock zablokowanie - jak blokada nie ma właściciela to blokada, jak ma to do kolejki
trylock próba zablokowania - zjak lock, ale nie idzie do kolejki, tylko zwraca kod błędu



```bash
# tty 1
mkfifo potok
echo komunikat > potok

#tty 2
cat potok

file ?
```
## Rozdział 5.1.5

1. Czy bez zastosowania zatrzasku wszystkie zadania filozofów będą zawsze przetwarzane współbieżnie w każdym uruchomieniu skryptu i czy program zawierający rozwiązanie ucztujących filozofów będzie wówczas nadal poprawny? 
Nie zawsze będzie współbieżne przetwarzanie zadań filozofów - może wystąpić przetwarzanie sekwencyjne Jak usuniemy zatrzask, dalej będzie poprawny program
Tylko zapewnia wymóg współbieżności

2. Czym jest zatrzask? - Barierą jednokrotnego zastosowania, umożliwia jednokrotną synchronizację
zadań.

3. Z jakich elementów złożony jest zatrzask i ile udostępnia operacji?
udostępnia jedną operację - osiągnięcie bariery - dla ostatniego zadania wszystkie są wypuszczane
Składa się z: Licznika i Kolejki zadań oczekujących - nie musi być zarządzana FIFO, bo to nie wypływa na zasadę zachowania żywotności

4. Jakie mechanizmy synchronizacji i komunikacji zostały zastosowane przy budowie zatrzasku?
Synchronizacja - blokada wyłączna
Komunikacja - potok nazwany (mechanizm komunikacji pomiędzy procesami, który umożliwia przesyłanie danych w sposób jednokierunkowy). bo ma swoją implementację plikową
ważna jest tylko luczba komunikatów

5. Czym jest spotkanie i kto uczestniczy w pojedynczym spotkaniu, na czym polega komunikacja synchroniczna w spotkaniu?
Mechanizm spotkań - służy do synchronizacji zadań. Umożliwia procesom/wątkom synchronizację swoich działań w precyzyjnym momencie. Odbiorca może być tylko jeden, nadawców może być wielu. Mechanizm spotkań jest realizowany przez potok nazwany (nieanonimowy).
Nadawca także czeka na odbiorcę z odebraniem komunikatu - mamy zatem do czynienia ze spotkaniem

potok - synchroniczne, jednokierunkowe
nie może być buforowania

6. Dla ustalonej przez nauczyciela liczby filozofów ile będzie wszystkich spotkań? - spotkań będzie o 1 mniej niż filozofów

7. Który filozof uczestniczy w każdym ze spotkań i jaką pełni w nich rolę (nadawcy/odbiorcy)?
Będzie to ten który pierwszy osiągnął barierę - rolę odbiorcy
Ten, który jako pierwszy wykona funkcję zaczekaj_na_innych().

8. Dla ustalonej przez nauczyciela liczby filozofów ile minimalnie a ile maksymalnie komunikatów może odczytać jednorazowo odbiorca z pliku potoku?
Najwięcej tyle ile spotkań, min. 1