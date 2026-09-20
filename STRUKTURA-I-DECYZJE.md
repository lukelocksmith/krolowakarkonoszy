# Królowa Karkonoszy, struktura strony i podjęte decyzje

Dokument dla grafika i osób pracujących nad kolejną iteracją strony. Opisuje, co zostało zmienione względem poprzedniej wersji, dlaczego, i co jeszcze wymaga uzupełnienia.

Stan na: 2026-09-20, żywa strona: https://krolowa.important.is

---

## 1. Punkt wyjścia i problem

Klient (przez Marcina) zgłosił trzy zastrzeżenia do wcześniejszej wersji strony:

1. Strona jest za długa pod względem ilości informacji.
2. Za dużo miejsca zajmuje historia obiektu, na jednej stronie w środku serwisu.
3. Sekcja Grupy jest za daleko. Klientowi zależy na podkreśleniu, że obiekt jest nastawiony na grupy zorganizowane: sportowe, szkolne, szachowe, seniorów.

Pomiar starej struktury potwierdził te zastrzeżenia liczbowo:

- Historia stanowiła 36% całego tekstu strony i jedną trzecią mediów (dwa filmy, dwa tła po 4 MB).
- Sekcja Grupy zaczynała się na 69,8% długości strony, czyli trzeba było przewinąć prawie do końca.
- Gotowy komponent z kluczowymi faktami (liczba pokoi, wyżywienie, parking, lokalizacja) był napisany w kodzie, ale nigdy nie włączony.

## 2. Co zostało zmienione w strukturze

Nowa kolejność sekcji na stronie każdego obiektu (Szklarska Poręba i Karpacz mają identyczny układ):

1. Hero, z dwoma przyciskami akcji: „Zapytaj o termin" i „Przyjazd grupy zorganizowanej"
2. Pasek faktów (liczba pokoi, wyżywienie, parking, lokalizacja), wcześniej nieużywany
3. Grupy, pełna sekcja z trzema typami (sportowe, wyjazdy zorganizowane, wsparcie organizatora), zapleczem dla grup i banerem z zapytaniem ofertowym
4. Noclegi, wybór Standard/Premium
5. Wyżywienie
6. Zdjęcie pełnoekranowe
7. Galeria
8. Wyróżniki (4 kafelki)
9. Historia, skrócona do jednego akapitu z linkiem na osobną podstronę
10. Wnętrza i materiały
11. Lokalizacja
12. Okolica aktywna (Jakuszyce)
13. Opinie
14. FAQ
15. Kontakt

Efekt: Grupy zaczynają się teraz na 6,4% długości strony zamiast 69,8%. Historia zeszła z 36% tekstu do jednego akapitu plus link.

### Historia jako osobna podstrona

Pełna historia obiektu (sześć rozdziałów, materiały archiwalne, dwa filmy) przeniesiona na osobne strony:

- `historia.html`, dla Szklarskiej Poręby
- `historia-karpacz.html`, dla Karpacza

Na stronie głównej obiektu został jeden akapit i link „Poznaj historię obiektu". Podstrona ma własny nav i sekcję zamykającą z linkiem powrotnym do strony obiektu.

### Odlinkowana podstrona dla obozów dzieci

