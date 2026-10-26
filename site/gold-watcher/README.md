# Gold Watcher — prototyp r3.4

Klikalny kontrakt designu i zachowania, nie kod aplikacji. Baza: `r3.3`. Prototyp dostosowany do aplikacji z 24.10.2026 (GW-DESIGNER-BRIEF-010 z odpowiedziami Lidera 1 i 2). Pokazuje ustawienia domyślne aplikacji na minimalnym zestawie danych: próg paniki 20%, kalkulator 2000 r. / 10 lat, pierwszy wiersz tabeli sytuacji. Pozostałe przyciski są widoczne w swoich wariantach, ale nie przełączają danych.
- **`index.html`:** ekran `gold-overview` w 7 stanach na 5 targetach.
- **`catalog.html`:** katalog komponentów i ich wariantów, z tego samego kodu i tych samych fixture'ów.

## Uruchomienie
```
cd prototype
python3 -m http.server 8731 --bind 127.0.0.1
```
- Ekran: `http://127.0.0.1:8731/index.html?mode=user_preview&state=gold-overview.ready&target=mac-wide`. Pasek narzędzi ma wybór stanu i targetu.
- Katalog: `http://127.0.0.1:8731/catalog.html`.

Strona wczytuje `prototype-manifest.json` i `assets/fixtures.json` z lokalnego serwera; przez `file://` pokazuje błąd `fixtures-unavailable`. Runtime (React) i kod ekranu są wbudowane w HTML, bez sieci.

## Render
- `index.html?mode=capture&state=<state_id>&target=<target_id>`.
- Stany:
  - `gold-overview.ready` — 15.10.2026, poza rynkiem, sekcje zwinięte;
  - `gold-overview.ready-in-market` — 30.09.2024, w złocie (ceny w dolarach z 30.09.2024);
  - `gold-overview.details-expanded` — „Liczby…”, objaśnienia wykresu i wszystkie 7 sekcji rozwinięte, kalkulator z wykresem typowego dnia decyzji;
  - `gold-overview.loading` — „Pobieranie historii cen 34 z 94”, „Pobieranie…” zajęty;
  - `gold-overview.refreshing` — dane widoczne, „Odświeżanie…” zajęty;
  - `gold-overview.stale` — baner „Dane nieodświeżone” nad treścią;
  - `gold-overview.unavailable` — karta „Brak danych o cenie złota” i „Spróbuj ponownie”.
- Scenariusz `capture_scenario=price-chart-hover-normal` działa tylko z `gold-overview.ready` na `mac-wide`: zakres 1 rok, dzień 09.01.2026, podpowiedź z datą i ceną.
- Kody błędów: `missing-parameter`, `unknown-state`, `unknown-target`, `state-unavailable-for-target`, `unknown-capture-scenario`, `capture-scenario-unavailable`, `fixtures-unavailable`.
- Anchor `element.product-work-surface` występuje dokładnie raz, ma rozmiar targetu, `data-prototype-ready="true"` i nie zawiera narzędzi prototypu. Strona przewija się wewnątrz anchoru, w jednym kontenerze `main`.
- Telefony renderują się z dotykiem jako głównym wejściem (wskazówka dotykowa pod wykresem); `mac-wide` — z myszą.
- Przyciski „Odśwież” i „Spróbuj ponownie” nie zmieniają stanu (stany wybiera się parametrem albo paskiem narzędzi).

## Dane
`assets/fixtures.json` ma jeden rejestr `fixtures`:
- `default` — `GW-DESIGNER-DATA-STRAZNIK-002.json` (dzień danych 15.10.2026);
- `in-market` — `GW-DESIGNER-DATA-STRAZNIK-002-2024-09-30.json` (30.09.2024, własna seria);
- `no-data` — tekst postępu ładowania.

Struktura i wartości 1:1 z eksportu silnika (README `apps/web/scripts/designer-export`), bez redakcji tekstów. Jedyna zmiana: seria `"18 mies."` jest pominięta, bo jest identyczna z ostatnimi 379 dniami serii `"Całość"` (sprawdzone na wszystkich polach w obu plikach). Sekcja `prototyp` zawiera pola spoza eksportu:
- `szczyt1980` (3600 / 3400) i `lataKalkulatora` — ze zrzutu `GW-DESIGNER-DATA-TEKSTY-2026-10-24.txt`;
- `ostatniaSynchronizacja` dla banera `stale` — z r3.3;
- `capture_scenarios` i tekst ładowania.

