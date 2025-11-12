# DTD
## Co to w ogóle jest?

DTD (Dokument Type Definition) - definiuje strukturę xmla, którego piszemy. Mówi parserowi jakie elementy i atrybuty mogą/muszą występować i jak wygląda to "drzewo" atrybutów, czyli które elementy są wewnątrz których.

## Jak to wygląda?

Dokument dtd załączamy w xml w następujący sposób:

```xml
<!DOCTYPE nazwa_elementu_glownego SYSTEM "nazwa_pliku_dtd.dtd">
```

## Dodawanie elementów

Jeżeli element zawiera tylko dane, dodajemy tak:
```dtd
<!ELEMENT nazwa_elementu (#PCDATA)>
```

Jeżeli mamy inne elementy w środku, dodajemy tak:

```dtd
<!ELEMENT nazwa_elementu (podelement1, podelement2*, podelement3+)>
```

W przypadku elementów pustych np `<element/>` robimy coś takiego:
```dtd
<!ELEMENT element EMPTY>
```

### Separatory:
- `,` np. `<!ELEMENT nazwa_elementu (podelement1?, podelement2>` - podelementy muszą wystąpić w podanej kolejności
- `|` np. `<!ELEMENT nazwa_elementu (podelement1?,  podelement2, | podelement3+>` - lub, czyli wystąpią dwa pierwsze podelementy lub ostatni

### Określenie liczności elementów

- `podelement+` - przynajmniej jeden taki musi być w środku
- `track*` - zero lub więcej takich elementów musi być w środku
- `lastModified?` - element opcjonalny, ale może być max 1
- `podelement` - element musi wystąpić dokładnie raz

**Uwaga!** Jeżeli znaczek jest za całą grupą elementów `(a, b)+`. To znaczy, że w xml powtarza się to a i b **jako cały zestaw**

## Dodawanie atrybutów

```dtd
<!ELEMENT nazwa_elementu (#PCDATA)>
<!ATTLIST nazwa_elementu nazwa_atrybutu ID #REQUIRED>
```

I teraz mamy różne dostępne **typy atrybutów**:
- `CDATA` - dowolny tekst
- `ID` - id unikalne w całym dokumencie
- `IDREF` - lista odwołań do id
- `ENTITY` - odwołanie to zdefiniowanej encji obrazka, pliku itp
- `ENTITIES` - lista encji
- `NMTOKEN` - Pojedynczy token (bez spacji, czyli: litery / liczby / _ / -)
- `NMTOKENS` - lista tokenów

I różne **reguły obowiązkowości**:
- `#REQUIRED` - atrybut wymagany
- `#IMPLIED` - opcjonalny
- `#FIXED "wartość"` - ma stałą, niezmienialną wartość
- brak - zwykle działa jak `#IMPLIED`, ale lepiej pisać to `#IMPLIED`

Można sobie grupować te atrybuty. Np.:

```dtd
<!ATTLIST nazwa_elementu nazwa_atrybutu ID #REQUIRED
                         nazwa_atrybutu2 CDATA #IMPLIED
>
```

