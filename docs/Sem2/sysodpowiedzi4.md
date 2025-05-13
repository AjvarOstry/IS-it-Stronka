# Sysodpowiedzi 4

### 4.1

#### 4. Do czego służy klasyfikacja procesów na ograniczone procesorem i ograniczone wejściem wyjściem oraz na jakiej podstawie zweryfikowano, że proces jest ograniczony wejściem-wyjściem? - weryfikacja przygotowanych w listingach obliczeń i wskazanie w nich wirtualnego czasu życia procesu realizującego odczyty z dysku /dev/sda.

Procesy realizują **zadania**. Jeden wątek to jedno zadanie
Klasyfikujac procesy możemy usprawnić przetwarzanie zadań przez proces poprzez dostosowanie przydziału jednostek przetwarzających lub zmianę polityki szeregowania.

Proces ograniczony procesorem zwykle czeka na jednostkę przetwarzającą, proces ograniczony w-w na **operacje wejścia-wyjścia** odczyt i zapis - na przykład oprecaje dyskowe. Możemy je przyspieszyć zmianą klasy(`ionice -c`) lub priorytetu (`ionice -n`) kiedy używamy algorytmu windy **bfq**

Nie zawsze możemy przyśpieszyć wykonywanie zadania. Na przykład kiedy czekamy na użytkownika, to go nie przyspieszymy.

```bash
### Eksperyment do zadania 4 ###
echo bfq > /sys/block/sda/queue/scheduler
cat /sys/block/sda/queue/scheduler

time perf sched record ionice -c 1 -n 0 dd if=/dev/sda of=/dev/null bs=1G count=100
pidof dd #tu: 6479
perf sched latency -p |grep -E "Task|dd:6479"

# 100 * (real - (Runtime(s) + (count * Avg delay(s)))) / real
echo "scale=3; 100 * (28.919 - (11.966 + (3466 * 0.000026))) / 28.919" | bc

```
**Czas wirtualny życia procesu** liczymy ze wzoru vruntime = runtime * (NICE_0_WEIGHT / process_weight). Jednak dla nice = 0 jest to po prostu runtime. Jest to czas w stanie wykonywanym, czyli czas realizacji zadania czas w jakim kod programu był wykonywany.

***

#### 5. Co reprezentuje wartość %cpu dla procesu jednowątkowego? - dla jakiego procesu wartość %cpu może przekroczyć 100%? - wymagane zaprezentowanie %cpu dla procesu jednowątkowego w listingu.

**%cpu** reprezentuje procentowy udział czasu procesora (CPU) wykorzystanego przez proces w stosunku do całkowitego dostępnego czasu CPU, w skrócie **stosunek wirtualnego życia procesu do rzeczywistego**

```bash
### Eksperyment do zadania 5 i 6 ###
dd if=/dev/sda of=/dev/null bs=1G count=100 &
ps -o pid,cmd,%cpu,%mem -p <PID> 
    PID CMD                         %CPU
  58215 dd if=/dev/sda of=/dev/null 48.0

```
***

#### 6. Co reprezentuje wartość %mem dla procesu? - wymagane wykazanie w listingach wartości, na bazie których %mem jest wyliczany, czy wartość %mem może przekroczyć 100%? - wymagane zaprezentowanie %mem dla procesu jednowątkowego w listingu.

**%mem** to procentowy udział pamięci fizycznej (RAM) zajętej przez proces w stosunku do całkowitej dostępnej pamięci fizycznej w systemie.

odnosi się tylko do pamięci fizycznej (RAM). Proces może jednak zaalokować znacznie więcej pamięci wirtualnej, co nie wpływa na %mem.
```bash
### listing do zadania 6, 9, 10, 12 ###
pmap -x 20754
20754:   dd if=/dev/sda of=/dev/null bs=1G count=100
Address           Kbytes     RSS   Dirty Mode  Mapping
0000560d99d91000      40      40       0 r-x-- dd #segment z pliku
0000560d99d9b000      20      20       0 r---- dd 
0000560d99da0000       4       4       4 r---- dd
0000560d99da1000       4       4       4 rw--- dd
0000560dc0683000     132      16      16 rw---   [ anon ] #sterta
00007f0b367fe000 1048584 1048580 1048580 rw---   [ anon ] # bufor zalokowany dynamicznie tworzony przez bs=1G
00007f0b76800000  226908     416       0 r---- locale-archive
00007f0b8466c000     360     192       0 r---- coreutils.mo
00007f0b846c6000      12       8       8 rw---   [ anon ]
00007f0b846c9000       4       4       0 r---- libc.so.6
00007f0b846ca000    1468     996       0 r-x-- libc.so.6 # rozmiar 1468, ale tylko 996 zalokowane od pamięci
00007f0b84839000     468     328       0 r---- libc.so.6
00007f0b848ae000      16      16      16 r---- libc.so.6
00007f0b848b2000       8       8       8 rw--- libc.so.6
00007f0b848b4000      40      24      24 rw---   [ anon ]
00007f0b848d0000      28      28       0 r--s- gconv-modules.cache
00007f0b848d7000       8       0       0 r----   [ anon ]
00007f0b848d9000       8       0       0 r----   [ anon ]
00007f0b848db000       8       8       0 r-x--   [ anon ]
00007f0b848dd000       4       4       0 r---- ld-linux-x86-64.so.2 #biblioteka
00007f0b848de000     164     164       0 r-x-- ld-linux-x86-64.so.2
00007f0b84907000      44      44       0 r---- ld-linux-x86-64.so.2
00007f0b84912000       8       8       8 r---- ld-linux-x86-64.so.2
00007f0b84914000       8       8       8 rw--- ld-linux-x86-64.so.2
00007ffed70d2000     132      16      16 rw---   [ stack ] #stos
ffffffffff600000       4       0       0 --x--   [ anon ]
---------------- ------- ------- ------- 
total kB         1278484 1050936 1048692

free -k
#kolumna total wiersz Mem
               total        used        free      shared  buff/cache   available
Mem:         8088064     2471308     1270548        7736     4773384     5616756 
Swap:        8087548         764     8086784
```
***

