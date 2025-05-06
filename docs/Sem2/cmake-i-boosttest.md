
# Obsługa Cmake i BoostTest

## Cmake :gear:

### Budowanie, kompilacja, uruchamianie i testowanie programu
```bash
cmake -G "Unix Makefiles" ..
```
Czyta cmakelists i tworzy zestaw plików potrzebny do kompilacji (wpisujemy, kiedy zmieniliśmy coś w cmakelistach, dodaliśmy plik lub bibliotekę w kodzie) Ta komenda uruchamiona jest w katalogu build, jako argument(tu ..) podajemy katalog główny projektu

```bash 
make #w build żeby zbudować projekt
make -j #to samo, tylko leci wszystkimi wątkami porcesora
```

Kiedy chcemy uruchomić zbudowany projekt w katalogu `build` znajdujemy `program/Program` i uruchamiamy jak skrypt bash

```bash
ctest --verbose
```
A tak uruchamiamy testy, też w katalogu `build`

### CMakeLists

Biblioteki dodajemy tutaj:

```bash
    set(SOURCE_FILES
    #tu wrzucamy ścieżki do kolejnych bibliotek
    #clion zwykle robi to autoamtycznie
    )
```