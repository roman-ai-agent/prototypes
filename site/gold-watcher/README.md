# Gold Watcher — prototyp r2.2 (base: r2.1)

Klikalny kontrakt designu i zachowania, a nie kod aplikacji. r2.0 to kanoniczna migracja zaakceptowanego r1.3. r2.1 zmienia wyłącznie narzędzie `user_preview` (jeden dolny panel, skala 1:1). r2.2 migruje wyłącznie nazwy pól `instances[]` w manifeście (`configuration`, `visibility`); pliki HTML są identyczne z r2.1. Powierzchnia produktu, stany, fixture'y, publiczne ID i `mode=capture` są bez zmian.

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
- `changelog.md`: r2.1 → r2.2, r2.0 → r2.1, r1.3 → r2.0 i migracje.
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
- **Manifest — OK.**
  - Poprawny JSON z dokładnie 15 sekcjami, `revision_id: "r2.2"` i `base_revision: "r2.1"`.
  - Każdy z 29 wpisów `instances[]` ma wyłącznie `configuration` i `visibility` (`state_ids`, `profile_ids`). W całym manifeście nie ma `config`, `visible_in_states` ani `visible_in_profiles`.
  - Treść `configuration` i wartości widoczności są identyczne z r2.1 (porównanie wpis po wpisie), a wszystkie `state_ids` i `profile_ids` istnieją.
  - `element.product-work-surface` występuje raz w `instances[]`, z 10 stanami i 5 targetami.
  - `entry`, `profiles[].capture_surface`, stany, fixture'y, interakcje i `id_migrations[]` są bez zmian.
- **Interakcje — OK.** Wszystkie 16 wpisów `interactions[]` ma instancję z tym samym `interaction_id`.
- **Pliki bez zmian — OK.** `index.html`, `catalog.html`, `responsive-spec.md` i `assets/fixtures.json` mają te same sumy SHA-256 co w r2.1.
- **Smoke test — OK.**
  - `mode=capture`: w stanach alert-settings-editing (pixel-8) i intraday-rebound (galaxy-s20) anchor występuje raz, w pozycji 0,0 i w rozmiarze targetu; jest jeden korzeń stanu i brak narzędzi.
  - `user_preview`: dolny panel i przełączanie stanu działają.
- **Konsola i układ — OK.** Brak błędów konsoli, a automatyczny przegląd strony zakończył się bez zgłoszeń.
