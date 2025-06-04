# SysOdpowiedzi 5.1.2 i 5.1.3

### 5.1.2
#### 1. Rodzaje przetwarzania współbieżnego, ze względu na liczbę wykorzystywanych jednostek przetwarzających. 

Program współbieżny - program, który może wykonywać wiele zadań jednocześnie

- **pseudorównoległe** - wymaga rywalizacji zadań gotowych do wykonania o przydział jednostki przetwarzającej(przeplot)
- **równoległe** - jednocześnie przetwarzanych jest wiele zadań przez różne/wiele jednostek przetwarzających



> Przetwarzanie współbieżne zbioru procesów/wątków sekwencyjnych jest ciągiem następujących kolejno stanów i zdarzeń, występujących w ramach wykonania rozkazów z różnych zadań. Dwa wątki sekwencyjne (jednego lub różnych procesów) są współbieżne, jeżeli jeden z nich rozpoczyna się przed zakończeniem drugiego. Program zawierający przynajmniej dwa wątki przetwarzające zadania współbieżnie nazywamy programem współbieżnym. Procesy mogą być przetwarzane współbieżnie, mimo że nie zostały utworzone poprzez uruchomienie jednego programu współbieżnego.

***
2. Kryteria poprawności programu współbieżnego. 

- **Cecha bezpieczeństwa (safety)** - w trakcie przetwarzania mogą wystąpić jedynie
zdarzenia dopuszczalne
- **Cecha żywotności (liveness)** - każde z przetwarzanych zadań musi wykazać postęp w
skończonym czasie

> Zdarzenie, które może pojawić się w określonym stanie przetwarzania sekwencyjnego wątku procesu i nie narusza ustalonych reguł przetwarzania danych (zgodnie z ustaloną specyfikacją funkcjonalną oprogramowania) jest **zdarzeniem dopuszczalnym**

>  Żywotność - po skończonej liczbie zdarzeń przetwarzanie kolejnych instrukcji programu zostanie kontynuowane. Żywotność wymaga w skończonym czasie postępu w realizacji każdego z współbieżnie przetwarzanych zadań. Oczekiwanie zadania z cechą żywotności na spełnienie określonego warunku musi więc gwarantować jego zajście w skończonym czasie. (slajd 16)

> Uszczegółowieniem cechy żywotności jest właściwość uczciwości/sprawiedliwości (fairness), która precyzuje czas oczekiwania zadania na wystąpienie oczekiwanego stanu/spełnienie oczekiwanego warunku/ wystąpienie oczekiwanego zdarzenia (slajd 17)

***
3. Który z warunków poprawności narusza zagłodzenie i uwięzienie oraz jaka jest różnica pomiędzy zagłodzeniem i uwięzieniem? 

Zjawiska naruszają **cechę żywotności**.
Zagłodzenie - przetwarzane zadanie cały czas przebywa w stanie
zablokowanym, przez co nie ma przydzielonej jednostki przetwarzającej, co powoduje brak postępu
Uwięzienie - jednostka przetwarzająca jest przydzielona do zadania, ale nie wykazuje ono żadnego postępu

Jaki jest przykład uwięzienia?
- np. wykonywanie nieskończonej pętli po której są jakieś rozkazy, wtedy zadanie
nigdy nie wyjdzie z pętli i nie będzie przetwarzało rozkazów za pętlą

***
4. Wyjaśnienie pojęć: hazard/wyścig, przeplot, konflikt. Wskazanie unikalnego wyścigu dla zadań ucztujących filozofów.

> Jeżeli wykonywane współbieżnie operacje pomiędzy przetwarzanymi procesami/wątkami (m. in. konflikty) powodują, że w zależności od kolejności występowania zdarzeń system operacyjny uzyska różne stany, to zjawisko to określa się wyścigiem lub hazardem (race condition).

> Przeplot - globalny porządek wykonywania rozkazów różnych zadań

> konflikt w dostępie do danych (w tym także jednostek zasobów) - jeżeli wykonywane instrukcje przynależące do różnych procesów posiadają wspólny operand i przynajmniej jedna z nich jest operacją wyjścia (zapisu)

- Jeden filozof próbuje podnieść widelec, gdy drugi go odkłada
- Dwóch filozofów sięga po tem sam widelec
- Dotarcie do bariery, w zależności który dotrze pierwszy, ten będzie czekał na
innych i
będzie właścicielem blokady


Czy może wystąpić konflikt między wątkami w tym samym procesie?
może
Czy taki konflikt wystąpi pomiędzy zadaniami przetwarzanymi sekwencyjnie?
nie


***
5. Gdzie zapisane jest zadanie filozofa i jaki to jest rodzaj zadania?
*tu odpalamy biblioteke filozofa*

Jak to się dzieje, że mamy jedno zadanie filozofa, wydzielone w funkcji, a tych zadań powoływanych jest wiele i one się od siebie różnią?
- funkcja jest sparametryzowana, czyli parametry wpływają na zadanie, dlatego powoływanych jest wiele zadań, zadania różnią się od siebie parametrami
Czy każdy parametr funkcji jest jednocześnie parametrem zadania filozofa?
- nie, tutaj parametrami zadania filozofa będą: numer filozofa, plik pierwszego widelca, plik drugiego widelca
Jak to się dzieje, że pojedyncze zadanie filozofa jest przetwarzane w osobnym
procesie?
- przez znak & (dokładniej w podpunkcie 7)

***
6. W jaki sposób powoływane są procesy realizujące zadania poszczególnych filozofów i jaką mają charakterystykę?

