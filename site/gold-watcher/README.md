# Gold Watcher — prototyp r2.4

Klikalny kontrakt designu i zachowania, a nie kod aplikacji. Baza: r2.3. Zmiany: `mac-wide` 1440×900 z zagęszczonym układem wide, bez przewijania poza `source-error`, oraz kraje Polska, Finlandia i Francja. Telefony bez zmian. Historia zmian jest w `changelog.md`.

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
- **Targety:** `mac-wide` 1440×900, `iphone-16` 393×852, `pixel-8` 412×915, `galaxy-s20` 360×800, `iphone-16-pro-max` 440×956.
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
Kontrola eksportu z 2026-09-30. Wynik: **pozytywny**.
- **Archiwum i checksumy — OK.** ZIP zawiera wyłącznie katalog `prototype/` z 8 plikami. `checksums.sha256` pokrywa 7 pozostałych plików i wszystkie sumy się zgadzają.
- **Manifest — OK.** `schema_version: 3`, `revision_id: "r2.4"` i `id_migrations: []`. `mac-wide` ma 1440×900, 16 stanów, a każdy `fixture_id` i każda wskazana seria istnieją w `assets/fixtures.json`.
- **mac-wide 1440×900 — OK.** W `mode=capture` zmierzono wysokość treści we wszystkich 10 stanach dozwolonych na `mac-wide`:
  - 9 stanów mieści się w 900 bez przewijania; najwyższa kolumna kończy się na 864;
  - `source-error` ma 950: baner u góry, przewijanie jest dopuszczalne.
  - Powierzchnia ma 1440×900, bez powielonych `data-element-id` / `data-interaction-id`.
- **Telefony bez zmian — OK.** Znormalizowany DOM powierzchni jest identyczny z r2.3 dla `ready` (iphone-16), `stale-housing-details-expanded` (galaxy-s20), `alert-settings-editing` (pixel-8), `empty-range` (iphone-16-pro-max) i `source-error` (iphone-16).
- **Kraje — OK.**
  - Selector na mac-wide i lista na telefonie pokazują wyłącznie: Polska · Warszawa · PLN, Finlandia · Helsinki · EUR, Francja · Paryż · EUR.
  - Po przełączeniu na FI i FR karty i wykres siły nabywczej przeliczają się bez wartości NaN czy undefined, a kurs pokazuje 1 USD = 0,8600 EUR.
  - W prototypie, danych i katalogu nie ma USA ani Belgii.
- **Błędne parametry — OK.** `missing-parameter`, `unknown-state`, `unknown-target` i `state-unavailable-for-target` działają jak w r2.3.
- **user_preview — OK.** Domyślnie `ready` na `mac-wide`, a karta targetu pokazuje 1440×900.
- **Konsola i układ — OK.** Poza zamierzonymi błędami dla błędnych parametrów nie ma błędów konsoli, a automatyczny przegląd strony zakończył się bez zgłoszeń.
