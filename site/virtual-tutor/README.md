# VirtualTutor V4 — prototyp VT-VIRTUALTUTOR-PROTOTYPE-R11-RC3

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC3` (ta sama w `prototype-manifest.json → revision_id`, `index.html`, `catalog.html`, `responsive-spec.md` i `changelog.md`).
- **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC1`.
- **Standard:** `process/docs/prototype-package-standard.md` (manifest v3); paczka 10 plików — wyjątek Lidera dla `assets/voice_help/*.png`.
- **Status:** zaakceptowany przez Użytkownika 05.10.2026.
- **Zakres:** korekty R11 po audycie Lidera i uwagi Lidera z podglądu. Szczegóły: `changelog.md`.

## Uruchomienie

Sposób uruchamiania prototypu jest opisany w narzędziach procesu.

| Plik | Rola |
| --- | --- |
| `index.html` | Produkt: 74 stany, tryby `capture` i `user_preview` |
| `catalog.html` | Katalog komponentów (`data-component-id`) |
| `prototype-manifest.json` | Manifest v3: `schema_version`, `revision_id`, `id_migrations`, `targets`, `states` |
| `responsive-spec.md` | Specyfikacja responsywności dla człowieka |
| `assets/voice_help/*.png` | 2 ilustracje `voice-download-callout` (pliki bez zmian) |
| `assets/fixtures.json` | Rejestr danych `{ "fixtures": { … } }`, 74 wpisy |
| `changelog.md` | Zmiany R11-RC3, historia rewizji i migracji ID |
| `checksums.sha256` | SHA-256 pozostałych 9 plików |

## Adresy

- **Capture:** `index.html?mode=capture&state=<state_id>&target=<target_id>`. Wymaga obu parametrów; każdy inny parametr, nieznany stan lub target albo niedozwolona para → jawny błąd (`body[data-prototype-state="__error__"]`). Po gotowości: dokładnie jeden nieinteraktywny `[data-element-id="element.product-work-surface"]` o rozmiarze `targets[].width × height` z `data-prototype-ready="true"` (selektor gotowości: `[data-element-id="element.product-work-surface"][data-prototype-ready="true"]`); pomocniczo także `body[data-prototype-ready="true"]` i `data-prototype-state="<state_id>"`.
- **Podgląd:** `index.html` lub `index.html?mode=user_preview`, opcjonalnie z kompletną parą `&state=<state_id>&target=<target_id>`. Tylko jeden z pary, nieznany stan lub target → błąd; pozostałe parametry są ignorowane. Nie jest częścią kontraktu capture.
- **Pary:** wszystkie 74 × 5 (`states[]` bez `target_ids`).

## Targety

| `target_id` | `width × height` |
| --- | --- |
| `mac-wide` | 1440×900 |
| `iphone-16` | 393×852 |
| `pixel-8` | 412×915 |
| `galaxy-s20` | 360×800 |
| `iphone-16-pro-max` | 440×956 |

## Raport kontroli (proporcjonalna kontrola eksportu, 04.10.2026)

- **Układ ZIP-a:** katalog główny `prototype/`, 10 plików: 8 plików standardu + `assets/voice_help/open-voice-details-compact.png` (560×100) i `assets/voice_help/download-voice-compact.png` (560×112), bez zmian względem plików źródłowych.
- **Checksumy:** `checksums.sha256` obejmuje 9 plików; zweryfikowane 9/9 na plikach, z których budowany jest ZIP. Rozpakowania gotowego ZIP-a nie wykonano.
- **Manifest v3:** JSON parsuje się; dokładnie 5 kluczy; `schema_version: 3`; `revision_id` = `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC3`; `id_migrations: []`; 5 targetów (`mac-wide` 1440×900); 74 unikalne stany, każdy `fixture_id` ma dane w `assets/fixtures.json` (74 wpisy).
- **Zasoby lokalne:** 0 odwołań zewnętrznych w `index.html` i `catalog.html`; oba obrazy `voice_help` odwołują się do plików obecnych w paczce.
- **Smoke test zmienionego zakresu** (05.10.2026; każdy stan otwarty świeżo pod rzeczywistym adresem `index.html?mode=capture&state=<state_id>&target=<target_id>`, okno 1440×900, pomiar DOM po selektorze gotowości anchoru; 0 błędów konsoli we wszystkich):
  - `chat-reply-error` (`mac-wide`): gotowość po ~2,3 s; 1 × anchor; karta błędu `chat-reply-error` — tło #ffece2, obramowanie 1 px #f5cebd, promień 14 px; tekst „Nie udało się uzyskać odpowiedzi tutora.”; „Spróbuj ponownie” (`primary-cta[primary]`) i „Popraw wiadomość” (`primary-cta[secondary]`); brak „Ponów”; composer ukryty; karta widoczna w strumieniu, strumień przewinięty na dół. Po „Popraw wiadomość”: karta znika, composer widoczny z zachowaną treścią.
  - `training-tutor-reply-error` (`mac-wide`): ta sama karta (tło, obramowanie, promień, akcje); composer ukryty; po „Popraw wiadomość” composer z zachowaną treścią.
  - `chat-reply-plain-translation-error` (`mac-wide`): ta sama karta; tekst „Nie udało się przetłumaczyć odpowiedzi.”, tylko „Spróbuj ponownie”; lewa krawędź i szerokość identyczne z `supplemental-tutor-card[notice]` (202 px / 724 px).
  - `chat-variant-switched` (`mac-wide`): gotowość po ~2,5 s; 1 × anchor; komunikat „Od teraz: English UK”; pigułka `variant-pill` widoczna z tekstem „English UK”.
  - Wcześniej w tej rewizji: `wizard-voice-unavailable-with-fallbacks` (`mac-wide`) i `characters-returning` (`galaxy-s20`) — bez zgłoszonych problemów.
- **Nie wykonano:** capture 74 × 5, zrzuty, porównania pikselowe, porównanie aplikacja ↔ prototyp.
