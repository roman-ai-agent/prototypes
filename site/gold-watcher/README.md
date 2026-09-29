# Gold Watcher — prototyp r2.3

Klikalny kontrakt designu i zachowania, a nie kod aplikacji. Baza: zaakceptowany r2.2. Wygląd UI jest bez zmian. Historia zmian jest w `changelog.md`.

## Uruchomienie
Prototyp trzeba otworzyć przez lokalny serwer HTTP, bo `index.html` wczytuje `prototype-manifest.json` i `assets/fixtures.json`. Przykład:
```
cd prototype && python3 -m http.server 8000
```
Potem otwórz `http://localhost:8000/index.html`. Otwarcie przez `file://` kończy się błędem `fixtures-unavailable`.

## Tryby
- **capture:** `index.html?mode=capture&state=<state_id>&target=<target_id>`.
  - Renderuje wyłącznie `element.product-work-surface` w rozmiarze targetu, 1:1, w pozycji 0,0.
  - Gotowość: `data-prototype-ready="true"` na anchorze.
  - Błąd: `[data-prototype-error]` z jednym z kodów: `missing-parameter`, `unknown-state`, `unknown-target`, `state-unavailable-for-target`, `fixtures-unavailable`, `render-invalid`.
- **user_preview:** `index.html?mode=user_preview&state=<state_id>&target=<target_id>`.
  - Bez parametrów otwiera `gold-overview.ready` na `mac-wide`.
  - Dolny panel narzędzi: link do katalogu, EKRAN / STAN (lista, ‹ ›, ← / →) i wybór targetu.
  - Preferencje zapisuje w `localStorage` pod kluczem `gold-watcher.r1.prefs`, wyłącznie w tym trybie.

## Stany i targety
Źródłem listy jest `prototype-manifest.json` (`schema_version: 3`).
- **Targety:** `mac-wide` 1440×932, `iphone-16` 393×852, `pixel-8` 412×915, `galaxy-s20` 360×800, `iphone-16-pro-max` 440×956.
- **Stany:** 16, w tym 6 stanów `*-housing-details-expanded` dozwolonych wyłącznie na 4 telefonach.

## Pliki
- `index.html`: prototyp.
- `catalog.html`: katalog komponentów.
- `assets/fixtures.json`: jedyne źródło danych. `shared_series` zawiera wspólne serie, a `fixtures` jest rejestrem fixture'ów wskazywanych przez `states[].fixture_id`.
- `prototype-manifest.json`: indeks stanów i targetów.
- `responsive-spec.md`: reguły responsywności dla człowieka.
- `changelog.md`: historia zmian.
- `checksums.sha256`: SHA-256 pozostałych 7 plików.

## Publiczne atrybuty DOM
`data-screen-id`, `data-state-id`, `data-element-id`, `data-component-id`, `data-interaction-id`, `data-variant`. Narzędzia `user_preview` mają `data-prototype-tool` i nie istnieją w `mode=capture`.

## Raport kontroli
Kontrola eksportu z 2026-09-29. Wynik: **pozytywny**.
- **Archiwum i checksumy — OK.** ZIP zawiera wyłącznie katalog `prototype/` z 8 plikami. `checksums.sha256` pokrywa 7 pozostałych plików i wszystkie sumy się zgadzają.
- **Manifest — OK.** Poprawny JSON zawierający wyłącznie pola schematu 3: 5 targetów i 16 stanów. Każdy `fixture_id` istnieje w `assets/fixtures.json → fixtures`, a każda seria wskazana przez fixture istnieje w `shared_series`.
- **Zasoby lokalne — OK.** Poza `prototype-manifest.json` i `assets/fixtures.json` (ten sam katalog, przez HTTP) nie ma zależności sieciowych. Dane nie są osadzone w HTML.
- **Smoke test — OK.**
  - **Capture — reprezentatywny zestaw 30 par**, obejmujący wszystkie 5 fixture'ów, wszystkie 5 targetów, stany rozwinięte i stany alertu:
    - `ready`, `loading`, `refreshing` i `stale` na wszystkich 5 targetach;
    - `source-error` (pixel-8, mac-wide), `unavailable` (iphone-16-pro-max, mac-wide), `empty-range` (galaxy-s20) i `intraday-rebound` (mac-wide);
    - `intraday-rebound-housing-details-expanded` (pixel-8), `stale-housing-details-expanded` (galaxy-s20) i `alert-settings-editing` (mac-wide).

    W każdej parze jest jeden anchor w rozmiarze targetu w pozycji 0,0, jeden korzeń stanu i brak narzędzi. Znormalizowany DOM powierzchni jest identyczny z r2.2; dla stanów rozwiniętych porównanie dotyczy r2.2 po kliknięciu „Szczegóły obliczenia”.
  - **Para niedozwolona:** `ready-housing-details-expanded` × mac-wide kończy się błędem `state-unavailable-for-target`.
  - **Błędne parametry:** brak `state` lub `target` daje `missing-parameter`, a nieznane wartości — `unknown-state` / `unknown-target`.
  - **Interakcja:** kliknięcie `interaction.pp-details-toggle` na iphone-16 zmienia `data-state-id` na `gold-overview.ready-housing-details-expanded`, a DOM jest identyczny z bezpośrednim otwarciem tego stanu.
  - **user_preview:** domyślnie `ready` na `mac-wide`, a przełączanie targetu i szczegółów działa.
- **Konsola i układ — OK.** Poza zamierzonymi błędami dla niedozwolonych par i błędnych parametrów nie ma błędów konsoli, a automatyczny przegląd strony zakończył się bez zgłoszeń.
