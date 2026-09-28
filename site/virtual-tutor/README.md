# VirtualTutor V4 — prototyp VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1` (ta sama w `prototype-manifest.json → revision_id`, `index.html`, `catalog.html`, `responsive-spec.md` i `changelog.md`). Nazwa pliku ZIP nie jest numerem wersji.
- **base_revision:** `VT-VIRTUALTUTOR-PROTOTYPE-R9-RC2 (local working copy)` — `VirtualTutor-V4/product/ux/prototype/r9-rc2/`, nie historyczny ZIP R9-RC2.
- **Standard:** `CLD-PROTOTYPE-TECHNICAL-STANDARD-004`, brief `VT-DESIGNER-BRIEF-004`.
- **Status:** kandydat do odbioru.
- **Zakres R10-RC1:** migracja techniczna formatu. Wygląd i zachowanie produktu bez zmian względem R9-RC2. Zmienione: kontrakt URL, fixture jako dane JSON, kształt manifestu, ID profili. Szczegóły: `changelog.md`.
- **Osobny pakiet assetów:** `virtualtutor-ui-assets-r9-RC1.zip` (bez zmian). Nie wchodzi do tej paczki; prototyp ma te same ikony osadzone lokalnie.

## Uruchomienie

```
cd prototype
python3 -m http.server 8000
```

Otwórz `http://localhost:8000/index.html` (= `?mode=user_preview`). Przez `file://` prototyp pokazuje ekran diagnostyczny. Paczka działa bez sieci: fonty, portrety, React/ReactDOM i runtime są wbudowane w `index.html` i `catalog.html`.

| Plik | Rola |
| --- | --- |
| `index.html` | Produkt (74 stany, 30 ekranów); tryby `capture` i `user_preview` |
| `catalog.html` | Katalog komponentów = `components[]` (33) |
| `prototype-manifest.json` | Manifest kanoniczny (`schema_version: "1.0"`, 15 sekcji) |
| `responsive-spec.md` | Specyfikacja responsywności. Maszynowo: `responsive_rules.rules[]` |
| `assets/fixtures.json` | Dane 74 fixture: `{ "fixtures": { "<fixture_id>": { "data": { … } } } }` |
| `changelog.md` | Zmiany R10-RC1 i historia, migracje ID |
| `checksums.sha256` | SHA-256 pozostałych 7 plików |

## Kontrakt URL

- **Capture:** `entry.capture_url_template` = `index.html?mode=capture&state={state_id}&target={target_id}`. Czysty produkt, bez otoczki. `target_id` = `profiles[].profile_id`. Układ i rozmiar obszaru roboczego wynikają z `capture_surface.logical_size` targetu, nie z okna przeglądarki.
- **Podgląd:** `entry.user_preview_url` = `index.html?mode=user_preview`. Stan i target wybiera się w otoczce; adres się nie zmienia.
- **Domyślny tryb:** brak `mode` = `user_preview` (samo `index.html`, także z parametrami dodanymi przez podgląd aplikacji Claude). Jawny `mode` jest zawsze respektowany; do zrzutów ustawia się `mode=capture`.
- **Adresy legacy usunięte:** `?embed=1&state=…&profile=…`. Parametry `embed`, `profile`, `fixture` lub nieznany `mode` dają ekran diagnostyczny (`data-prototype-state="__error__"`); w `mode=capture` także brak lub nieznany `state` / `target`.
- **Gotowość:** `entry.ready_selector` = `body[data-prototype-ready="true"]`, z `data-prototype-state="<state_id>"`.
- **Selektor stanu:** `states[].selector` = `[data-component-id="product-surface"][data-state-id="<state_id>"]`.
- **Obszar roboczy:** `[data-element-id="element.product-work-surface"]` — jeden na stan, nieinteraktywny, `aria-hidden`.
- **Fixture:** manifest → `states[].fixture_id` → `fixtures[].path` → `fixtures[fixture_id].data`. Każdy stan ma dokładnie jedno fixture `fx-<state_id>`.
- **Klatka capture:** w `mode=capture` animacje nieskończone zatrzymane w `components[].motion.capture_frame_ms` przed sygnałem gotowości.
- **API pomocnicze (niekontraktowe):** `window.VirtualTutorPrototype`.

## Targety

| `target_id` | Rozmiar | Dawne ID |
| --- | --- | --- |
| `mac-wide` | 1440×932 | — |
| `iphone-16` | 393×852 | `ios-iphone-15` |
| `pixel-8` | 412×915 | `android-pixel-8` |
| `galaxy-s20` | 360×800 | `android-galaxy-s20` |
| `iphone-16-pro-max` | 440×956 | `ios-iphone-16-pro-max` |

## Raport kontroli R10-RC1

### Wykonane (kontrola danych na plikach paczki, 28.09.2026)

**Manifest**
- JSON poprawny; 15 sekcji w kolejności standardu; `schema_version` `1.0`, `revision_id` R10-RC1.
- 74 stany (unikalne), 30 ekranów (każdy stan ma istniejący ekran), 33 komponenty, 201 instancji (0 zduplikowanych `element_id`), 66 interakcji, 5 szablonów stanów.
- 74 fixture 1:1 ze stanami: `fixture_id` = `fx-<state_id>` w 74/74; każde `fixtures[].path` = `assets/fixtures.json`; 74/74 wpisów z `data` w pliku, 0 nadmiarowych.
- `id_migrations[]`: 39 wpisów (35 historycznych + 4 migracje profili R10-RC1: `ios-iphone-15` → `iphone-16`, `android-pixel-8` → `pixel-8`, `android-galaxy-s20` → `galaxy-s20`, `ios-iphone-16-pro-max` → `iphone-16-pro-max`); `mac-wide` bez migracji.
- 5 profili `role: product`; `capture_surface.logical_size` = `viewport` w 5/5.
- `entry.capture_url_template` i `entry.user_preview_url` zgodne z kontraktem; `components[app-shell].configuration` = `?mode=capture|user_preview (brak mode = user_preview)`, `?state`, `?target`.
- Integralność odwołań: każdy stan i target w `instances[].visibility` istnieje; każde `interaction_id` instancji istnieje; każda interakcja wskazuje istniejącą instancję.

**Adresy legacy i stare ID**
- 0 wystąpień `embed=1`, `?profile=` / `&profile=` i starych ID profili w `index.html`, `catalog.html`, `responsive-spec.md` oraz w manifeście poza `id_migrations[].from`.
- Stare ID występują w `changelog.md` (opis migracji i historia) oraz w tabeli „Targety” powyżej.

**Dokumenty**
- `README.md`, `changelog.md`, `responsive-spec.md`: rewizja R10-RC1, base_revision R9-RC2 (local working copy).
- `checksums.sha256`: wygenerowany dla 7 plików.

### Do wykonania przez odbierającego

1. Otwarcie 74 stanów × 5 targetów (370 uruchomień) przez `capture_url_template`: sygnał gotowości, błędy konsoli, elementy wychodzące poza obszar roboczy.
2. Spakowanie paczki, rozpakowanie, zgodność `checksums.sha256` i uruchomienie `index.html` z rozpakowanej kopii.

**Liczby:** 74 stanów · 30 ekranów · 33 komponentów · 201 instancji · 66 interakcji · 74 fixture · 39 migracji ID · 5 targetów.

**Status:** kandydat do odbioru.
