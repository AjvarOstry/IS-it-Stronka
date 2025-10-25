
1. Rodzaje przetwarzania współbieżnego, ze względu na liczbę
wykorzystywanych jednostek przetwarzających. 

- **pseudorównoległe** - wymaga rywalizacji zadań gotowych do wykonania/wykonywanych o przydział jednostki aktywnej przetwarzającej

- **równoległe** - zadania prztwarzane są przez inne jednostki przetwarzajace

2. Kryteria poprawności programu współbieżnego. 

- **Cecha bezpieczeństwa (safety)** - w trakcie przetwarzania mogą wystąpić jedynie
zdarzenia dopuszczalne
- **Cecha żywotności (liveness)** - każde z przetwarzanych zadań musi wykazać postęp w
skończonym czasie

> Zdarzenie, które może pojawić się w określonym stanie przetwarzania sekwencyjnego wątku procesu i nie narusza ustalonych reguł przetwarzania danych (zgodnie z ustaloną specyfikacją funkcjonalną oprogramowania) jest **zdarzeniem dopuszczalnym**

**specyfikacja funkcjonalna** - formalny dokument, który precyzyjnie opisuje, co ma robić i jak ma działać produkt - zbiór danych i reguły przetwarzania (danych wejściowych)

3. Który z warunków poprawności narusza zagłodzenie i uwięzienie oraz jaka jest różnica pomiędzy zagłodzeniem i uwięzieniem? 
Zjawiska naruszają **cechę żywotności**
zagłodzenie - nie ma jednostki - wątek ciągle w stanie zablokowanym
uwięzienie - jest przetwarzany, ale nie wykazuje postepu - na przykład pętla nieskończona

4. Wyjaśnienie pojęć: hazard/wyścig, przeplot, konflikt. Wskazanie unikalnego wyścigu dla zadań ucztujących filozofów.

**hazard/wyścig** - Jeżeli wykonywane współbieżnie operacje pomiędzy przetwarzanymi procesami/wątkami (m. in. konflikty) powodują, że w zależności od kolejności występowania zdarzeń system operacyjny uzyska różne stany
**konflikt** - kiedy conajmniej dwa zadania operują na jednym zasobie i jedno dokonuje jej modyfikacji

    przeplot - globalny porządek wykonywania rozkazów z różnych zadań (inny co uruchamiasz)

    Jeden dotarł do bariery synchronizującej(zaczekaj_na_innych) stał się właścicielem blokady i odbiera komunikaty - może odebrać dwa albo jeden komunikat
        wyślij wyślij odbierz, albo wyślij odbierz wyślij

5. Gdzie zapisane jest zadanie filozofa i jaki to jest rodzaj zadania? *tu odpalamy biblioteke filozofa*

zadanie sparametryzowane
Czy każdy parametr funkcji jest jednocześnie parametrem zadania filozofa?
nie, tutaj parametrami zadania filozofa będą: numer filozofa, plik pierwszego
widelca, plik drugiego widelca

6. W jaki sposób powoływane są procesy realizujące zadania poszczególnych filozofów i jaką mają charakterystykę?

proces rodzica kończy się ostatni
zadanie filozofa przetwarzane jest w osobnym procesie przez ampersant &

Każdy z powołanych procesów dostanie swoje zadanie filozofa do wykonania, dzięki parametrom te zadania będą rozróżnione

7. Ile będzie utworzonych procesów dla ustalonej przez nauczyciela liczby filozofów i który z procesów będzie kończył się jako ostatni? - wymagane wskazanie odpowiedniej instrukcji w skrypcie ucztujących filozofów.

Co musi wykonać proces rodzica po instrukcji wait (z czym musi zaczekać)?
- musi zaczekać z instrukcją, która usuwa katalog reprezentujący stół oraz pliki widelców
Dlaczego intrukcja usunięcia katalogu i plików nie może być wykonana przed
wait?
- ponieważ procesy potomne korzystają z tych zasobów i nie można ich im odebrać, powodowałoby to błędy (odwołanie się do pliku/katalogu, który nie istnieje)

7
5.1.3
1. Jak zauważyć problem zakleszczenia w przygotowanym grafie?

 Każdy filozof podniósł jeden widelec. To tworzy cykl zależności(dochodzi do zakleszczenia): F1 → WID:2 → F2 → WID:3 → F3 → WID:4 → F4 → WID:5 → F5 → WID:1 → F1. Każdy filozof czeka na zasób (widelec), który jest już zajęty, co uniemożliwia wszystkim kontynuowanie jedzenia.

2. Ilu zadań dotyczy zakleszczenie i jak jego wystąpienie wpływa na poprawność programu?

5 zadan
Filozof to zadanie
Widelce to zasoby wspólne, do których każdy filozof chce uzyskać dostęp wyłączny
wystąpienie zakleszczenia dowodzi niepoprawności programu
złamana została zasada żywotności

3. Warunki konieczne wystąpienia zakleszczenia i który z nich został wyeliminowany poprzez zastosowane rozwiązanie eliminujące zakleszczenia filozofów? 

        - nie może być wywłaszczeń - nie można siłowo odebrać widelca
        - jest wzajemne wykluczanie - zasoby o dostępie wyłącznym
        - przetrzymywanie i oczekiwanie - żąda zajętych zasobów
        - cykl oczekiwań - który widać w grafie

4. Jak zastosować hierarchię zasobów i dwufazowe blokowanie.
    cykl wyeliminowany przy zastosowaniu **hierarchii zasobów**
        - zasoby wspólne o dostepie wyłącznym należy ponumerować
        - wprowadzić regułę dostępu do tych zasobów
        jeżeli dla zadania został przydzielony zasób, to nie będziemy mogli przydzielić zasobu o niższym numerze

        filozof najpierw zdobywa dwa widelce, potem dopiero zwalnia jakiekolwiek (faza rosnąca i kurcząca)
        Kolejność zakładania blokad wymusza uporządkowany dostęp.
        Dwufazowość (najpierw zdobycie obu widelców, potem jedzenie, potem oddanie obu) powoduje, że filozofowie nie kolidują w czasie.

5. Czym jest współbieżność konkurencyjna i współbieżność kooperacyjna? - wymagane wskazanie, gdzie występuje każda z tych współbieżności pomiędzy zadaniami filozofów?

        konkurencyjna - dwóch filozofów sięga po jeden widelec
        kooperacyjna - kiedy wykorzystują metody synchronizacji komunikacji na Przykład przez potok

Konkurencyjna - wątki i procesy nie współpracują ze sobą, a wzajemne
oddziaływanie pomiędzy nimi sprowadza się do rywalizacji o dostęp do zasobów - filozofowie konkurują o widelec
Kooperacyjna - wątki i procesy zadania współpracują ze sobą, poprzez
komunikację i synchronizację koordynują wykonywanie swoich zadań.
Współbieżność kooperacyjna, komunikacja przy użyciu potoku nieanonimowego -
echo >$5 wysłany pusty wiersz, cat$5 - pusty komunikat