**%mem** jest to **stosunek** fizycznej pamięci ulotnej procesu **total RSS do** fizycznej
pamięci ulotnej systemu operacyjnego **total mem**.

#### 9. Który z segmentów procesu zawiera stertę, a który stos i czy są one segmentami anonimowymi? - wymagane wskazanie segmentu sterty i stosu w mapie pamięci wybranego procesu.

**Stos ([ stack ]):**
- Zawsze oznaczony jako [ stack ] w pmap lub /proc/[pid]/maps.
- Ma uprawnienia rw--- (odczyt i zapis, bez wykonywania).
- Zazwyczaj ma rozmiar od kilkudziesięciu KB do kilku MB (domyślny limit stosu to 8 MB, ale używana jest tylko część).
- Znajduje się w wysokich adresach pamięci (np. 0x7ffc... w Twoim przykładzie).
**Sterta ([ anon ]):**
- Oznaczona jako [ anon ], ponieważ nie jest powiązana z plikiem.
- Ma uprawnienia rw---, bo jest modyfikowalna.
- Zwykle znajduje się w niższych adresach niż stos, ale powyżej segmentów kodu i danych programu (np. 0x55d... lub 0x7f...).
- Rozmiar zależy od programu: proste programy (np. dd) mają małą stertę (kilkaset KB), a aplikacje takie jak przeglądarki mogą mieć sterty rzędu GB.
Uwaga: Duże segmenty [ anon ] (np. 1 TB w Twoim przykładzie) mogą być rezerwacjami przestrzeni adresowej, a nie stertą. Sterta jest zwykle mniejsza i rośnie dynamicznie.


**Kolejność w typowym układzie pamięci**
W 64-bitowym Linuksie (x86_64) przestrzeń adresowa procesu jest zorganizowana od niskich do wysokich adresów w następującej kolejności (na podstawie Twojego przykładu i ogólnych zasad):

- Segment kodu programu (np. 000055d7e43d0000 r-x-- dd).
- Segment danych i BSS (np. 000055d7e43da000 r---- dd, 000055d7e43e0000 rw--- dd).
- Sterta (np. 000055d810e92000 rw--- [ anon ]).
- Biblioteki współdzielone (np. 00007f160b42b000 r-x-- libc.so.6, 00007f160b63f000 r-x-- ld-linux-x86-64.so.2).
- Mapowania plików i inne segmenty anonimowe (np. 00007f15fd600000 r---- locale-archive, 00007f15bd5fe000 rw--- [ anon ]).
- Stos (np. 00007ffc4a68a000 rw--- [ stack ]).
- Segmenty systemowe (np. ffffffffff600000 --x-- [ anon ]).
***

#### 10. Czy całość jego kodu i danych jest wczytywana z pliku programu ELF oraz plików bibliotek ELF do fizycznej pamięci ulotnej?  - wymagane zaprezentowania odpowiednich wartości dla przykładowego nieanonimowego segmentu danych i segmentów tekstu w listingu z mapą pamięci procesu jednowątkowego, gdzie nie wszystkie strony segmentu są załadowane do ramek.

Część kodu nie pochodzi ani z pliku progrmu ELF, ani z bibliotek, ale z **segmentów anonimowych**, które reperezentują na przykład pamięć alokowaną dynamicznie jak sterta, czy ten bufor.

Alternatywna wersja rozumienia tego pytania:
Tylko aktualnie niezbędne fragmenty kodu programu i bibliotek są ładowane do pamięci, na przykład biblioteka `libc`
Pamięć wirtualna (Kbytes, łączny rozmiar stron) jest większa od fizycznej (RSS, łączny rozmiar ramek).

**Przydasie:**
- Kod (.text) → segment `r-xp`
- Stałe (.rodata) → segment `r--p`
- Zmienne (.data, .bss) → segment `rw-p`
***

#### 12. Jaki rozmiar zajmują wszystkie strony procesu a jaki wszystkie przydzielone dla procesu ramki oraz jak nazywa się sytuacja, gdy potrzebna strona nie znajduje się w ramce procesu (należy wymienić możliwe warianty tej sytuacji oraz gdzie system będzie szukał potrzebnej strony)?  - wymagane wskazanie odpowiednich wartości w listingach.