Teksty stałe (nagłówki, opisy sekcji, instrukcje) są takie jak w kodzie aplikacji i sprawdzone ze zrzutem tekstów.

## Raport kontroli
Kontrola eksportu z 2026-10-26, powtórzona na paczce po poprawce interakcji wykresu ceny. Pomiar w przeglądarce na zbudowanym `index.html`: każda para otwarta osobno w `mode=capture`. Wynik: **pozytywny**.
- **Archiwum i checksumy — OK.** ZIP zawiera wyłącznie katalog `prototype/` z 8 plikami ze standardu. `checksums.sha256` pokrywa 7 pozostałych plików.
- **Manifest — OK.** `schema_version: 3`, `revision_id: "r3.4"`, 5 targetów, 7 stanów, `id_migrations`: `element.market-assessment` → `element.signal-status`, `element.recent-period` → `element.signal-status`. Każdy `fixture_id` istnieje w rejestrze.
- **Lokalne zasoby — OK.** Brak odwołań do sieci; manifest i fixture z tego samego katalogu.
- **Capture — OK we wszystkich 35 parach i w scenariuszu.** Każda para dała:
  - jeden anchor `element.product-work-surface` o rozmiarze targetu (1440×900, 393×852, 412×915, 360×800, 440×956) z `data-prototype-ready="true"`;
  - brak narzędzi prototypu, powielonych `data-element-id` i poziomego przewijania.

  Scenariusz `price-chart-hover-normal` ma `data-capture-scenario-id` i podpowiedź. Sprawdzone kody błędów: `missing-parameter`, `unknown-state`, `unknown-target`, `unknown-capture-scenario`, `capture-scenario-unavailable`; przy każdym nie ma anchoru.
- **Interakcje — OK.** `interaction.price-chart-hover`, `interaction.chart-zoom-click` i `interaction.chart-drag` mają w DOM własne `data-interaction-id`, `data-action` i `data-expected-effect`.
- **Nagłówek — OK we wszystkich 35 parach.** Tekst świeżości nie jest obcięty (`scrollWidth = clientWidth`). Odstęp od prawej krawędzi tekstu do lewej krawędzi „Odśwież” (px):

  | target | ready / in-market / expanded | loading | refreshing | stale | unavailable |
  | --- | --- | --- | --- | --- | --- |
  | mac-wide | 12 | 12 | 12 | 12 | 12 |
  | iphone-16 | 101,7 | 80,2 | 12 | 12 | 153,4 |
  | pixel-8 | 120,7 | 99,2 | 12 | 18 | 172,4 |
  | galaxy-s20 | 68,7 | 47,2 | 12 | 12 | 120,4 |
  | iphone-16-pro-max | 148,7 | 127,2 | 12 | 46 | 200,4 |
- **„Sygnał na dziś” na pierwszym ekranie — OK.** Pomiar: dolna krawędź trzeciego statusu względem góry anchoru (px).
  - `gold-overview.ready`: mac-wide 426/900, iphone-16 676/852, pixel-8 650/915, galaxy-s20 715/800, iphone-16-pro-max 650/956.
  - `ready-in-market`: 426, 659, 634, 681, 634.

  Etykieta, znak i zdanie są wyżej. W `stale` baner jest nad treścią, więc statusy są niżej (na iphone-16 i galaxy-s20 poniżej pierwszego ekranu); wymaganie dotyczy `ready`.
- **Teksty bez obcięcia — OK we wszystkich 35 parach.**
  - Pigułka znaku: tekst mieści się w tle z marginesem 20 px na `mac-wide` i 12 px na telefonach.
  - Żaden element z `white-space: nowrap` nie ma `scrollWidth > clientWidth`; wyjątek to tabela historii, która przewija się we własnym kontenerze.
- **Przegląd ręczny — OK.** `ready` i `ready-in-market` na `mac-wide` i `galaxy-s20`. Układ i geometria porównane z kodem aplikacji (`apps/web/src`: ta sama budowa komponentów, kolory, wymiary i progi responsywności), bez otwierania opublikowanej aplikacji. Treść zgodna z eksportem i zrzutem tekstów.
- **Katalog — OK.** Wszystkie wpisy renderują się z fixture'ów.
- **Konsola — OK.** Brak błędów w `index.html` i `catalog.html`.
