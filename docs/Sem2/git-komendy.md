# Jest Git

Taki cheatsheet podstawowych komend do obługi repo. 

Rozwijam to na bieżąco i pracuję nad drugą częścią, która trochę wyjaśnia jaką strategię warto przyjąć podczas pracy pod nagłówkiem [Filozofia GITa](#filozofia-gita).

## Cheatsheet
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

git merge <nazwa-brancha> #jeżeli chcemy połączyć branch z innym

git push origin <nazwa-brancha> #robimy pusha (wrzucamy brancha do zdalnego repo)
```
### Jak zacząć?
Jeżeli zdalne reposytorium już istnieje i chcemy stworzyć je tylko lokalnie:
```bash
# przenosimy się do folderu, gdzie chcemy umieścić folder repo
git clone https://link-do.repo/tu/wklejamy

# a tak klonujemy jak chcemy zaciągnąć tylko 1 brancha
git clone -b nazwa_branch --single-branch
```
W przeciwnym razie zakładamy zdalne repo:
```bash
# w folderze, który będzie repo
git init
git commit -m "pierwszy commit"   # pierwszy commit
git branch -M main                # opcjonalnie - zmieniamy nazwę głównej gałęzi na main
git remote add origin https://link-do.repo/tu/wklejamy # podłączamy remote(zdalne repo)
git push -u origin main # robimy pusha
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
#### Porównywanie:
```bash
git diff branch1..branch2
```

#### Łączenie
```bash
#robimy checkout do brancha do którego chcemy zmergeować...
git merge <nazwa brancha z ktorego mergeujemy> #łączymy branche
```
#### Rebase
```bash
# Najpierw robimy checkout na brancha, którego rebase chcemy zrobić
git rebase
```
**Ale ale**, czym w ogóle jest rebase?
Otóż jak mamy jakąś gałąź na przykład dupa i ona ma jakieś commity (A, B).
```
dupa -- A -- B
```
I robimy sobie brancha po commicie(będę kreatywny, nazwę go dupa2) B i na nim kilka commitów (C i D). Ale ktoś robi sobie commity na tym głównym branchu albo mergeuje do niego commity z innego brancha (E i F).
```
dupa -- A -- B -- E -- F
             \
         dupa2 \-- C -- D
```
No i się nam robi takie drzewko. I chcemy sobie dalej dokładać commity do dupa2, ale chcemy to robić na aktualnym kodzie z głównego brancha. Więc robimy `git rebase dupa` na branchu dupa2 i wtedy bam:
```
dupa -- A -- B -- E -- F
                        \
                    dupa2 \-- C -- D
```
Nasze commity przesuwają się na wierzch i wsystko jest tak, jakbyśmy robili nasze zmiany na dupa2 już po commicie F

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

### Użytkownik:
#### Ustawianie nazwy użytkownika i email:
```bash
# w folderze z repo
git config user.name "Twoje Imię"
git config user.email "twoj@email.com"

# jeżeli chcemy to zrobić dla wszystkich repo na urządzeniu 
# dodajemy parametr --global np.:
git config --global user.name "Twoje Imię"
```


## Filozofia GITa

Legendy głoszą, że tu kiedyś coś będzie