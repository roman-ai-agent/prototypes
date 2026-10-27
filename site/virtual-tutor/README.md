# VirtualTutor V4 — prototyp VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10` (ta sama w `prototype-manifest.json → revision_id`, `index.html`, `catalog.html`, `responsive-spec.md` i `changelog.md`).
- **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC9`.
- **Brief:** `VT-DESIGNER-BRIEF-010` (strona WWW pierwszej wersji, 1.0.0, bez „beta”). Bramka §4: 10 pytań; odpowiedzi Lidera `VT-DESIGNER-BRIEF-010-answers.md` (26.10.2026). D1, D2 `accepted`; D3 zaakceptowane 26.10, następnie **zmienione przez Użytkownika w przeglądzie 27.10** (nowa kolejność i forma — `changelog.md` p. 1–2) — do potwierdzenia w rejestrze decyzji.
- **Standard:** manifest v3; paczka 10 plików (wyjątek Lidera dla `assets/voice_help/*.png`, bez zmian).
- **Status:** kandydat do odbioru, niezaakceptowany.
- **Zakres:** nowy ekran `landing` / stan `landing-default` — publiczna strona do pobrania aplikacji na Maca, w prototypie jako osobna zakładka „Strona WWW”; 9 komponentów strony w katalogu; responsywność przez container queries. Ekrany aplikacji, ich stany i zachowanie bez zmian. Szczegóły: `changelog.md`.

## Pliki

| Plik | Rola |
| --- | --- |
| `index.html` | Produkt: 86 stanów (85 aplikacji + `landing-default`), tryby `capture` i `user_preview`; **zawiera kod strony WWW do publikacji** (sekcja „Publikacja strony”) |
| `catalog.html` | Katalog komponentów (48 `component_id`), nowa sekcja „12 · Strona WWW” (przykłady wczytywane z `index.html`) |
| `prototype-manifest.json` | Manifest v3; 87 wpisów `states[]` (z `catalog`) |
| `responsive-spec.md` | Specyfikacja responsywności (nowy §5.20) |
| `assets/fixtures.json` | 86 wpisów; nowy `fx-landing-default` |
| `assets/voice_help/*.png` | bez zmian |
| `changelog.md` | Zmiany RC10 (stany, ID, instancje akcji), historia |
| `checksums.sha256` | SHA-256 pozostałych 9 plików |

## Adresy

- **Zakładka strony:** `index.html` → przełącznik **„Aplikacja | Strona WWW”** w pasku `preview-profile`; albo `index.html?mode=user_preview&state=landing-default&target=<target_id>`. Ten sam wybór profilu (`mac-wide`, `iphone-16`, `pixel-8`, `galaxy-s20`, `iphone-16-pro-max` = brief: `mac-wide`, `iphone-standard`, `android-standard`, `mobile-compact`, `mobile-large`). „Aplikacja” wraca do ostatniego stanu aplikacji. Stan jest też na liście „Wybierz ekran” (ostatni, `landing · landing-default`).
- **Capture:** `index.html?mode=capture&state=landing-default&target=<target_id>` — kontrakt RC6–RC9 bez zmian: jeden `element.product-work-surface` o rozmiarze targetu z `data-prototype-ready="true"`; `product-surface[data-screen-id="landing"][data-state-id="landing-default"]` jest obszarem przewijania strony.
- **Selektor stanu:** `#vt-landing[data-screen-id="landing"][data-state-id="landing-default"]`. Manifest zawiera tylko `{ "state_id": "landing-default", "fixture_id": "fx-landing-default" }` — standard v3 nie zapisuje w manifeście ekranów ani selektorów (brief §7.6 wymienia je jako cechy stanu; są w DOM i w tym README).

## Publikacja strony (brief §7)

Kod strony leży wyłącznie w `index.html`, w dwóch blokach (bloku skryptu nie ma — strona go nie potrzebuje):

1. **Styl** — `<style id="vt-landing-style"> … </style>`, ostatni element `<head>` (poprzedza go komentarz `vt-landing: blok stylu`). Zawiera fonty jako `data:` (Plus Jakarta Sans 200–800 i Lora italic, pod nazwami `VTL …`); ok. 575 KB. Portrety tutorów (12 × WebP 288 px, ok. 160 KB) są `data:` w fragmencie. **Żaden `<link>` do fontów nie jest potrzebny.**
2. **Fragment** — od `<!-- vt-landing:start -->` do `<!-- vt-landing:end -->` włącznie: dokładnie jeden `<main id="vt-landing" data-screen-id="landing" data-state-id="landing-default">`. W prototypie fragment leży w `<template id="vt-landing-source">` tuż przed `</body>` (chrom klonuje go do zakładki) — kopiuje się tylko tekst między komentarzami, bez znacznika `<template>`.

Złożenie (zamień `STYL` i `FRAGMENT` na skopiowane bloki):

```html
<!doctype html><html lang="pl"><head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>VirtualTutor — rozmowy po angielsku z tutorami-postaciami na Macu</title>
<meta name="description" content="Rozmawiaj po angielsku o tym, co Cię ciekawi — z Sherlockiem Holmesem, Kleopatrą i innymi tutorami. Tutor przy okazji poprawia błędy i tłumaczy na polski.">
STYL
</head><body>
FRAGMENT
</body></html>
```

- **Tytuł:** `VirtualTutor — rozmowy po angielsku z tutorami-postaciami na Macu`
- **Opis:** `Rozmawiaj po angielsku o tym, co Cię ciekawi — z Sherlockiem Holmesem, Kleopatrą i innymi tutorami. Tutor przy okazji poprawia błędy i tłumaczy na polski.`

### Wartości do podstawienia

| Wartość | Miejsca we fragmencie | Przykład w zakładce |
| --- | --- | --- |
| `[[VT_DOWNLOAD_URL_MAC]]` | `href` w `landing-hero-download-mac` i `landing-download-mac` (2) | bez podstawienia; kliknięcie przechwytuje chrom |
| `[[VT_APP_VERSION]]` | `landing-hero-meta` („Wersja …”), parametr „Wersja” w `landing-platform-mac` (2) | 1.0.0 |
| `[[VT_DMG_SIZE]]` | `landing-hero-meta`, `landing-install-step-1` („Plik VirtualTutor.dmg ma …”), parametr „Plik” w `landing-platform-mac` (3) | 28 MB |
| `[[VT_MIN_MACOS]]` | `landing-hero-meta` („… lub nowszy”), parametr „Wymagania” w `landing-platform-mac` (2) | macOS 10.15 |

Przykłady pochodzą z `fx-landing-default` i są wstawiane przez chrom tylko w tekście kopii w zakładce; źródło w `index.html` zawiera znaczniki. Linki `download="VirtualTutor.dmg"` pomagają przy tym samym pochodzeniu; przy Cloudflare o nazwie pliku decyduje serwer.

### Izolacja

- Wszystkie selektory zaczynają się od `#vt-landing`; brak reguł dla `html`, `body`, `*`, `:root` i klas prototypu; tokeny `--vtl-*` zdefiniowane na `#vt-landing` (wartości z R11). Jedyne `@media` to `prefers-reduced-motion`; układ — `@container vtl`.
- `#vt-landing` ma `position:absolute; top:0; left:0; right:0; min-height:100%`: domyślny margines `body` pustej strony i tło `body` prototypu nie wpływają na wygląd; dokument przewija się normalnie (nawigacja `sticky`).
- Fragment nie zawiera `{{ }}`, `sc-*`, skryptów, atrybutów `style`, obrazów zewnętrznych ani odwołań do prototypu; ikony i flagi to inline SVG, portrety `data:image/webp`. Atrybuty `data-element-id` / `data-component-id` zostają.

## Raport kontroli (kontrola eksportu R11-RC10, 26.10.2026)

- **Bramka:** pytania z 26.10.2026 rozstrzygnięte w `VT-DESIGNER-BRIEF-010-answers.md`; uwagi Użytkownika z przeglądu 27.10.2026 wdrożone (`changelog.md` p. 2). Otwarte: potwierdzenie zmienionego D3 w rejestrze decyzji.
- **Struktura ZIP-a:** katalog główny `prototype/`, 10 plików (8 plików standardu + 2 obrazy `assets/voice_help/`).
- **Checksumy:** `checksums.sha256` obejmuje 9 plików; zweryfikowane na plikach zapisanych do ZIP-a.
- **Manifest v3:** JSON parsuje się; 5 kluczy; `schema_version: 3`; `revision_id` = `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10`; `id_migrations: []`; 5 targetów; 86 stanów (+ `catalog`), każdy `fixture_id` ma dane; brak `renderer_id`.
- **Próba „wycięcia” (brief §7, §10):** oba bloki wycięte tekstowo z `index.html` i złożone według szablonu wyżej w pusty dokument; dokument otwarty w ramkach o rozmiarach pięciu profili, dwa warianty: ze znacznikami `[[VT_…]]` i z podstawionymi przykładami.

  | Profil | Fonty VTL (400/700/italic) | Poziome przewijanie (`scrollWidth` / `clientWidth`) | Elementy poza szerokością | Geometria 47 `data-element-id` vs zakładka | Wysokość strony |
  | --- | --- | --- | --- | --- | --- |
  | `mac-wide` 1440×900 | wczytane | 1440 / 1440 — brak | 0 | identyczna (0 różnic) | 6392 |
  | `iphone-16` 393×852 | wczytane | 393 / 393 — brak | 0 | identyczna | 8428 |
  | `pixel-8` 412×915 | wczytane | 412 / 412 — brak | 0 | identyczna | 8277 |
  | `galaxy-s20` 360×800 | wczytane | 360 / 360 — brak | 0 | identyczna | 8780 |
  | `iphone-16-pro-max` 440×956 | wczytane | 440 / 440 — brak | 0 | identyczna | 8129 |

  Brak skryptów w złożonej stronie (0 błędów konsoli możliwych z jej strony); brak odwołań do zasobów zewnętrznych (`href`: tylko 5 kotwic `#vtl-*` i `[[VT_DOWNLOAD_URL_MAC]]`; `src` i `url()` tylko `data:`). Porównanie „identycznie” = pozycja i rozmiar każdego elementu z `data-element-id` względem `#vt-landing` (wariant z przykładami vs zakładka) oraz wczytane fonty; porównania pikselowego nie wykonano.
- **Zakładka w pięciu profilach** (`user_preview` i `capture`): każdy profil — `data-prototype-ready="true"`, jeden `element.product-work-surface` o rozmiarze targetu, `product-surface[data-screen-id=landing][data-state-id=landing-default]`, strona w obszarze przewijania targetu, `h1` jeden; przełącznik „Strona WWW” → `landing-default`, „Aplikacja” → poprzedni stan (`welcome`); zmiana profilu przy otwartej stronie przeładowuje ją w nowym rozmiarze (360 × 800: obszar 360 px, `scrollWidth` 360). Kliknięcie `landing-download-mac` / `landing-hero-download-mac`: brak nawigacji, komunikat „Pobranie VirtualTutor.dmg (prototyp: plik nie jest pobierany)”. Kotwica „Pobierz” przewija do sekcji wersji (nagłówek sekcji tuż pod przyklejoną nawigacją).
- **Regresja aplikacji (capture):** pełna macierz 86 × 5 = **430/430** na pierwszej wersji strony; po przebudowie strony (zmiana wyłącznie treści fragmentu i stylu) powtórzona na `galaxy-s20`: **86/86** (`body[data-prototype-state]`, jeden `element.product-work-surface` z gotowością i rozmiarem targetu, `product-surface[data-state-id]`); 0 błędów konsoli. Wyniki DOM stanów RC9 (czas rozmowy, blokady, tłumaczenie) identyczne z RC9 na wszystkich targetach. Przełączenie `landing-default` → `welcome` w tym samym dokumencie: strona usunięta, aplikacja gotowa.
- **Treść:** jeden `h1`; słowa „beta” i „limit” nie występują; 47 `data-element-id` z przedrostkiem `landing-`, bez duplikatów; wszystkie `img` z `alt=""` (dekoracyjne; nazwy w tekście lub `aria-label`); kontrast tekstu ≥ 4,5:1 (najsłabsze pary: `#6d7178` na `#fefbf8` 4,8:1, `#914f2f` na `#fffcf9` 6,1:1, `#2f6b45` na `#dcefe2` 5,3:1; na `#1f5053`: `#d5e7e7` 7,0:1, `#f5cebd` 6,2:1, `#fefbf8` 8,7:1; tytuł okna podglądu na `#f7f3ee` poprawiony na `#50565e` 6,7:1); widoczny fokus (3 px `#286265`, w sekcji wersji `#fefbf8`).
- **Katalog:** sekcja „12 · Strona WWW”: 9 wpisów, przykłady wczytane z `index.html`, ramki reagują na profil; licznik komponentów 48; nawigacja katalogu zawiera sekcję.
- **Ograniczenia:** zrzuty referencyjne i porównania pikselowe nie wykonane; ocena wizualna na zrzutach 1440, 393 i 360 px. Pobranie DMG nie jest testowane (adres znany przy publikacji). Wygląd ostrzeżenia macOS nie jest odtwarzany.
