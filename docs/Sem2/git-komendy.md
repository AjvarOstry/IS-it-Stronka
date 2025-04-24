# Jest Git

### Szybka, rutynowa obsługa :sunglasses: :

**(Szczegóły są niżej :arrow_down:, tu szybka fiszka)**
    
Tak wygląda praca za każdym razem jak obsługujemy gita:

> git branch - sprawdzamy na jakim branchu jesteśmy
> git checkout *nazwa-brancha* - zmieniamy brancha


> git pull origin *nazwa-brancha* -  robimy pulla (zaciągamy brancha ze zdalnego repo)

> git status - sprawdzamy co mamy w stageu
> git add * - dodajemy wszystkie zmiany do commita
> git commit -m *"Wiadomosć"* - robimmy commita

> git push origin *nazwa-brancha* - robimy pusha (wrzucamy brancha do zdalnego repo)

### Branche 🪴

#### Nowy branch:
> git checkout -b *nazwa nowego brancha* - i od razu jesteśmy jeszcze przełączeni

#### Poruszanie się
> git branch - sprawdzamy na jakim branchu jesteśmy
> git checkout *nazwa_brancha* - przełaczamy się na dany branch

#### Łączenie
>  *robimy checkout do brancha do którego chcemy zmergeować...*
> git merge *nazwa brancha z ktorego mergeujemy* - łączymy branche

### Commity i stage :arrow_up:

#### Tworzenie commita:
> git status - wyświetla co jest w stage'u
> git add * - robimy stage (dodajemy do przysżłego commita) tu wszystkie pliki, można pojedynczo
> git commit -m *"Wiadomosć"* - robimmy commita

#### Cofanie commita: 
> git reset --soft HEAD~1 - cofa ostatniego commita bez kasowania zmian w plikach
> git restore --staged . - cofa wszystkie pliki ze stage (alternatywnie ścieżka do pliku zamiast kropki dla konkretnych plików)

### Tagowanie :label:

> git tag *Nazwa_taga* - robimy taga w danym miejscu na branchu na którym jesteśmy

> git push origin *Nazwa_taga* - pushujemy taga
> git push origin --tags - albo pushujemy wszystkie tagi na raz

### Pliki specjalne git :page_facing_up:
> .gitignore - w katalogu głównym repo. Tu dorzucamy ścieżki do plików/katalogów, które mają yć ignorowane. (Działa * i **)

> .gitkeep - pusty plik, który powoduje, że git widzi pusty katalog

### Cmake :gear: - tak dodatkowo

> cmake -G "Unix Makefiles" .. - czyta cmakelists i tworzy zestaw plików potrzebny do kompilacji (wpisujemy, kiedy zmieniliśmy coś w cmakelistach, dodaliśmy plik lub bibliotekę w kodzie)

> make - w build żeby zbudować projekt
> make -j - to samo, tylko leci wszystkimi wątkami porcesora