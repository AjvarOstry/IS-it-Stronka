# Jest Git

### Szybka, rutynowa obsługa :sunglasses: :

**(Szczegóły są niżej :arrow_down:, tu szybka fiszka)**
    
Tak wygląda praca za każdym razem jak obsługujemy gita:
```bash
git branch #sprawdzamy na jakim branchu jesteśmy
git checkout <nazwa-brancha> #zmieniamy brancha


git pull origin <nazwa-brancha> #robimy pulla (zaciągamy brancha ze zdalnego repo)

# Tu sobie pracujemy na plikach lokalnie...

git status #sprawdzamy co mamy w stageu
git add * #dodajemy wszystkie zmiany do commita
git commit -m "Wiadomosć" #robimmy commita

git push origin <nazwa-brancha> #robimy pusha (wrzucamy brancha do zdalnego repo)
```
### Branche 🪴

#### Nowy branch:
```bash
git checkout -b <nazwa nowego brancha> #i od razu jesteśmy jeszcze przełączeni
```
#### Poruszanie się
```bash
git branch #sprawdzamy na jakim branchu jesteśmy
git checkout <nazwa_brancha> #przełaczamy się na dany branch
```
#### Łączenie
```bash
#robimy checkout do brancha do którego chcemy zmergeować...
git merge <nazwa brancha z ktorego mergeujemy> #łączymy branche
```
### Commity i stage :arrow_up:

#### Tworzenie commita:
```bash
git status #wyświetla co jest w stage'u
git add * #robimy stage (dodajemy do przysżłego commita) tu wszystkie pliki, można pojedynczo
git commit -m "Wiadomosć" #robimmy commita
```
#### Cofanie commita: 
```bash
git reset --soft HEAD~1 #cofa ostatniego commita bez kasowania zmian w plikach
git restore --staged . #cofa wszystkie pliki ze stage (alternatywnie ścieżka do pliku zamiast kropki dla konkretnych plików)
```
### Tagowanie :label:
```bash
git tag <Nazwa_taga> #robimy taga w danym miejscu na branchu na którym jesteśmy

git push origin <Nazwa_taga> #pushujemy taga
git push origin --tags #albo pushujemy wszystkie tagi na raz
```
### Pliki specjalne git :page_facing_up:
```bash
.gitignore #w katalogu głównym repo. Tu dorzucamy ścieżki do plików/katalogów, które mają yć ignorowane. (Działa * i **)

.gitkeep #pusty plik, który powoduje, że git widzi pusty katalog
```
