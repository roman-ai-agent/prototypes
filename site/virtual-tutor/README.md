# VirtualTutor V4 — prototyp VT-VIRTUALTUTOR-PROTOTYPE-R10-RC2

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC2` (ta sama w `prototype-manifest.json → revision_id`, `index.html`, `catalog.html`, `responsive-spec.md` i `changelog.md`).
- **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1` — `product/ux/prototypes/macos/r10-rc1/prototype/`.
- **Brief / standard:** `VT-DESIGNER-BRIEF-005`, `CANONICAL_PROTOTYPE_PACKAGE_STANDARD-V3`.
- **Status:** kandydat do odbioru, niezaakceptowany.
- **Zakres:** migracja techniczna do manifestu v3. Wygląd, zachowanie, treści, fixture, targety, 74 stany i publiczne ID DOM bez zmian. Szczegóły: `changelog.md`.

## Uruchomienie

```
cd prototype
python3 -m http.server 8000
```

Otwórz `http://localhost:8000/index.html` (= `mode=user_preview`). Przez `file://` prototyp pokazuje ekran diagnostyczny. Fonty, portrety, React/ReactDOM i runtime są wbudowane; paczka działa bez sieci.

| Plik | Rola |
| --- | --- |
| `index.html` | Produkt: 74 stany, tryby `capture` i `user_preview` |
| `catalog.html` | Katalog komponentów (`data-component-id`) |
| `prototype-manifest.json` | Manifest v3: `schema_version`, `revision_id`, `id_migrations`, `targets`, `states` |
| `responsive-spec.md` | Specyfikacja responsywności dla człowieka |
| `assets/fixtures.json` | Rejestr danych `{ "fixtures": { … } }`, 74 wpisy |
| `changelog.md` | Zmiany R10-RC2, historia rewizji i migracji ID |
| `checksums.sha256` | SHA-256 pozostałych 7 plików |

## Adresy

- **Capture:** `index.html?mode=capture&state=<state_id>&target=<target_id>`. Wymaga obu parametrów; każdy inny parametr, nieznany stan lub target albo niedozwolona para → jawny błąd (`body[data-prototype-state="__error__"]`). Po gotowości: dokładnie jeden nieinteraktywny `[data-element-id="element.product-work-surface"]` o rozmiarze `targets[].width × height` z `data-prototype-ready="true"` (selektor gotowości: `[data-element-id="element.product-work-surface"][data-prototype-ready="true"]`); pomocniczo także `body[data-prototype-ready="true"]` i `data-prototype-state="<state_id>"`.
- **Podgląd:** `index.html` lub `index.html?mode=user_preview`, opcjonalnie z kompletną parą `&state=<state_id>&target=<target_id>`. Tylko jeden z pary, nieznany stan lub target → błąd; pozostałe parametry są ignorowane. Nie jest częścią kontraktu capture.
- **Pary:** wszystkie 74 × 5 (`states[]` bez `target_ids`).

## Targety

| `target_id` | `width × height` |
| --- | --- |
| `mac-wide` | 1440×932 |
| `iphone-16` | 393×852 |
| `pixel-8` | 412×915 |
| `galaxy-s20` | 360×800 |
| `iphone-16-pro-max` | 440×956 |

## Raport kontroli (minimalna kontrola eksportu, 29.09.2026; po zwrocie odbioru)

- **Układ ZIP-a i checksumy:** katalog główny `prototype/` z 8 plikami standardu (`assets/fixtures.json` jako jedyny plik w `assets/`). `checksums.sha256` obejmuje 7 pozostałych plików; sumy policzone na plikach tuż przed spakowaniem i zweryfikowane 7/7. Rozpakowania gotowego ZIP-a nie wykonano.
- **Manifest v3:** JSON parsuje się; dokładnie 5 kluczy w kolejności standardu; `schema_version: 3`; `id_migrations: []`; 5 targetów z polami `target_id`, `width`, `height` i wymiarami zgodnymi z briefem; 74 unikalne stany z polami `state_id`, `fixture_id`, identyczne (ID i `fixture_id`, ta sama kolejność) z R10-RC1; każdy `fixture_id` wskazuje wpis z `data` w `assets/fixtures.json` (74/74). `assets/fixtures.json` bajtowo bez zmian względem R10-RC1.
- **Zasoby lokalne:** 0 odwołań do zewnętrznych adresów (`http(s)://`, `//`) w `src`, `href`, `url()`, `@import` i `fetch` w `index.html` i `catalog.html`.
- **Smoke test capture — rzeczywisty adres standardowy** (bez otoczki podglądu; dokument otwarty bezpośrednio pod `index.html?mode=capture&state=chat-reply-plain-translation-ready&target=mac-wide`, okno 1440×932): selektor `[data-element-id="element.product-work-surface"][data-prototype-ready="true"]` wyrenderowany po ~5 s i stabilny po kolejnych 1,5 s; 1 × anchor, 1440×932, `pointer-events: none`, `aria-hidden="true"`; `data-prototype-state` zgodny, ekran `chat-reply`, układ `wide`; brak otoczki podglądu i `data-prototype-tool`; brak błędów konsoli.
- **Smoke test negatywny:** `mode=capture` z dodatkowymi parametrami (`t`, `srcmap`, `_wr`) → jawny błąd „Nieznany parametr adresu capture”.
- **Smoke test podglądu:** `mode=user_preview&state=chat-reply-plain-translation-ready&target=iphone-16` → otoczka z wybranym stanem (lista 74 stanów) i targetem `iphone-16`.
- **Nie wykonano (poza zakresem briefu):** pełna macierz 74 × 5, zrzuty, porównanie pikselowe, ręczny przegląd.
