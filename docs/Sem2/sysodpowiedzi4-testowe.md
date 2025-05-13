#### 1. Jakie są podstawowe stany wątku procesu? 

Podstawowe są trzy:
- Zablokowany
- Gotowy do wykonania
- Wykonywany

***

#### 2. Jakie warunki muszą być spełnione aby wątek procesu mógł uzyskać przydział jednostki przetwarzającej?

Przydzielana jednostka musi być aktywna, a wątek musi być w stanie gotowym do wykonania

***

#### 3. Jaka jest charakterystyka jednowątkowego procesu ograniczonego wejściem-wyjściem a jaka procesu ograniczonego procesorem? 

Proces ograniczony wejściem-wyjściem będzie często zwalniał przydzieloną mu jednostkę
przetwarzającą, bo **ponad połowę** swojego czasu życia spędza w stanie zablokowanym. Wątek ograniczony procesorem będzie za to spędzał większą część czasu w dowolnym z dwóch pozostałych stanów - gotowym do wykonania lub wykonywanym.

***

#### 7. Z jakich obszarów składa się pamięć wirtualna procesu i czy one ze sobą bezpośrednio sąsiadują zgodnie z adresacją logiczną?

Pamięć wirtualna procesu składa się z segmentów pamięci, które z kolei podzielone są na strony pamięci. Nie muszą sąsiadować ze sobą, bezpośrednio, jednak muszą występować w określonej kolejności. Każdy z obszarów pamięci ma przypisane uprawnienia zależne od segmentu procesu, do jakiego należy *(co było już wykazywane w odpowiedzi z zadania 2 - polecenie `pmap`)*

***

#### 8. Czym jest sterta procesu i co zawiera?

Sterta pamięci to dynamicznie przydzielany dla procesu obszar przeznaczony na dane wytworzone przez proces. Zwykle zawiera dane dynamicznie alokowane (`new` w c++), tablice itp.

***

#### 11. Czy proces będzie posiadał więcej ramek czy stron pamięci i co mogą one zawierać?

Pamięć wirtualna, [o której była mowa w zadaniu 7](#7-z-jakich-obszarów-składa-się-pamięć-wirtualna-procesu-i-czy-one-ze-sobą-bezpośrednio-sąsiadują-zgodnie-z-adresacją-logiczną) , dzieli się na strony.natomiast pamięć fizyczna (RAM) dzieli się na ramki, do których ładowane są strony. W każdej ramce mieści się jedna strona. Należy pamiętać, że pamięć wirtualna procesu może być większa niż pamięć fizyczna. 

**Eksperyment**
```bash
cat /proc/[PID]/status |grep -e VmSize #tu będzie rozmiar pamięci wirtualnej
getconf PAGE_SIZE #rozmiar strony - dzielimy VmSize przez to i mamy ilość stron
 cat /proc/meminfo | grep -e MemTotal # a tu mamy cały RAM, który możemy tak samo przeliczyć na ramki
```

***

#### 13. Kto zgłasza błędy braku strony, jakie są rodzaje błędów stron oraz jakie odpowiadają im chybienia i przestoje związane z ich obsługą, które z błędów stron występują częściej dla procesu?

W występują dwa podstawowe rodzaje błędu strony - główny i poboczny. Oba zgłaszane przez MMU
- Poboczny - jest wynikiem chybienia miękkiego
- Główny - jest wynikiem chybienia twardego - występuje rzadziej, powoduje większe opóźnienia

***

#### 14. Na czym polega zjawisko pobocznego błędu strony i jakie działania podejmuje system operacyjny w tej sytuacji?  

Błąd poboczny polega na tym, że strona znajduje się już w pamięci RAM, ale tablica stron nie wskazuje jeszcze na ramkę.
Ramka zawierająca potrzebną stronę zostaje zlokalizowana w pamięci swap cache i przydzielona do zbioru roboczego procesu. Tablica stron jest akutalizowana zgodnie ze zmianami, jakie wystąpiły w zbiorze roboczym

***

#### 15. Jakie odwzorowania przechowuje tablica stron procesu, jak jest zbudowana i kto odczytuje jej zawartość? Czym jest pamięć asocjacyjna TLB? 

Tablica stron procesu - nieciągła struktura danych, która zawiera odwzorowania adresów stron i ramek naeżących do procesu. Jej zawartość jest odczytywana przez układ zarządzania pamięcią MMU, która nie musi każdorazowo przy wykonaniu transformacji adresu logicznego na adres fizyczny przeszukiwać tablicy stron dzięki wykorzystaniu wbudowanej w MMU pamięci asocjacyjnej TLB. Jest wielopoziomowa i utrzymywana przez jądro systemu operacyjnego dla każdego procesu

***

#### 16. Czym jest obszar pamięci swap cache, jaki jest jego rozmiar w systemie operacyjnym i w jakiej sytuacji do tego obszaru zostaje przydzielona ramka ze stroną? 



***

#### 17. W jakiej sytuacji występuje zjawisko głównego błędu strony i jakie działania podejmuje system operacyjny w celu obsługi tej sytuacji?  

#### 18. Gdzie system operacyjny szuka potrzebnej strony w przypadku wystąpienia głównego błędu braku strony? 

#### 19. Z jakich elementów zbudowana jest przestrzeń wymiany, jakie rozróżniamy rodzaje tych elementów? 

#### 22. Czym jest presja pamięci oraz migotanie, jakie działania podejmuje system operacyjny w przypadku napotkania presji pamięci?

***

#### 23. Na czym polega procedura OOM i jakie ma negatywne skutki? 

***