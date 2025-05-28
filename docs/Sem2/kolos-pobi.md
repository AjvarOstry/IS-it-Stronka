# Moje notateczki do kolosa z PObi

## Skróty klawiszowe

- `alt + shift + lewy myszy` - ustawia kursor w wielu miejscach na raz
- `ctrl + shift + D` - generuje definicję metody
- `alt + insert` - generowanie metod
- `shift + F6` - zmienia nazwę metody wszędzie gdzie występuje
- `ctrl + /` - komentuj / odkomentuj wiele linii na raz

### Pamiętajki 

- zakomentuj log_all_errors w testach nauczyciela
- includuj typedefs tam, gdzie enumy są
- ogólnie lepiej dać za dużo includów(na przykład wszystkie) jak za mało
-patrz sobie na testy nauczyciela, tam są wszystkie konstruktory itp


## Typedefs

W typedefs.h piszemy sobie:
- wszystkie enumy
- wszystkie klasy
- wszystkie shared_ptr ??? - tak w razie co podobno?

## Klasa
#### Obsługa vecotora
```c++
//.h
    private:
        std::pmr::vector<IstotaPtr> istoty;
    public: 
        void zaladuj(const IstotaPtr &istota);
        void wyladuj(const IstotaPtr &istota);

//.cpp
    void Arka::zaladuj(const IstotaPtr &istota)
    {
        istoty.push_back(istota);
    }
    void Arka::wyladuj(const IstotaPtr &istota)
    {
        istoty.erase(std::remove(istoty.begin(), istoty.end(), istota), istoty.end());
    }
```

## Klasa abstrakcyjna

#### Destruktor

Trzeba dopisać do tego genrowanego. Powinien wyglądać tak:
```c++
    virtual ~Spacecraft() = 0;
```

## Klasa dziedziczona

Przy definicji pokazujemy po czym dziedziczy:
```c++
    class Child : public Parent {

    }
```

#### Konstruktor w .cpp


**Pytańska**

- gdzie gettey - w wirtualnej, czy pochodnych?
- 



