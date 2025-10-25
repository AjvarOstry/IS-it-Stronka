# Sysodpowiedzi 4.4
*
#### 6. Jeżeli dwa procesy rywalizują o jednostkę przetwarzającą i mają identyczną politykę szeregowania SCHED_RR oraz różne wartości rtprio, to które z nich uzyska preferencję w przydziale jednostki przetwarzającej i który z nich uzyskuje korzystniejszy przydział jednostki przetwarzającej?

 *wymagane zaprezentowanie przypadku z rywalizacją o jednostkę przetwarzającą dwóch procesów. Kiedy wątek w stanie gotowym do wykonania, który zostanie przydzielony do kolejki na niższym poziomie (zgodnie z wielopoziomowymi kolejkami w planowaniu wielokolejkowym) uzyska przydział jednostki przetwarzającej?*

 ```bash
 #### Eksperyment do zadania 6 ####
lscpu |grep On-line
chcpu -d 1-11
lscpu |grep On-line

 chrt -r 1 yes zadanie6 > /dev/null & chrt -r 6 yes zadanie6 > /dev/null &
 # czekamy półtorej minuty
 ps -o pid,tty,etime,cmd,policy,rtprio,pcpu,cputime -C yes
killall -v yes

```
**Round Robin** to kolejka rotacyjna

Preferencję jednostki przetwarzającej (większe wykorzystanie CPU) uzyska proces o większej wartości
priorytetu statycznego rtprio. Im wyższa wartość priorytetu statycznego, tym proces jest bardziej
preferowany w przydzialue CPU przez planistę krótkoterminowego. 
Im wyższy jest poziom kolejki (wartość rtprio), tym kwant czasu przydziału 
jednostki przetwarzającej jest krótszy. (na krotszy okres czasu ale z wieksza czestotliwoscia)
Te dwa procesy trafiają do 2 różnych kolejek na 2 różnych poziomach co wynika z rtprio.

**Planista krótkoterminowy** - element jądra systemu operacyjnego odpowiedzialny za ustalanie kolejności przydziału jednostki przetwarzającej dla rywalizujących o nią zadań realizowanych w wątkach.

**Priorytet statyczny rtprio** - w planowaniu wielokolejowym, USTALA POZIOM KOLEJKI,  do której trafia wątek w stanie gotowym do wykonania w oczekiwaniu na przydział jednostki przetwarzając

**Zajęciowe:**
Czy można wyłączyć wszystkie jednostki przetwarzające? 
    Nie, bo by nie było przetwarzania

Jaki cel wyłączenia jednostek?
    żeby zapewnić rywalizację o jednostkę przetwarzającą

Żeby był lepszy przydział - wyższy priorytet niezależnie od polityki

Rtprio ustala poziom kolejki

Ten, który ma wyższy priorytet ma krótszy kwant czasu, ale częściej

Drugi może korzystać, kiedy pierwszy zwolni jednostkę przez zablokowanie lub zakończenie zadania
kolejki na wyższych poziomach muszą być puste

2. korzystniejszy przydział otrzymał wyższy priorytet
3. rtprio reprezentuje poziom kolejki

5. jeżeli ten o wyższym przejdzie w tan zablokowany lub się zakończy
    

***
#### 8. Co powoduje, że przy identycznych niezerowych, wartościach rtprio proces z polityką SCHED_FIFO wygrywa rywalizację o jednostkę przetwarzającą z procesem posiadającym politykę SCHED_RR? Które z przełączeń kontekstu nie będą występowały pomiędzy tymi zadaniami? - wymagane zaprezentowanie przypadku z rywalizacją o jednostkę przetwarzającą dwóch procesów. 

```bash
#### Eksperyment do zadania 8 ####
 chrt -f 1 yes zadanie8 > /dev/null & chrt -r 1 yes zadanie8 > /dev/null &

ps -o pid,tty,etime,cmd,policy,rtprio,pcpu,cputime -C yes
killall -v yes
```
**Zajęciowe:**
Polityki numa - liczba mnoga
reguły koliacji
rtprio
czy zawsze?
SCHED_DEADLINE - bo to zależy od parametrów
polityka szeregowania
poziom uprzejmości nice
Honorowane przez Bash i Other
reguła arbitrażu

rr - round robin - algorytm cykliczny karuzelowy
Czy trafiły do tej samej kolejki?
Nie - dwie różne kolejki tego samego poziomu - różnią się alogrytmem szeregowania

Dlaczego korzystniejszy przydział?
Bo brak wywłaszczeń fcfs -> round robin (wywłaszczenie - wymuszone przełaczenie kontekstu)

korzystniejszy jest algrytm fcfs, bo nie obsługuje wywłaszczenia
może przekazać przez dobrowolne przełączenie kontekstu.
    
w rr wywłaszczenie występuje, kiedy skończy się przydzielony kwant czasu

co jak weźmiemy fifo 99 stress  i rr mniej
 system się zatrzyma, bo zadania jądra ne będą się wykonywać. (niektóre zadania jądra mają 4)