Przez użycie znaku & (ampersand) zadania są powoływane jako osobne procesy działające w tle

Każdy z powołanych procesów dostanie swoje zadanie filozofa do wykonania, dzięki parametrom te zadania będą rozróżnione


***
7. Ile będzie utworzonych procesów dla ustalonej przez nauczyciela liczby filozofów i który z procesów będzie kończył się jako ostatni? - wymagane wskazanie odpowiedniej instrukcji w skrypcie ucztujących filozofów. 

Ilośc procesów to ilość filozofów +1 (np. 7 filozofów, to 8 procesów), ponieważ ten dodatkowy proces to proces rodzica, który powołał procesy filozofów (potomne). Proces rodzica będzie kończył się jako ostatni, dowodzi tego instrukcja wait, która nakazuje mu oczekiwać na zakończenie procesów potomnych.

Co musi wykonać proces rodzica po instrukcji wait (z czym musi zaczekać)?
- musi zaczekać z instrukcją, która usuwa katalog reprezentujący stół oraz pliki widelców
Dlaczego intrukcja usunięcia katalogu i plików nie może być wykonana przed
wait?
- ponieważ procesy potomne korzystają z tych zasobów i nie można ich im odebrać, powodowałoby to błędy (odwołanie się do pliku/katalogu, który nie istnieje)

***


### Rozdział 5.1.3
1. Jak zauważyć problem zakleszczenia w przygotowanym grafie?

Każdy filozof podniósł jeden widelec. TO tworzy cykl zależności: F1 → WID:2 → F2 → WID:3 → F3 → WID:4 → F4 → WID:5 → F5 → WID:1 → F1. Każdy filozof czeka na zasób (widelec), który jest już zajęty, co uniemożliwia wszystkim kontynuowanie jedzenia.

Filozof to zadanie
Widelce to zasoby wspólne, do których każdy filozof chce uzyskać dostęp wyłączny

***
2. Ilu zadań dotyczy zakleszczenie i jak jego wystąpienie wpływa na poprawność programu?
5 zadań 

Występuje zakleszczenie, stan nieprzerwanego wzajemnego blokowania się
zadań, które nie wykazują postępu. System oczekuje na zajście zdarzenia, które
nigdy nie zajdzie. Występuje ciągłość blokowania wynikające z wykorzystywania
zasobów o dostępie wyłącznym, które nie zostają zwolnione w wyniku
wstrzymania wykonania zadania. Łamana jest zasada żywotności (zadania muszą
wykazać postęp w określonym czasie).

***
3. Warunki konieczne wystąpienia zakleszczenia i który z nich został wyeliminowany poprzez zastosowane rozwiązanie eliminujące zakleszczenia filozofów? 

- Wzajemne wykluczanie - zasób może być w danej chwili wykorzystywany
tylko przez jedno zadanie i nie może być dostępny dla innych zadań (dostęp wyłączny).
- Brak wywłaszczeń - zwolnienie zasobu może być zrealizowane tylko z inicjatywy zadania, które wykorzystuje ten zasób (wywłaszczenie - wymuszone przełączenie kontekstu).
- Przetrzymywanie i oczekiwanie - zadanie posiadające przydzielony zasób może żądać jednostek innego zasobu, które są przydzielone innemu zadaniu.
- Cykl oczekiwania - istnienie zbioru zadań, w którym można wyróżnić cykl, w którym kolejne pary zadań są powiązanie poprzez zależność oczekiwania i przetrzymywania zasobów

Wszystkie muszą być spełnione, aby **mogło wystąpić** zakleszczenie - warunki konieczne, jednak nie wystarczające

Wyeliminowany został cykl oczekiwania poprzez wprowadzenie hierarchii
zasobów - określony został globalny porządek dla wszystkich zasobów o dostępie wyłącznym


Wyeliminowany został cykl oczekiwania poprzez wprowadzenie hierarchii
zasobów (filozof sięga po widelec z mniejszym numerem)

***
4. Jak zastosować hierarchię zasobów i dwufazowe blokowanie. 

hierarchia - wprowadzenie globalnego porządku dla zasobów wspólnych o dostępie wyłącznym np. poprzez numerowanie i ustalenie reguły dostępu do tych zasobów. Np. jeżeli zadanie ma przydzielony n-ty zasób to nie może być mu przydzielony zasób mniejszy od n

blokowanie dwufazowe - protokół, w ramach którego wyróżnia się fazę blokowania niezbędnych zasobów oraz fazę przetwarzania z wykorzystaniem tych zasobów i zwracanie ich do systemu operacyjnego. Zadanie najpierw blokuje wszystkie wspólne zasoby zwrotne i dopiero wtedy następuje przejście do fazy
drugiej. Wszystkie jednostki zasobu muszą być zamawiane w jednym zleceniu

***
5. Czym jest współbieżność konkurencyjna i współbieżność kooperacyjna? - wymagane wskazanie, gdzie występuje każda z tych współbieżności pomiędzy zadaniami filozofów?

Konkurencyjna - wątki i procesy nie współpracują ze sobą, a wzajemne
oddziaływanie pomiędzy nimi sprowadza się do rywalizacji o dostęp do zasobów - filozofowie konkurują o widelec

Kooperacyjna - wątki i procesy zadania współpracują ze sobą, poprzez
komunikację i synchronizację koordynują wykonywanie swoich zadań.
Współbieżność kooperacyjna, komunikacja przy użyciu potoku nieanonimowego -
echo >$5 wysłany pusty wiersz, cat$5 - pusty komunikat