**Rozmiar stron:** total Kbytes - pamięć, do której proces ma dostęp. Może być większa od pamięci fizycznej ulotnej
**Rozmiar ramek:** total RSS -  nie uwzględnia swap, ale uwzględnia pamięć bibliotek współdzielonych, czyli pokazuje tylko aktualny stan ramek w pamięci fizycznej. Do ramki załadowane są tylko potrzebne strony

***

#### 20. Jeżeli plik zawierający program/bibliotekę ELF zostanie usunięty, to skąd system operacyjny będzie doczytywał potrzebne strony, jaki jest związek pomiędzy niezwolnionymi blokami systemu plików a stronami? - omówienie statystyk węzłów i bloków systemu plików, aby stwierdzić czy po usunięciu pliku zostały one zwolnione wraz z usuniętym wpisem katalogowym. 

System operacyjny będzie odczytywał strony **z przestrzeni wymiany swap.**

W eksperymencie pokazano dostępne bloki i węzły. Nie zostaną zwolnione, póki nie zakończymy procesu.
Usuwany jest tylko wpis katalogowy, usunięcie bloków i węzłów jest zablokowane przez mechanizm pamięci wirtualnej. 

W tej sytuacji bloki są stronami, rozmiar bloków systemu plików
jest równy rozmiarowi pojedynczej strony. Mapowanie 1:1, 4kib do 4kib

```bash 
### Eksperyment do zadania 20 ###
# Terminal 1
which nano
/usr/bin/nano
cp /usr/bin/nano /home/sysop/nano
file /home/sysop/nano 
/home/sysop/nano: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=234eb7d3646eff02d31a29c4176f223f55eb89d8, for GNU/Linux 3.2.0, stripped
/home/sysop/nano 

#terminal 2
pidof nano
22570
stat -f /home
  Plik: "/home"
    ID: dd47b0d54e281a54 długość nazwy: 255     typ: ext2/ext3
rozmiar bloku: 4096       podstawowy rozmiar bloku: 4096
bloków: Razem: 755402     wolnych: 711510     dostępnych: 668093
Inody: razem: 196608     wolnych: 194206

rm -r /home/sysop/nano
stat -f /home
  Plik: "/home"
    ID: dd47b0d54e281a54 długość nazwy: 255     typ: ext2/ext3
rozmiar bloku: 4096       podstawowy rozmiar bloku: 4096
bloków: Razem: 755402     wolnych: 711510     dostępnych: 668093
Inody: razem: 196608     wolnych: 194206

kill 22570
stat -f /home # tu będzie więcej wolnych, ale mi się nie chciało
  Plik: "/home"
    ID: dd47b0d54e281a54 długość nazwy: 255     typ: ext2/ext3
rozmiar bloku: 4096       podstawowy rozmiar bloku: 4096
bloków: Razem: 755402     wolnych: 711510     dostępnych: 668093
Inody: razem: 196608     wolnych: 194206
```
***

#### 21. Z jakiego obszaru pamięci ulotnej wybierane są niepuste ramki i jakie kryteria musi spełniać ramka wybrana przez algorytm wymiany? - analiza wyników free –w dotycząca różnicy w wartościach pomiędzy kolumną free i available w wierszu mem. 

***

#### 24. Jakie zasoby systemowe trzeba monitorować i jakie działania można podjąć aby procedura OOM nie była zastosowana? - wymagane zaprezentowanie odpowiednich wartości w listingach.

***

## (nie)Przydatne polecenia:
```bash
ps -o
    pid	#Process ID
    ppid	#Parent Process ID
    lwp	    #Light Weight Process (czyli ID wątku)
    nlwp	#Number of threads in process
    uid	    #ID właściciela
    user	#Nazwa użytkownika
    gid	    #Group ID
    group	#Nazwa grupy
    comm	#Nazwa programu
    cmd	    #Komenda z argumentami
    args	#Alias cmd
    stat	#Stan procesu (S, R, Z, itd.)
    stime	#Czas startu procesu
    time	#Całkowity czas procesora
    c	    #CPU usage
    pri	    #Priorytet
    ni	    #Nice value
    psr	    #Numer CPU, na którym aktualnie działa
    tty	    #Terminal
    rss	    #Resident Set Size (RAM, KB)
    vsz	    #Virtual memory size (KB)
    pcpu	#Procent użycia CPU
    pmem	#Procent użycia pamięci
    start	#Data i czas rozpoczęcia
    etimes	#Czas działania w sekundach
    etime	#Formatowany czas działania
    flags	#Flagi procesu
    sid	    #Session ID
    pgid	#Process Group ID
    session	#Alias sid
    wchan	#Nazwa kernela, w którym proces "czeka"
    fname	#Nazwa pliku wykonywalnego
    sgi_p	#Alias sgid
    rssize	#Rozmiar pamięci RAM
    nice	#Alias ni
    class	#Klasa harmonogramu
    policy	#Polityka planowania
    maj_flt	#Liczba dużych page faultów
    min_flt	#Liczba małych page faultów
```

