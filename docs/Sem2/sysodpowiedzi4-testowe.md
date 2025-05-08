#### 1. Jakie są podstawowe stany wątku procesu? 
(testowe)

Podstawowe są trzy:
- Zablokowany
- Gotowy do wykonania
- Wykonywany

***

#### 2. Jakie warunki muszą być spełnione aby wątek procesu mógł uzyskać przydział jednostki przetwarzającej?
(testowe)

Przydzielana jednostka musi być aktywna, a wątek musi być w stanie gotowym do wykonania

***

#### 3. Jaka jest charakterystyka jednowątkowego procesu ograniczonego wejściem-wyjściem a jaka procesu ograniczonego procesorem? 
(testowe)

Proces ograniczony wejściem-wyjściem będzie często zwalniał przydzieloną mu jednostkę
przetwarzającą, bo **ponad połowę** swojego czasu życia spędza w stanie zablokowanym. Wątek ograniczony procesorem będzie za to spędzał większą część czasu w dowolnym z dwóch pozostałych stanów - gotowym do wykonania lub wykonywanym.

***

#### 7. Z jakich obszarów składa się pamięć wirtualna procesu i czy one ze sobą bezpośrednio sąsiadują zgodnie z adresacją logiczną?
(testowe)

Pamięć wirtualna procesu składa się z segmentów pamięci, które z kolei podzielone są na strony pamięci. Nie muszą sąsiadować ze sobą, bezpośrednio, jednak muszą występować w określonej kolejności. Każdy z obszarów pamięci ma przypisane uprawnienia zależne od segmentu procesu, do jakiego należy *(co było już wykazywane w odpowiedzi z zadania 2 - polecenie `pmap`)*

***