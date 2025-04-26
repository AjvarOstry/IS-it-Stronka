# Sysodpowiedzi 4

### 4.1
#### 1. Jakie są podstawowe stany wątku procesu? 
(testowe)

Podstawowe są trzy:
- Zablokowany
- Gotowy do wykonania
- Wykonywany

*W systemie operacyjnym GNU/Linux listę procesów udostępnia narzędzie ps, gdzie stan procesu jest reprezentowany odpowiednią wielką literą w kolumnie stat: gotowy do wykonania i wykonywany mają identyczne oznaczenie (R), zablokowany w zależności od rodzaju uśpienia z reakcją na sygnały (S) lub bez reakcji (D), wstrzymany (T), proces uszkodzony (Z). Dodatkowo wątki jądra mogą wykazywać stan bezczynności (I).*

***

#### 2. Jakie warunki muszą być spełnione aby wątek procesu mógł uzyskać przydział jednostki przetwarzającej?
(testowe)

Przydzielana jednostka musi być aktywna, a wątek musi być w stanie gotowym do wykonania

***

#### 3. Jaka jest charakterystyka jednowątkowego procesu ograniczonego wejściem-wyjściem a jaka procesu ograniczonego procesorem? 
(testowe)

Proces ograniczony wejściem-wyjściem będzie często zwalniał przydzieloną mu jednostkę
przetwarzającą, bo **ponad połowę** swojego czasu życia spędza w stanie zablokowanym. Wątek ograniczony procesorem będzie za to spędzał więksża część czasu w dowolnym z dwóch pozostałych stanów - gotowym do wykonania lub wykonywanym.

***

#### 4. Do czego służy klasyfikacja procesów na ograniczone procesorem i ograniczone wejściem wyjściem oraz na jakiej podstawie zweryfikowano, że proces jest ograniczony wejściem-wyjściem? - weryfikacja przygotowanych w listingach obliczeń i wskazanie w nich wirtualnego czasu życia procesu realizującego odczyty z dysku /dev/sda.

***

#### 5. Co reprezentuje wartość %cpu dla procesu jednowątkowego? - dla jakiego procesu wartość %cpu może przekroczyć 100%? - wymagane zaprezentowanie %cpu dla procesu jednowątkowego w listingu.

***

#### 6. Co reprezentuje wartość %mem dla procesu? - wymagane wykazanie w listingach wartości, na bazie których %mem jest wyliczany, czy wartość %mem może przekroczyć 100%? - wymagane zaprezentowanie %mem dla procesu jednowątkowego w listingu.

***

#### 7. Z jakich obszarów składa się pamięć wirtualna procesu i czy one ze sobą bezpośrednio sąsiadują zgodnie z adresacją logiczną?
(testowe)

Pamięć wirtualna procesu składa się z segmentów pamięci, które z kolei podzielone są na strony pamięci. Nie muszą sąsiadować ze sobą, bezpośrednio, jednak muszą występować w określonej kolejności. Każdy z obszarów pamięci ma przypisane uprawnienia zależne od segmentu procesu, do jakiego należy *(co było już wykazywane w odpowiedzi z zadania 2 - polecenie `pmap`)*

***

#### 8. Czym jest sterta procesu i co zawiera?
(testowe)

Sterta pamięci to dynamicznie przydzielany dla procesu obszar przeznaczony na dane wytworzone przez proces. Zwykle zawiera dane dynamicznie alokowane (`new` w c++), tablice itp.

***

#### 9. Który z segmentów procesu zawiera stertę, a który stos i czy są one segmentami anonimowymi? - wymagane wskazanie segmentu sterty i stosu w mapie pamięci wybranego procesu.

#### 10. Czy całość jego kodu i danych jest wczytywana z pliku programu ELF oraz plików bibliotek ELF do fizycznej pamięci ulotnej?  - wymagane zaprezentowania odpowiednich wartości dla przykładowego nieanonimowego segmentu danych i segmentów tekstu w listingu z mapą pamięci procesu jednowątkowego, gdzie nie wszystkie strony segmentu są załadowane do ramek.

#### 11. Czy proces będzie posiadał więcej ramek czy stron pamięci i co mogą one zawierać?
(testowe)

Pamięć wirtualna, [o której była mowa w zadaniu 7](#7-z-jakich-obszarów-składa-się-pamięć-wirtualna-procesu-i-czy-one-ze-sobą-bezpośrednio-sąsiadują-zgodnie-z-adresacją-logiczną) , dzieli się na strony.natomiast pamięć fizyczna (RAM) dzieli się na ramki, do których ładowane są strony. W każdej ramce mieści się jedna strona. Należy pamiętać, że pamięć wirtualna procesu może być większa niż pamięć fizyczna. 

**Eksperyment**
```bash
cat /proc/[PID]/status |grep -e VmSize #tu będzie rozmiar pamięci wirtualnej
getconf PAGE_SIZE #rozmiar strony - dzielimy VmSize przez to i mamy ilość stron
 cat /proc/meminfo | grep -e MemTotal # a tu mamy cały RAM, który możemy tak samo przeliczyć na ramki
```

***

Jaki rozmiar zajmują wszystkie strony procesu a jaki wszystkie przydzielone dla procesu ramki oraz jak nazywa się sytuacja, gdy potrzebna strona nie znajduje się w ramce procesu (należy wymienić możliwe warianty tej sytuacji oraz gdzie system będzie szukał potrzebnej strony)?  - wymagane wskazanie odpowiednich wartości w listingach.

Kto zgłasza błędy braku strony, jakie są rodzaje błędów stron oraz jakie odpowiadają im chybienia i przestoje związane z ich obsługą, które z błędów stron występują częściej dla procesu?

Na czym polega zjawisko pobocznego błędu strony i jakie działania podejmuje system operacyjny w tej sytuacji?  

Jakie odwzorowania przechowuje tablica stron procesu, jak jest zbudowana i kto odczytuje jej zawartość? Czym jest pamięć asocjacyjna TLB? 

Czym jest obszar pamięci swap cache, jaki jest jego rozmiar w systemie operacyjnym i w jakiej sytuacji do tego obszaru zostaje przydzielona ramka ze stroną? 

W jakiej sytuacji występuje zjawisko głównego błędu strony i jakie działania podejmuje system operacyjny w celu obsługi tej sytuacji?  

Gdzie system operacyjny szuka potrzebnej strony w przypadku wystąpienia głównego błędu braku strony? 

Z jakich elementów zbudowana jest przestrzeń wymiany, jakie rozróżniamy rodzaje tych elementów? 

Jeżeli plik zawierający program/bibliotekę ELF zostanie usunięty, to skąd system operacyjny będzie doczytywał potrzebne strony, jaki jest związek pomiędzy niezwolnionymi blokami systemu plików a stronami? - omówienie statystyk węzłów i bloków systemu plików, aby stwierdzić czy po usunięciu pliku zostały one zwolnione wraz z usuniętym wpisem katalogowym. 

Z jakiego obszaru pamięci ulotnej wybierane są niepuste ramki i jakie kryteria musi spełniać ramka wybrana przez algorytm wymiany? - analiza wyników free –w dotycząca różnicy w wartościach pomiędzy kolumną free i available w wierszu mem. 

Czym jest presja pamięci oraz migotanie, jakie działania podejmuje system operacyjny w przypadku napotkania presji pamięci?

Na czym polega procedura OOM i jakie ma negatywne skutki? 

Jakie zasoby systemowe trzeba monitorować i jakie działania można podjąć aby procedura OOM nie była zastosowana? - wymagane zaprezentowanie odpowiednich wartości w listingach.