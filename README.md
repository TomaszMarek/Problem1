# Automated Data Acquisition & Validation

### Automatyzacja pozyskiwania i weryfikacji historycznych danych sportowych

**Studium przypadku projektu zrealizowanego w Pythonie**

## 1. Problem

Dane historyczne dotyczące rozgrywek sportowych były rozproszone pomiędzy wieloma podstronami, sezonami i kategoriami rozgrywek.

Ręczne pobieranie każdego pliku wymagałoby wielokrotnego przeglądania stron, wyszukiwania odpowiednich odnośników, zapisywania danych oraz kontrolowania, które pliki zostały już pobrane.

Celem projektu było zautomatyzowanie tego procesu i ograniczenie konieczności ręcznej obsługi.

W ramach analizy źródeł zidentyfikowano:

- **27 stron zawierających zasoby danych.**
- **727 unikalnych plików CSV.**
- Dane obejmujące wiele krajów, lig i sezonów historycznych.

Ze względu na wykorzystanie pozyskanych danych w innym, rozwijanym projekcie, szczegółowe informacje o źródłach oraz kod implementacji pozostają prywatne.

## 2. Rozpoznanie struktury danych

Pierwszym etapem było stworzenie modułu analizującego strukturę stron i identyfikującego dostępne pliki CSV.

Program automatycznie rozpoznaje odnośniki do zasobów, usuwa duplikaty i przygotowuje manifest w formacie JSON.

**Wyniki skanowania:**

| Wskaźnik | Wynik |
|---|---:|
| Przeanalizowane strony | 27 |
| Wykryte unikalne pliki CSV | 727 |
| Błędy skanowania | 0 |

*Tutaj zrzut ekranu podsumowania skanowania, bez adresów URL i nazw źródeł.*

## 3. Automatyczne pobieranie

Na podstawie przygotowanego manifestu opracowano mechanizm pobierania i organizowania plików.

Najważniejsze funkcjonalności:

- Automatyczne pobieranie plików CSV.
- Organizowanie danych według kategorii i okresów.
- Pomijanie wcześniej pobranych plików.
- Wykrywanie uszkodzonych lub zmodyfikowanych plików za pomocą SHA-256.
- Ponawianie wybranych nieudanych operacji.
- Obsługa bezpiecznych przekierowań HTTP.
- Zapisywanie stanu pobierania i raportów JSON.

## 4. Testowanie

Poprawność wybranych funkcjonalności sprawdzono za pomocą automatycznych testów jednostkowych w bibliotece `pytest`.

Testowane scenariusze obejmowały m.in. ponowne pobieranie uszkodzonego pliku, pomijanie prawidłowych plików, obsługę błędów HTTP, kontrolę przekierowań oraz zachowanie znaków Unicode.

*Tutaj zrzut ekranu z rzeczywistym wynikiem testów.*

## 5. Rezultaty

Projekt umożliwił zautomatyzowanie procesu pozyskiwania archiwalnych danych sportowych i przygotowanie ich do dalszego przetwarzania.

Program rozróżnia trzy rodzaje operacji:

- `DOWNLOADED` — plik pobrany podczas bieżącego uruchomienia.
- `SKIPPED` — plik istnieje i przeszedł weryfikację integralności.
- `FAILED` — operacja zakończona niepowodzeniem.

### Przykład działania mechanizmu uzupełniania danych

*W tym miejscu zostanie umieszczony rzeczywisty wynik uruchomienia programu z jednym brakującym plikiem.*

| Operacja | Liczba |
|---|---:|
| Przetworzone zasoby | 727 |
| Nowe pobrania | Do uzupełnienia |
| Pominięte pliki | Do uzupełnienia |
| Błędy | Do uzupełnienia |
| Czas wykonania | Do uzupełnienia |

*Tutaj zrzut ekranu raportu końcowego, bez nazw źródeł i adresów.*

## 6. Technologie

Python · Requests · BeautifulSoup · JSON · CSV · SHA-256 · Pytest

## 7. Czego dotyczył projekt?

Realizacja pozwoliła wykorzystać w praktyce zagadnienia związane z automatyzacją procesów, pozyskiwaniem danych, walidacją, obsługą błędów, testowaniem oraz raportowaniem wyników.

Kod źródłowy i szczegółowe informacje o źródłach danych nie są udostępniane publicznie, ponieważ stanowią część rozwijanego prywatnie rozwiązania.