Te dwa procesy trafiają do 2 różnych kolejek oraz maja ten sam poziom kolejki.
Kolejki różnią się tym, że dla procesu o polityce szeregowania SCHED_FIFO 
kolejka będzie zarządzana algorytmem szeregowania FCFS (First Come First Served), 
a dla SCHED_RR będzie to algorytm szeregowania RR (Round Robin) - cyklicznie 
sprawiedliwy (zwany również karuzelowym/rotacyjnym).
Z racji tego, że algorytm FCFS nie obsługuje wywłaszczeń, SCHED_FIFO może 
być wywłaszczany tylko przez proces o wyższym priorytecie statycznym rtprio, 
//albo przy operacji wejscia-wyjscia
a SCHED_RR nie ma wyższej wartości rtprio, bo mamy takie same wartości. A dla 
SCHED_RR mogą być wywłaszczenia, po upływie kwantu czasu przydziału jednostki 
przetwarzającej. SCHED_FIFO ma nielimitowany czas przydziału jednostki przetwarzającej.
Nie będzie wymuszonych przełączeń kontekstu z FIFO do Round Robin. 
W drugą stronę będą możliwe.
Mogą występować dobrowolne przełączenia kontekstu w obu kierunkach.

SCHED_FIFO (FCFS - first come, first served) - zarządza wykonaniem zadań gotowych do 
wykonania || wykonanie procesów/wątków zgodnie z
kolejnością zgłoszeń w systemie operacyjnym

SCHED_RR (round robin) - po wykorzystaniu przydzielonego kwantu czasu procesora
proces/wątek jest ponownie umieszczany w kolejece gotowych do wykonania. Następnie
wybierany jest kolejny proces/wątek, któremu zgodnie z ustalonym w kolejce porządkiem,
przydzielana jest jednostka przetwarzająca na identyczny kwant czasu.

***
#### 9. Jaką charakterystykę przydziału jednostki przetwarzającej posiada polityka szeregowania SCHED_DEADLINE i jakie parametry ustala dla procesu? - wymagane zaprezentowanie dwóch przypadków z rywalizacją o jednostkę przetwarzającą dwóch procesów, z których jeden posiada politykę SCHED_DEADLINE a drugi SCHED_FIFO, dobierając tylko wartość parametru przydziału jednostki przetwarzającej dla SCHED_DEADLINE należy wykazać, że w pierwszym przypadku rywalizację o jednostkę przetwarzającą wygra proces z polityką SCHED_DEADLINE, a w drugim przypadku ten proces przegra rywalizację.

```bash
echo 0 > /proc/sys/kernel/sched_deadline_period_min_us
cat /proc/sys/kernel/sched_deadline_period_min_us

# Deadline wygrywa
chrt -r 4 yes Przypadek3 > /dev/null & chrt -d -T 80000 -D 
80000 -P 95000 0 yes Przypadek 3 > /dev/null &

ps -o pid,tty,etime,cmd,policy,rtprio,pcpu,cputime -C yes
killall -v yes

# Deadline przegrywa
chrt -r 4 yes Przypadek3 > /dev/null & chrt -d -T 10000 -D 80000 -P 95000 0 yes Przypadek 3 > /dev/null &

ps -o pid,tty,etime,cmd,policy,rtprio,pcpu,cputime -C yes
killall -v yes
```
**Zajęciowe:**
    Od dobranych parametrów
    
Polityki numa - liczba mnoga, reguły koliacji, rtprio
czy zawsze? SCHED_DEADLINE - bo to zależy od parametrów
polityka szeregowania, poziom uprzejmości nice - Honorowane przez Bash i Other
reguła arbitrażu, zasada natychmiastowości przydziału
Deadline gwarantuje przydział
Co się musi odbyć w okresie rozliczeniowym?
Przydział musi się odbyć w okresie - przed linią krytyczną

Deadline oznacza linię krytyczną, do kiedy całościowy przydział musi się odbyć
    Deadline regulujemy przerwy między przydziałami

Kiedy system nie jest w stanie zagwarantować to nie obsłuży deadline w ogóle
Bo deadline gwarantuje przydział - żeby zapewnić gwarancję

Między linią krytyczną, a rozpocząciem nowego przydziału, czyli między 
kwant czasu?

przerwa w przydziale jednostki przetwarzającej dla procesu

Parametry:
-T - wykonanie (runtime), jest to maksymalny gwarantowany czas przydziału jednostki przetwarzającej (nie dla calego zycia procesu), dla zadania w każdym z okresów rozliczeniowych
-D - termin/deadline, wyznacza linię krytyczną przed którą musi odbyć się **cały** przydział
jednostki przetwarzającej, termin jest liczony od początku każdego okresu rozliczeniowego
-P - okres rozliczeniowy, wpływa na **częstotliwość** przydziału jednostki przetwarzającej dla
procesu, im dłuższy okres rozliczeniowy, tym rzadziej będzie przydzielana jednostka

