# Standardy kodu

## Spis treści

1. [Nazewnictwo](#1-nazewnictwo)
2. [Komentarze](#2-komentarze)
3. [Struktura projektu](#3-struktura-projektu)
4. [Zasady pisania kodu](#4-zasady-pisania-kodu)
5. [Narzędzia i automatyzacja](#5-narzędzia-i-automatyzacja)
6. [Git i code review](#6-git-i-code-review)

---

## 1. Nazewnictwo

### 1.1 Konwencje wielkości liter

| Element | Konwencja |
|---------|-----------|
| Zmienne, parametry | `camelCase` |
| Funkcje, metody | `camelCase` (czasownik) |
| Klasy | `PascalCase` (rzeczownik) |
| Interfejsy | `PascalCase`, **bez** prefiksu `I` |
| Typy (`type`) | `PascalCase` |
| Enumy i ich wartości | `PascalCase` |
| Parametry generyczne | `T` lub opisowa nazwa z prefiksem `T` |
| Stałe (na poziomie modułu) | `UPPER_SNAKE_CASE` |
| Komponenty React | `PascalCase` |
| Hooki React | `camelCase` z prefiksem `use` |
| Pliki (ogólne) | `kebab-case` |
| Pliki komponentów React | `PascalCase` |
| Foldery | `kebab-case` |

### 1.2 Reguły szczegółowe

- **Zmienne boolowskie** zaczynamy od `is`, `has`, `can`, `should`.
- **Funkcje** to czasowniki opisujące akcję.
- **Kolekcje** nazywamy w liczbie mnogiej, bez dopisków `List`/`Array`.
- **Stałe** zamiast magic numbers i magic stringów.
- **Prefiksy i sufiksy** typu `I` w interfejsach czy `Type`/`Enum` w nazwach są zbędne.
- **Handlery zdarzeń:** `handleXxx` dla funkcji obsługujących, `onXxx` dla propsów.

### 1.3 Typy i `any`

- **Zakaz `any`.** Jeśli typ jest naprawdę nieznany, użyj `unknown` i zawęź go.
- Tryb `strict` w `tsconfig.json` jest obowiązkowy.
- Funkcje publiczne i eksportowane mają jawnie opisane typy zwracane.
- `interface` dla kształtu obiektów i kontraktów, `type` dla unii, przecięć i aliasów.
- Unikamy asercji typów (`as`), chyba że mamy pewność i wyjaśniamy to komentarzem.

---

## 2. Komentarze

### 2.1 Filozofia

Dobry kod tłumaczy **co** robi sam, przez nazwy i strukturę. Komentarz służy do wyjaśnienia **dlaczego**. Jeśli musisz komentować **co** robi fragment kodu, najpierw popraw nazwy lub wydziel funkcję.

### 2.2 Kiedy komentować

- Nieoczywista decyzja biznesowa lub techniczna (dlaczego tak, a nie inaczej).
- Obejścia (workaroundy) błędów bibliotek lub API, z linkiem do issue.
- Skomplikowane algorytmy lub wyrażenia regularne.
- Ostrzeżenia o skutkach ubocznych lub ograniczeniach.

### 2.3 Kiedy NIE komentować

- Gdy komentarz powtarza to, co widać w kodzie.
- Zamiast usunąć martwy kod. **Zakomentowany kod usuwamy**, historia jest w gitcie.
- Do prowadzenia dziennika zmian w pliku (od tego jest `git log`).

### 2.4 JSDoc / TSDoc

- Stosujemy dla **publicznych API, bibliotek i eksportowanych funkcji**.
- Typów nie powtarzamy w opisie, bo TypeScript już je zna.
- Opisujemy: co robi funkcja, parametry (`@param`), wynik (`@returns`), rzucane błędy (`@throws`).

### 2.5 TODO / FIXME

- Zawsze z autorem i kontekstem (numer ticketu lub powód), w formacie `TODO(autor): opis, ticket #123`.
- TODO bez kontekstu nie przechodzi code review.

---

## 3. Struktura projektu

### 3.1 Zasady

- Organizujemy kod **według funkcjonalności (feature-based)**, a nie według typu pliku.
- Jeden plik = jedna odpowiedzialność. Plik powyżej ~300 linii to sygnał do podziału.
- Kod współdzielony trafia do `shared/`, kod specyficzny dla funkcjonalności zostaje w jej folderze.
- Warstwy się nie mieszają: logika biznesowa nie siedzi w komponentach UI ani w kontrolerach HTTP.
- Zależności płyną w jedną stronę: `features` → `shared`, nigdy odwrotnie. Funkcjonalności nie importują się nawzajem bez wyraźnej potrzeby.

### 3.2 Układ katalogów

```
src/
├── app/                    # inicjalizacja aplikacji, routing, providery
├── features/
│   └── <feature-name>/
│       ├── components/     # komponenty UI tej funkcjonalności
│       ├── hooks/          # hooki specyficzne dla funkcjonalności
│       ├── services/       # logika biznesowa, wywołania API
│       ├── types/          # typy i interfejsy
│       ├── utils/          # funkcje pomocnicze tylko dla tej funkcjonalności
│       └── index.ts        # publiczne API modułu
├── shared/
│   ├── components/         # wspólne komponenty UI
│   ├── hooks/              # wspólne hooki
│   ├── utils/              # wspólne funkcje pomocnicze
│   ├── types/              # wspólne typy
│   └── constants/          # wspólne stałe
├── config/                 # konfiguracja, zmienne środowiskowe
└── main.ts
```

### 3.3 Importy

- Każdy moduł udostępnia publiczne API przez `index.ts`. Importujemy z `index.ts`, nie z wnętrza modułu.
- Używamy aliasów ścieżek (`@/features/...`) zamiast długich ścieżek względnych.
- Kolejność importów (egzekwowana lintem):
  1. Biblioteki zewnętrzne
  2. Moduły z aliasów (`@/...`)
  3. Importy względne (`./`, `../`)
  4. Style i zasoby
- Zakaz cyklicznych zależności między modułami.
- Preferujemy **named exports** zamiast `export default` (łatwiejszy refactoring i wyszukiwanie).

### 3.4 Pliki testowe

Testy leżą obok testowanego kodu, z sufiksem `.test.ts`.

---

## 4. Zasady pisania kodu

- **Jedna funkcja = jedna rzecz.** Jeśli opisujesz ją słowem "i", rozbij ją.
- Funkcje krótkie (orientacyjnie do ~30 linii), maksymalnie 3-4 parametry (więcej: przekaż obiekt).
- **Early return** zamiast głębokich zagnieżdżeń `if`.
- `const` domyślnie, `let` tylko gdy trzeba, `var` nigdy.
- Preferujemy niemutowalność i czyste funkcje.
- Zawsze `===` zamiast `==`.
- Błędy obsługujemy jawnie. Nie połykamy wyjątków (pusty `catch` jest zabroniony).
- DRY, ale bez przedwczesnej abstrakcji: trzy podobne miejsca to sygnał do wydzielenia wspólnego kodu, dwa jeszcze nie.
- Zakaz `console.log` w kodzie produkcyjnym. Używamy loggera.

---


## 5. Git i code review

### 5.1 Branche

Format: `typ/krotki-opis`, gdzie typ to `feature`, `fix`, `refactor`, `docs`, `chore`.

### 5.2 Commity (Conventional Commits)

- Format: `typ: krótki opis w trybie rozkazującym`.
- Dozwolone typy: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`.
