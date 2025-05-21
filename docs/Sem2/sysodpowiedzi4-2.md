# Sysodpowiedzi 4.2

#### 3. Należy wykazać, że liczba wątków w procesie wielowątkowym może się zarówno zwiększyć jak i zmniejszyć oraz, że wątki procesu mogą mieć różne cykle życia. - wymagane zaprezentowania odpowiednich wartości w listingu dla wybranego procesu wielowątkowego.

```bash
#### Eksperyment do zadania 3 i 4 ####
dnf install leafpad 
leafpad &
pidof leafpad
ps -T -o pid,spid,comm,stime,etime <PID>
ps -o nlwp <PID>

# klikasz zapisz jako i szybko wracasz do terminala zostawiając okno otwarte

ps -T -o pid,spid,stime,etime,comm <PID>
ps -o nlwp <PID>
```

**etime (elapsed)** - cykl życia wątku
**stime** - czas utworzenia wątku
**Wątek główny** - zawsze istnieje do konca życia procesu gdyż zwraca kod błędu, więc zakończy się jako ostatni.

> Wątek (tzw. lekki proces LWP ang. LightWeight Process) jest niezależnym zadaniem realizowanym w obrębie procesu lub jądra systemu posiadającym własne sterowanie wykonywaniem kodu programu. Każdy wątek posiada identyfikator SPID

> Wielowątkowość - możliwość działania wielu wątków w obrębie procesu, gdzie wątki wykonują oddzielne zadania *(wykład 11)*



***
#### 4. Ile włókien posiada zidentyfikowany proces wielowątkowy? - odpowiedź uzasadnić, czy sytuacja będzie korzystniejsza jeżeli wszystkie wątki procesu będą włóknami? Należy ustalić najkorzystniejszy przydział jednostek przetwarzających dla procesu wielowątkowego, gdy wszystkie wątki nie są włóknami. Jaki rodzaj przetwarzania równoległego występuje dla zadań procesu złożonego tylko z włókien?
**O chuj chodzi?**
> Wątki zaimplementowane w przestrzeni jądra – jądro systemu posiada tablicę wątków, w której znajdują się dane kontrolne każdego wątku,
wątki zaimplementowane w przestrzeni użytkownika (tzw. włókna) – struktury przechowujące stan wątków są przechowywane jedynie w przestrzeni adresowej procesu. Włókna nie są rozróżniane przez jądro systemu operacyjnego.
Istnieje możliwość stworzenia implementacji hybrydowej, łączącej w sobie cechy implementacji wątków w przestrzeni jądra oraz użytkownika. *(wykład 11)*
Wątki w przestrzeni jądra i użytkownika
Zalety:

>Jądro systemu nie musi posiadać żadnych struktur danych związanych z włóknami. Tablica wątków jest zaimplementowana w pamięci procesu wielowątkowego.
>Przydział procesora dla wątku realizowany wewnątrz procesu, możliwość precyzyjnego wyboru wątku względem ustalonych kryteriów (np. najbardziej obciążony wątek usługi sieciowej wskazywany przez ustalony w procesie wątek dyspozytora).
Wątki w ramach procesu mogą bez pośrednictwa jądra przekazywać przydzieloną jednostkę przetwarzającą.
Wady:

>Proces wielowątkowy jednocześnie może uzyskać przydział tylko pojedynczej jednostki przetwarzającej.
>W sytuacji, gdy wątek zostanie zablokowany w oczekiwaniu na realizację operacji wejścia-wyjścia, przydzielona wcześniej jednostka przetwarzająca jest przydzielana do innego procesu.

Włókna (wątki zaimplemenotwane w przestrzeni użytkownika) - struktury przechowujące
stan wątków, przechowywane jedynie w przestrzeni adresowej procesu. Włókna nie są
rozrózniane przez jądro systemu operacyjnego. 

**Włókna są niewidoczne dla systemu operacyjnego — istnieją wyłącznie w przestrzeni użytkownika i są zarządzane przez aplikację.**
Włókna przetwarzają pseudorównolegle - kilka włókien ma przydzieloną tą samą jednostkę przetwarzającą i muszą się nią dzielić. Każdy wątek może mieć przydzieloną 1 jednostkę przetwarzającą i wtedy mamy przetwarzanie równoległe.

Najkorzystniejszy przydział jednostek przetwarzających dla wielowątkowego procesu bezwłókien wynika z liczby wątków procesu, które są w stanie gotowym do wykonania lub wykonywanym (przy założeniu nie mniejszej liczby aktywnych jednostek przetwarzających).

Każdy z wątków procesu, który nie jest włóknem, może uzyskać przydział co najwyżej pojedynczej, aktywnej jednostki przetwarzającej.
W przypadku kiedy każdy wątek byłby włóknem, to moglibyśmy przydzielić tylko jedną jednostkę przetwarzającą dla całego procesu (dla wątku głównego).

//Kiedy wszystkie wątki są włóknami to będzie i tak wątek główny będzie rozróżnialny.




***
#### 13. Jakie wyróżnia się rodzaje przełączeń kontekstu zadań, które z nich występują najczęściej i w jakich sytuacjach? - wskaż w przygotowanych listingach dla poszczególnych wątków procesu liczbę odpowiednich przełączeń kontekstu.

```bash
#### Eksperyment do zadania 13 ####
at /proc/<PID>/status |grep switches
voluntary_ctxt_switches:	847 
nonvoluntary_ctxt_switches:	7 #
```

