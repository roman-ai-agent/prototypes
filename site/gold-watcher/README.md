# Gold Watcher — prototyp r2.1 (base: r2.0)

Klikalny kontrakt designu i zachowania, a nie kod aplikacji. r2.0 to kanoniczna migracja zaakceptowanego r1.3. r2.1 zmienia wyłącznie narzędzie `user_preview` (jeden dolny panel, skala 1:1). Powierzchnia produktu, stany, fixture'y, publiczne ID i `mode=capture` są bez zmian.

## Uruchomienie lokalne
Rozpakuj ZIP i otwórz `prototype/index.html` w przeglądarce, bezpośrednio (`file://`) albo przez dowolny lokalny serwer HTTP. Pliki są samowystarczalne i nie potrzebują sieci.

## Tryby
- **capture:** `index.html?mode=capture&state={state_id}&target={target_id}`. Renderuje wyłącznie `element.product-work-surface`, w skali 1:1, w lewym górnym rogu, bez rogów, cienia, widocznego paska przewijania, narzędzi i `preview-profile`. Nie korzysta z zapisanych preferencji; startuje z PL · 360 dni · USD/oz · próg 5%. Gotowość sygnalizuje `[data-element-id="element.product-work-surface"][data-prototype-ready="true"]`. Capture obejmuje widoczny viewport od góry, a nie całą przewijaną treść.
- **user_preview:** `index.html?mode=user_preview` albo `index.html`. Powierzchnia produktu ma zawsze rozmiar 1:1, równy `logical_size` targetu; gdy okno jest za małe, stronę się przewija. Powierzchnia jest wyśrodkowana w przewijanym obszarze (promień 0, obrys 1 px). Na dole okna, na pełnej szerokości, leży jeden ciemny panel narzędzi (`data-prototype-tool="user-preview-panel"`). Zawiera link „Katalog komponentów”, „EKRAN / STAN” (lista, przyciski ‹ i ›, licznik, ← / → na klawiaturze) oraz `element.preview-profile` (5 kart targetów z wymiarami). Fixture wynika ze stanu. Preferencje (kraj, zakres, jednostka, próg) są zapisywane w `localStorage` pod kluczem `gold-watcher.r1.prefs`.

## Stany (`screen_id: gold-overview`)
- `gold-overview.ready` (fixture `default`)
- `gold-overview.loading` (fixture `default`)
- `gold-overview.refreshing` (fixture `default`)
- `gold-overview.stale` (fixture `stale`)
- `gold-overview.source-error` (fixture `stale`)
- `gold-overview.unavailable` (fixture `no-gold`)
- `gold-overview.empty-range` (fixture `short-housing-history`)
- `gold-overview.intraday-rebound` (fixture `intraday-rebound`)
- `gold-overview.alert-settings-editing` (fixture `default`)
- `gold-overview.alert-settings-saved` (fixture `default`)

## Targety
Selector `capture_surface`: `[data-element-id="element.product-work-surface"]`. `logical_size` jest równy wymiarom targetu.

| target_id | wymiary | layout |
|---|---|---|
| mac-wide | 1440×932 | wide |
| iphone-16 | 393×852 | mobile |
| pixel-8 | 412×915 | mobile |
| galaxy-s20 | 360×800 | mobile+compact |
| iphone-16-pro-max | 440×956 | mobile |

## Pliki
- `prototype-manifest.json`: kanoniczny manifest (15 sekcji).
- `catalog.html`: katalog komponentów.
- `responsive-spec.md`: geometria, kolejność, widoczność, wariant i scroll dla 5 targetów.
- `assets/fixtures.json`: dane fixture'ów.
- `changelog.md`: r2.0 → r2.1, r1.3 → r2.0 i migracje.
- `checksums.sha256`: SHA-256 pozostałych 7 plików.

## Publiczne atrybuty
- `data-screen-id` i `data-state-id` oznaczają publiczny korzeń stanu:
  - w stanach alertu jest nim `element.product-work-surface`;
  - w pozostałych stanach jest nim `main`.
- `data-component-id`, `data-element-id`, `data-variant`, `data-option` i `data-part` oznaczają elementy UI.
- `data-prototype-tool`, `data-prototype-link` oraz `element.preview-profile` oznaczają narzędzia `user_preview`, które nie są UI produktu i leżą wyłącznie w dolnym panelu.

## Raport kontroli
Minimalna kontrola eksportu z 2026-09-29. Wynik: **pozytywny**.
- **Archiwum i checksumy — OK.** ZIP zawiera wyłącznie katalog `prototype/` z 8 wymaganymi plikami. `checksums.sha256` pokrywa 7 pozostałych plików i wszystkie sumy się zgadzają.
- **Manifest — OK.** Poprawny JSON z dokładnie 15 sekcjami, `revision_id: "r2.1"` i `base_revision: "r2.0"`. `entry`, `capture_url_template`, `profiles[].capture_surface`, stany, fixture'y i interakcje są identyczne z r2.0. Nie ma `renderer_id` ani parametrów legacy.
- **Interakcje — OK.** Wszystkie 16 wpisów `interactions[]` ma instancję z tym samym `interaction_id` i wskazuje istniejące stany.
- **Zasoby lokalne — OK.** Pliki są samowystarczalne, a linki prototyp ↔ katalog względne.
- **Smoke test zmienionego narzędzia — OK.**
  - **user_preview w oknie 1000×700:**
    - mac-wide renderuje się 1:1 (1440×932, bez transformacji), a obszar nad panelem się przewija i widać lewą krawędź;
    - panel leży na dole okna, pod obszarem powierzchni; wszystkie narzędzia i link do katalogu są wyłącznie w nim, a w powierzchni produktu nie ma żadnego narzędzia;
    - nie ma etykiety fixture'u ani skali.
  - **Działanie narzędzi:**
    - przyciski ‹ i ›, klawisze ← / → oraz wybór z listy zmieniają stan; lista, licznik i `state=` pokazują stan faktycznie otwarty, a klawisze strzałek w powierzchni produktu (np. kursor wykresu) nie zmieniają stanu;
    - przełączenie targetu na galaxy-s20 daje 360×800 i zachowuje stan.
  - **capture:** zachowanie jak w r2.0 dla alert-settings-editing (iphone-16), alert-settings-saved (mac-wide), intraday-rebound (galaxy-s20), stale (pixel-8) i empty-range (iphone-16-pro-max):
    - anchor występuje raz, w pozycji 0,0, w rozmiarze targetu, z rogami 0 i bez cienia;
    - jest jeden korzeń stanu, brak narzędzi, `ready` jest ustawione, a URL się nie zmienia.
- **Konsola i układ — OK.** Brak błędów konsoli, a automatyczny przegląd strony zakończył się bez zgłoszeń.