W trakcie prac znaleziono gotową, w pełni wypełnioną treścią podstronę `grupy-obozy-dzieci.html`, przygotowaną wcześniej, ale nigdy nie podłączoną (link był ukryty w kodzie z komentarzem „ukryty na start"). Ma własne hero, sekcję „Dlaczego u nas", ofertę, galerię i CTA kontaktowe. Komentarz w kodzie wskazuje, że to szablon do skopiowania pod inne typy grup, na przykład konferencje albo seniorzy.

Karty w sekcji Grupy na stronie Szklarskiej Poręby prowadzą teraz do tej podstrony. Wersja dla Karpacza nie istnieje, więc karty na stronie Karpacza pozostały bez linku.

## 3. Treść, która się zmieniła

- Opisy typów grup rozszerzone o konkretne przykłady: turnieje szachowe, warsztaty, grupy seniorów, zawody (wcześniej ogólnikowe „szkoły, firmy, turnieje, wycieczki").
- Dodana informacja o parkingu premium w przygotowaniu, w trzech miejscach: pasek faktów, karta zaplecza dla grup, FAQ.
- Dodany baner z zapytaniem ofertowym na końcu sekcji Grupy (mailto z tematem dopasowanym do obiektu, plus telefon).
- FAQ: dodane pytanie o strefę relaksu (zdjęta osobna sekcja SPA, informacja przeniesiona tutaj).

## 4. Poprawki dostępności (WCAG)

Do wiadomości dla grafika, bo dotyczą kolorów i rozmiarów elementów UI:

- Kolor etykiet sekcji (małe wersaliki typu „GRUPY", „NOCLEGI") na jasnym tle zmieniony z `#AD9575` na nowy token `#7d6b54` (`--beige-ink`). Stary kolor miał kontrast 2,54:1 wobec kremowego tła, nowy ma 4,55:1, czyli przechodzi próg WCAG AA. Na ciemnozielonych sekcjach kolor etykiet bez zmian.
- Przełącznik języka (PL/EN/DE/CZ) w nawigacji powiększony z 36×36 px do 44×44 px, zgodnie z minimalnym rozmiarem pola dotykowego.
- Dodany jeden spójny styl `:focus-visible` w całym serwisie (wcześniej istniał tylko punktowo).
- Dwa filmy w sekcji Historii (autoodtwarzane, zapętlone) teraz zatrzymują się, jeśli użytkownik ma w systemie włączone „ogranicz ruch" (`prefers-reduced-motion`).
- Zdjęcia w galeriach pokoi (Standard, Premium) miały puste opisy (`alt=""`). Teraz mają opisowy tekst, na przykład „Pokoje Standard, zdjęcie 3 z 12", zarówno w miniaturach jak i w podglądzie na powiększeniu.
- Dodany `<main>` i link „Przejdź do treści” dla użytkowników klawiatury i czytników ekranu.

## 5. Struktura plików

Strona to statyczny HTML z React ładowanym z CDN (bez builda), więc każda podstrona to osobny, samodzielny plik.

```
/                              strona rozdroża, wybór obiektu
/szklarska-poreba.html         strona obiektu Szklarska Poręba
/karpacz.html                  strona obiektu Karpacz
/historia.html                 pełna historia, Szklarska Poręba
/historia-karpacz.html         pełna historia, Karpacz
/grupy-obozy-dzieci.html       podstrona dla obozów dzieci (Szklarska)
/krolowa-karkonoszy/           wszystkie zdjęcia, wideo, ikony SVG, shared.css
```

Ważne dla grafika: nowe zdjęcia i grafiki trafiają do folderu `krolowa-karkonoszy/`, niezależnie od tego, której strony dotyczą. Strony HTML odwołują się do plików ścieżką bezwzględną, na przykład `/krolowa-karkonoszy/nazwa-pliku.jpg`.

## 6. Czego brakuje, do uzupełnienia

To jest lista otwarta, nie błędy techniczne, tylko brakująca treść lub materiały:

- **Karpacz, cały obiekt.** Hero, wyróżniki, historia to placeholdery z tekstem „do uzupełnienia". Strona rozdroża ma opis karty Karpacza jako „Opis lokalizacji, do uzupełnienia”. Adres obiektu w Karpaczu nieznany.
- **Zdjęcia w podstronie obozów.** Sześć zdjęć w galerii to surowe pliki z telefonu (`IMG_9118.JPG` i podobne, jedno waży 4 MB), bez opisów, wygląda na tymczasowe wypełnienie szablonu. Do wybrania i skompresowania właściwych zdjęć.
- **Sprzeczne odległości do dworca PKP.** W trzech miejscach są trzy różne liczby: „5 minut od PKP”, „700 metrów”, „około kilometra”. Do ustalenia jedna wartość ze źródła.
- **Brak liczb w głównej sekcji Grupy.** Podstrona obozów ma konkretne dane (96 pokoi, podział 50+46, typy pokoi dla grup z opiekunami), ale te liczby nie trafiły do sekcji Grupy na stronie głównej obiektu, gdzie organizator też ich szuka.
- **Brak widełek cenowych.** Nigdzie na stronie nie ma nawet orientacyjnej ceny za dobę ani informacji o wycenie dla grup.
- **Mapa w sekcji Kontakt** to osadzona mapa Google, na której widać nazwy sąsiednich obiektów konkurencji (Radisson, Pensjonat Magdalena, Hotel Dom na Białej Dolinie).

## 7. Czego świadomie nie ruszano

- Nie zmieniano stylu wizualnego, kolorystyki poza jednym tokenem kontrastu, ani układu istniejących komponentów. Prace dotyczyły wyłącznie kolejności sekcji, treści i dostępności.
- Zdjęcia pokoi powtarzają się w trzech miejscach (galeria pokoju, galeria główna, filtr galerii), to nadal nieużyty potencjał do skrócenia strony, zostawione do decyzji.
- Sekcja SPA usunięta z widoku strony (kod został w plikach), informacja o strefie relaksu w przygotowaniu przeniesiona do FAQ.