**dobrowolne/nieobowiązkowe przełączenie kontekstu (voluntary)**
występuje w następujących sytuacjach:
- zmiany stanu procesu/wątku na zablokowywany lub wstrzymany/zatrzymany, ew. zmiana trybu przetwarzania z nieuprzywilejowanego na uprzywilejowany w stanie wykonywanym
- wykonania rozkazu odstąpienia jednostki przetwarzającej, w wyniku wykonania którego jednostka przetwarzająca zostaje zwolniona lub zakończenia przetwarzania zadania.

**mimowolne/wymuszone przełączenie kontekstu (non voluntary)**
występuje(częściej) w sytuacji, gdy:
- dla procesu/wątku w stanie wykonywanym jednostka przetwarzająca jest odbierana w wyniku decyzji mechanizmów szeregujących systemu operacyjnego (np. upłynął przydzielony kwant czasu przydziału jednostki przetwarzającej lub w systemie pojawił się wątek jądra lub procesu gotowy do wykonania o wyższej preferencji w przydziale jednostki przetwarzającej).

> Zmiana przydziału jednostki przetwarzającej poprzez jej przekazanie pomiędzy dwoma wątkami tego samego procesu nazywa się **przełączeniem kontekstu wątków** (bo może być jeszcze procesów)
> Przełączenie kontekstu wątków w ramach jednego procesu jest mniej czasochłonne niż przełączenie kontekstu procesów. Wątki procesu nie są separowane przez system operacyjny a umożliwiają realizację różnych zadań, dlatego wątki te nazywane są lekkimi procesami LWP (Light Weight Process).
> Porównując przełączanie kontekstu wątków najwydajniej realizowane jest przełączenie kontekstu włókien (wątków w przestrzeni użytkownika), ponieważ nie wymaga udziału jądra systemu operacyjnego w realizacji tej operacji.



***
#### 18. Jakie ograniczenia wynikają z nadanej maski koligacji dla jednego z wątków procesu? - wymagane zaprezentowanie w listingu identyfikatorów jednostek przetwarzających jakie mogą być przyznawane dla wątku procesu ograniczonymi regułami koligacji.
```bash
pidstat -t -w -p <PID> # TID wszystkich wątków

taskset -ap <PID> # wyświetlenie masek koligacji dla wszystkich wątków
taskset –cpu-list -ap <PID> # wyświetlenie jednostek przetwarzających przypisanych do wątków

taskset -p 1 <TID> # ustawienie dla jednego wątku maski koligacji 1 , co oznacza, że będzie miał on przypisaną jednostkę przetwarzającą o identyfikatorze 0

taskset -ap <PID>
taskset –cpu-list -ap <PID>

printf "%08d\n" "$(bc <<< "obase=2; ibase=16; <CPU MASK>")" ###przeliczanie maski na binarny

```
**maska koligacji** (ang. CPU affinity mask) to binarny lub liczbowy sposób reprezentowania rdzeni CPU, na których proces lub wątek może się wykonywać.


***
#### 21. Zaprezentować, że zastosowana polityka NUMA do procesu wielowątkowego spowodowała ograniczenie wykorzystania zasobów jednostek przetwarzających i pamięci ulotnej z ustalonego pojedynczego węzła NUMA. - wymagane zaprezentowanie listingów.

```bash
#### Eksperyment do zadania 18 ####
numactl -H # informacje o topologii NUMA
numactl -m 1 -N 1 leafpad & #uruchamia proces tylko z przydziałem pamięci z węzła numa 1
# i z przypisaniem cpu w węźle 1
numastat -p <PID> #statystyki numa dla procesu
taskset -ap <PID> #pokazuje i ustawia maskę cpu
taskset --cpu-list -ap <PID> #pokazuje to co wyżej, tylko jako listę rdzeni, nie maskę
```
skumulowane na drugim wezle
zeby polityka numa istniala musi byc wiecej niz 1 wezel

numactl ustawia preferencje alokacji pamięci, ale:

Pliki binarne, biblioteki współdzielone (.so) i inne dane zmapowane przez system mogą być ładowane przez loader (ld.so) przed włączeniem polityki NUMA.

Te dane mogą już trafić np. na Node 0 lub domyślny node jądra.


***
#### 22. Wykazać z wykorzystaniem wybranego testu obciążeniowego NUMA, że przetwarzanie z wykorzystaniem zasobów systemowych jednego węzła może być efektywniejsze niż w przypadku przetwarzania z zastosowaniem zasobów z kilku/wszystkich węzłów NUMA. - wymagane zaprezentowanie listingów

```bash
sudo numademo 300m memset
memory interleaved on all nodes memset    Avg 10281.50 MB/s Max 27664.48 MB/s Min 4708.89 MB/s
memory on node 0 memset                   Avg 11160.88 MB/s Max 27408.97 MB/s Min 4688.89 MB/s
memory on node 1 memset                   Avg 11853.58 MB/s Max 28069.31 MB/s Min 4645.34 MB/s
memory on node 2 memset                   Avg 21310.64 MB/s Max 26893.46 MB/s Min 7833.96 MB/s
...
memory interleaved on 0 1 2 memset        Avg 15841.11 MB/s Max 27591.68 MB/s Min 4675.65 MB/s # no nie wykazaliśmy XD
#interleaved to jest przeplatany
```

Dla 3 węzłów średnia prędkość jest niższa niż dla jednego węzła, 
jest to spowodowane  opóźnieniem które wywołuje transfer danych

ta cała prędkość to chyab "Predkosc dostepu do pamieci" / 
"średnia przepustowość operacji zapisu do pamięci" / "średnia wydajność operacji zapisu 

Korzyść: ograniczając się do jednego węzła nie zachodzi transfer
 danych pomiędzy węzłami przez szyne interconnect które generuje opóźnienie
***