Proces z tą polityką SCHED_DEADLINE ma zawsze rtprio (piorytet) ustawione na 0, jednak mimo tego ma najwyższą preferencję i gwarantuje przydział jednostki przetwarzającej, jeśli ustawione są odpowiednie parametry. 

***
#### 10. Jaka jest kolejność rozpatrywania ustawień w sytuacji rozstrzygania, gdy dwa procesy jednowątkowe gotowe do wykonania rywalizują o jednostkę przetwarzającą? - podaj odpowiednią kolejność rozpatrywania reguł od najważniejszych do najmniej istotnych (polityka szeregowania, polityka NUMA, poziom kolejki rtprio oraz priorytet nice, reguła arbitrażu, reguły koligacji).

Rozstrzyga to planista krótkoterminowy.

1. Polityka NUMA (dla obu procesów) - nie można  zapewnić przydziału jednostek nieaktywnych

2. reguły koligacji - (z numa?)

3. poziom kolejki rtprio = tutaj wyjątkiem jest SCHED_DEADLINE bo przy odpowiednich 
parametrach ma to w pizdzie i leci po milion
(3 <-> 4 ?)
4. polityka szeregowania (SCHED_DEADLINE > FIFO > RR > OTHER > BATCH > IDLE)

5. priorytet nice = poziom uprzejmości honorowany przez SCHED_OTHER 
(dedykowana dla zadań interaktwynych) i SCHED_BATCH (d. dla zadań wsadowych)

6. reguła arbitrażu


**Zajęciowe:**
1. Polityki numa - liczba mnoga
2. reguły koliacji 
3. rtprio
    czy zawsze?
        SCHED_DEADLINE - bo to zależy od parametrów
4. polityka szeregowania
5. **poziom** uprzejmości nice
        Honorowane przez Bash i Other
6. reguła arbitrażu

Zasada, która mówi, że jak jest zasób to należy go przyznać?
        Nie może być złamana zasada natychmiastowości przydziału
    



> **Reguły koligacji** zawsze wymagają przyporządkowania przynajmniej jednej jednostki przetwarzającej dla procesu.
> **bez wywłaszczenia** (ang. nonpreemptive) – wówczas wybierany jest proces (lub jeden z jego wątków), który jest wykonywany do czasu zablokowania, zgłoszenia wywołania systemowego, wystąpienia przerwania lub dobrowolnego zwolnienia przydzielonej jednostki przetwarzającej
**z wywłaszczaniem**(ang. preemptive) – dla wybranego procesu (lub jednego z jego wątków) przydzielana jest jednostka przetwarzająca na ściśle określony czas. Wymagane jest wystąpienie przerwania (najczęściej przerwania zegara) na końcu przyznanego przedziału czasu, wówczas program szeregujący odzyskuje sterowanie i może wybrać inny proces, któremu zostanie przekazana jednostka przetwarzająca. Wywłaszczenie umożliwia wymuszone przełączenie kontekstu.
> **Planista krótkoterminowy** (ang. scheduler) to program szeregujący wykorzystujący algorytm szeregowania w celu dokonania wyboru wątku procesu lub jądra, któremu zostanie przydzielona jednostka przetwarzająca. Algorytm szeregowania wyznacza politykę planowania przydziału jednostki przetwarzającej tzw. politykę szeregowania.
> **Polityka szeregowania** określa klasę szeregowania, do której należy proces. Przypisanie polityki szeregowania dla procesu powinno uwzględniać charakterystykę realizowanego zadania (np. interaktywne, pierwszoplanowe, wsadowe), ponieważ oprócz określenia kolejności przydziału jednostki przetwarzającej dla zadań wpływa także na częstotliwość i czas przydziału jednostki przetwarzającej.
> We współczesnym systemie operacyjnym GNU/Linux zastosowano planowanie wielokolejkowe z wywłaszczeniem, wyróżnia się 100 poziomów priorytetów statycznych, przy czym dla pojedynczego priorytetu statycznego może istnieć wiele kolejek zadań gotowych do wykonania (każda osobno dla różnych polityk szeregowania).Wyróżnia się polityki/klasy szeregowania zadań (chrt -m):
SCHED_FIFO - zadania czasu rzeczywistego o identycznej wartości priorytetu statycznego (zakres wartości 1..99) stosują algorytm FCFS.
SCHED_RR – zadania czasu rzeczywistego o identycznej wartości priorytetu statycznego (zakres wartości 1..99) stosują algorytm rotacyjny RR.
SCHED_DEADLINE – zadania czasu rzeczywistego o wartości 0 priorytetu statycznego i ustaloną linią krytyczną stosują algorytm EDF (Earliest Deadline First) wraz z CBS (Constant Bandwidth Server).
SCHED_OTHER – zadania zwykłe/normalne i interaktywne współdzielą czas procesora zgodnie z algorytmem CF, priorytet statyczny o wartości 0.
SCHED_BATCH – zadania wsadowe, priorytet statyczny o wartości 0.
SCHED_IDLE – zadania bezczynne/leniwe z wartością 0 priorytetu statycznego.
> 
