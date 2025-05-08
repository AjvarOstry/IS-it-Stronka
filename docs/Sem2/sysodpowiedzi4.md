# Sysodpowiedzi 4

### 4.1

#### 4. Do czego służy klasyfikacja procesów na ograniczone procesorem i ograniczone wejściem wyjściem oraz na jakiej podstawie zweryfikowano, że proces jest ograniczony wejściem-wyjściem? - weryfikacja przygotowanych w listingach obliczeń i wskazanie w nich wirtualnego czasu życia procesu realizującego odczyty z dysku /dev/sda.

Procesy realizują **zadania**. Jeden wątek to jedno zadanie
Klasyfikujac procesy możemy usprawnić przetwarzanie zadań przez proces poprzez dostosowanie przydziału jednostek przetwarzających lub zmianę polityki szeregowania.

Proces ograniczony procesorem zwykle czeka na jednostkę przetwarzającą, proces ograniczony w-w na **operacje wejścia-wyjścia** odczyt i zapis - na przykład oprecaje dyskowe. Możemy je przyspieszyć zmianą klasy(`ionice -c`) lub priorytetu (`ionice -n`) kiedy używamy algorytmu windy **bfq**

Nie zawsze możemy przyśpieszyć wykonywanie zadania. Na przykład kiedy czekamy na użytkownika, to go nie przyspieszymy.

Eksperyment:
```bash
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

**%cpu** reprezentuje procentowy udział czasu procesora (CPU) wykorzystanego przez proces w stosunku do całkowitego dostępnego czasu CPU

```bash
dd if=/dev/sda of=/dev/null bs=1G count=100 &
ps -o pid,cmd,%cpu -p <PID>
    PID CMD                         %CPU
  58215 dd if=/dev/sda of=/dev/null 48.0

```

***

#### 6. Co reprezentuje wartość %mem dla procesu? - wymagane wykazanie w listingach wartości, na bazie których %mem jest wyliczany, czy wartość %mem może przekroczyć 100%? - wymagane zaprezentowanie %mem dla procesu jednowątkowego w listingu.

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

#### 12. Jaki rozmiar zajmują wszystkie strony procesu a jaki wszystkie przydzielone dla procesu ramki oraz jak nazywa się sytuacja, gdy potrzebna strona nie znajduje się w ramce procesu (należy wymienić możliwe warianty tej sytuacji oraz gdzie system będzie szukał potrzebnej strony)?  - wymagane wskazanie odpowiednich wartości w listingach.

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

#### 20. Jeżeli plik zawierający program/bibliotekę ELF zostanie usunięty, to skąd system operacyjny będzie doczytywał potrzebne strony, jaki jest związek pomiędzy niezwolnionymi blokami systemu plików a stronami? - omówienie statystyk węzłów i bloków systemu plików, aby stwierdzić czy po usunięciu pliku zostały one zwolnione wraz z usuniętym wpisem katalogowym. 

#### 21. Z jakiego obszaru pamięci ulotnej wybierane są niepuste ramki i jakie kryteria musi spełniać ramka wybrana przez algorytm wymiany? - analiza wyników free –w dotycząca różnicy w wartościach pomiędzy kolumną free i available w wierszu mem. 

#### 22. Czym jest presja pamięci oraz migotanie, jakie działania podejmuje system operacyjny w przypadku napotkania presji pamięci?

#### 23. Na czym polega procedura OOM i jakie ma negatywne skutki? 

#### 24. Jakie zasoby systemowe trzeba monitorować i jakie działania można podjąć aby procedura OOM nie była zastosowana? - wymagane zaprezentowanie odpowiednich wartości w listingach.

## Przydatne polecenia:
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

