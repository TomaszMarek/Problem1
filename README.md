# Automatyzacja pozyskiwania i weryfikacji danych sportowych

**Python · automatyzacja · kontrola jakości danych · raportowanie**

> **Publiczne studium przypadku.** Kod źródłowy, adresy stron i szczegóły źródeł pozostają prywatne, ponieważ rozwiązanie jest wykorzystywane w innym rozwijanym projekcie. Grafiki przedstawiają schematy opracowane na potrzeby portfolio, a nie zrzuty witryny źródłowej.

## 1. Problem

Historyczne dane sportowe były rozproszone pomiędzy wieloma stronami, kategoriami rozgrywek i sezonami. Ręczne zebranie plików CSV oznaczałoby powtarzalne otwieranie stron, wyszukiwanie odnośników, zapisywanie plików i kontrolowanie, które zasoby zostały już pobrane.

![Schemat problemu: 27 stron i 727 zasobów](screenshots/screenshot1.png)

W pierwszym etapie program przeanalizował **27 stron źródłowych** i wykrył **727 unikalnych adresów plików CSV**. Wszystkie 27 stron zostało przetworzonych bez błędu skanowania. Liczby dotyczą wykrytych odnośników, a nie liczby rekordów wewnątrz plików.

## 2. Cel

Celem było stworzenie narzędzia, które samodzielnie wykrywa dostępne pliki, porządkuje informacje o nich, pobiera dane, sprawdza ich integralność i pozwala bezpiecznie wznowić przerwaną pracę — bez ponownego pobierania poprawnych plików.

## 3. Rozwiązanie

![Cztery etapy automatyzacji](screenshots/screenshot2.png)

**Etap 1 — skanowanie.** Program analizuje strony, wyszukuje odnośniki do CSV, normalizuje adresy i usuwa duplikaty.

**Etap 2 — manifest JSON.** Zidentyfikowane zasoby są opisywane metadanymi: kategorią, rozgrywkami, sezonem (jeśli można go ustalić), nazwą pliku oraz lokalizacją źródłową. Manifest jest zapisywany lokalnie. **Rzeczywiste adresy URL nie są publikowane.**

**Etap 3 — pobieranie.** Program korzysta z manifestu, zapisuje pliki w uporządkowanych katalogach, obsługuje wybrane błędy HTTP i bezpieczne przekierowania oraz zapisuje stan operacji.

**Etap 4 — weryfikacja.** Podstawowa kontrola zawartości CSV oraz suma kontrolna SHA-256 pozwalają odrzucać nieprawidłowe odpowiedzi i rozpoznawać zmienione pliki. Wyniki trafiają do raportu JSON.

## 4. Przykład struktury danych

Poniższy fragment jest **schematem**, a nie rzeczywistym manifestem ani ujawnieniem nazw plików źródłowych:

```text
data/
├── Kategoria_A/
│   └── Rozgrywki_1/
│       ├── sezon_2023_2024.csv
│       └── sezon_2022_2023.csv
└── Kategoria_B/
    └── Rozgrywki_2/
        └── sezon_2023_2024.csv
```

## 5. Testy i kontrola jakości

Do testów jednostkowych wykorzystano `pytest`. Sprawdzane scenariusze obejmują m.in.:

| Scenariusz | Oczekiwane działanie |
|---|---|
| Plik istnieje i jego suma SHA-256 jest zgodna | Pominięcie ponownego pobrania |
| Plik lokalny został zmodyfikowany | Ponowne pobranie |
| Serwer zwraca HTTP 404 | Zgłoszenie niepowodzenia |
| Zamiast CSV otrzymano HTML | Odrzucenie odpowiedzi |
| Przekierowanie prowadzi poza dozwolone źródło | Zablokowanie przekierowania |
| Przekierowanie tworzy pętlę | Wykrycie pętli |
| Nazwa zawiera polskie znaki | Zachowanie znaków Unicode |

**Rzeczywisty wynik testów:**
![](screenshots/screenshot3.png)

## 6. Rezultaty i mechanizm wznawiania

Wykryto **727 unikalnych adresów CSV**; proces pobierania został zakończony z kompletem plików według relacji z uruchomienia. Program zapisuje status każdej operacji:

- `DOWNLOADED` — pobrano plik w danym uruchomieniu;
- `SKIPPED` — istniejący plik przeszedł weryfikację integralności;
- `FAILED` — nie udało się wykonać operacji.

![Schemat raportu demonstracyjnego](screenshots/03-results.svg)

**Uwaga:** ilustracja przedstawia planowany scenariusz demonstracyjny: jeden brakujący plik zostaje pobrany, a 726 istniejących plików zostaje pominiętych. **Nie jest to jeszcze potwierdzony wynik konkretnego uruchomienia.** Po wykonaniu demonstracji należy wpisać poniżej rzeczywiste liczby:

| Wskaźnik | Rzeczywisty wynik demonstracji |
|---|---:|
| Przetworzone zasoby | _do uzupełnienia_ |
| Nowo pobrane | _do uzupełnienia_ |
| Pominięte | _do uzupełnienia_ |
| Nieudane | _do uzupełnienia_ |
| Czas wykonania | _do uzupełnienia, jeśli zmierzono_ |

## 7. Technologie

`Python` · `Requests` · `BeautifulSoup4` · `CSV` · `JSON` · `SHA-256` · `pytest`

## 8. Wnioski

Projekt pokazuje praktyczne wykorzystanie automatyzacji do ograniczenia ręcznej, powtarzalnej pracy z danymi. Kluczowe było nie tylko pobranie zasobów, ale też zapewnienie możliwości wznowienia procesu, weryfikacji plików i udokumentowania rezultatów. Tak przygotowane dane mogą być następnie wykorzystane w analizach, m.in. w Excelu lub Power Query.

---

**Zakres publikacji:** wyłącznie opis rozwiązania i neutralne schematy. Kod, rzeczywisty manifest, pobrane pliki i informacje umożliwiające identyfikację źródła nie są udostępniane.
