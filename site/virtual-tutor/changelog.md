# changelog — VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC9` · **Status:** kandydat do odbioru, niezaakceptowany.
- **Brief:** `VT-DESIGNER-BRIEF-010` (aktualizacja 26.10.2026: pierwsza wersja 1.0.0, bez „beta”) — publiczna strona WWW do pobrania aplikacji na Maca. Bramka §4: 10 pytań, odpowiedzi Lidera `VT-DESIGNER-BRIEF-010-answers.md` (26.10.2026). D1, D2 `accepted`; **D3 `accepted`** (kolejność sekcji i podgląd w oknie Maca).

## R11-RC10 (względem R11-RC9) — strona WWW `landing`

1. **Nowy ekran `landing`, stan `landing-default`** — cała strona, przewijana. Kolejność po przeglądzie Użytkownika 27.10.2026 (zmienia propozycję D3 z 26.10): nawigacja → hero (12 portretów tutorów, hasło „Rozmawiaj o tym, co Cię ciekawi. Angielski przyjdzie przy okazji.”, pobranie) → ekran rozmowy z objaśnieniami 1–4 → Tutorzy (12 kart) → Co jeszcze potrafi (3 karty z grafikami) → Jak zacząć (4 kroki) → Wersje i pobranie → Prywatność → stopka.
2. **Uwagi Użytkownika z przeglądu (27.10):** portrety tutorów na stronie; przekaz „ciekawa rozmowa + nauka przy okazji”; ekran rozmowy odtworzony według ekranu `chat-*` R11 (Sherlock Holmes jako główna postać, spójna rozmowa o znikającym serze); grafiki funkcji zamiast elementów UI; trening pokazany jako seria podobnych zdań z tą samą poprawką; **brak jakichkolwiek informacji o limitach**; równe portrety w hero.
3. **Kod strony w `index.html`** (brief §7.1): `<style id="vt-landing-style">` (koniec `<head>`) i `<!-- vt-landing:start -->` … `<main id="vt-landing" …>` … `<!-- vt-landing:end -->` (w `<template id="vt-landing-source">` przed `</body>`). Bez skryptu strony.
4. **Izolacja:** selektory wyłącznie `#vt-landing …`; tokeny `--vtl-*` z wartościami R11 (w tym 9 teł `tutor-card`); kroje `VTL Plus Jakarta Sans` i `VTL Lora` (italic) jako `data:`; 12 portretów jako WebP 288 px `data:` (z portretów aplikacji, łącznie ok. 160 KB); `#vt-landing` pozycjonowany absolutnie — marginesy i tło `body` bez wpływu.
5. **Responsywność wyłącznie przez `@container vtl`** (progi 1000 i 600 px) — responsive-spec §5.20.
6. **Wartości do podstawienia:** `[[VT_DOWNLOAD_URL_MAC]]` (2 × href), `[[VT_APP_VERSION]]` (2), `[[VT_DMG_SIZE]]` (3), `[[VT_MIN_MACOS]]` (2).
7. **Treść wg odpowiedzi Lidera 26.10:** pierwsze uruchomienie (zdanie wyjaśnienia, Ustawienia systemowe › Prywatność i ochrona › „Otwórz mimo to”, drobniej macOS 14 i starsze); „Mac z Apple Silicon lub Intel, [[VT_MIN_MACOS]] lub nowszy”; prywatność (DeepSeek, serwer nie zapisuje treści, zbiorcze statystyki 90 dni, historia tylko na Macu, anonimowy identyfikator urządzenia — bez wzmianki o limicie); stopka „© 2026 Tomasz Białobrzeg Cyber IT”. Bez słowa „beta”.
8. **Chrom prototypu:** przełącznik „Aplikacja | Strona WWW”; ten sam wybór profilu; przykłady (1.0.0, 28 MB, macOS 10.15 z `fx-landing-default`) tylko w tekście kopii; kliknięcie pobrania przechwytywane → „Pobranie VirtualTutor.dmg (prototyp: plik nie jest pobierany)”.
9. **Katalog:** sekcja „12 · Strona WWW” (9 komponentów), przykłady wczytywane z `index.html`.

### Stany
| `state_id` | Zmiana |
| --- | --- |
| `landing-default` | **nowy**: ekran `landing`, fixture `fx-landing-default` |
| pozostałe 85 | bez zmian |

Stany: 85 → **86** (manifest: 87 wpisów z `catalog`); fixtures: 85 → 86.

### ID
- **Dodane `component_id` (katalog 39 → 48):** `landing-nav`, `landing-hero`, `landing-app-preview`, `landing-tutor-card`, `landing-feature-card`, `landing-install-step`, `landing-platform-tile` [`available`|`soon`], `landing-privacy`, `landing-footer`.
- **Dodane `element_id` strony (47):** `landing-page`, `landing-nav`, `landing-nav-brand`, `landing-nav-link-tutors`, `landing-nav-link-features`, `landing-nav-link-install`, `landing-nav-download`, `landing-hero`, `landing-hero-tutors`, `landing-hero-download-mac`, `landing-hero-install-link`, `landing-hero-meta`, `landing-showcase`, `landing-app-preview`, `landing-showcase-captions`, `landing-tutors`, `landing-tutor-{curie|davinci|sherlock|cleopatra|copernicus|lovelace|confucius|austen|shakespeare|alice|robinhood|dorothy}` (12), `landing-features`, `landing-feature-training`, `landing-feature-dialect`, `landing-feature-voice`, `landing-install-steps`, `landing-install-step-1` … `-4`, `landing-versions`, `landing-platform-mac`, `landing-download-mac`, `landing-platform-iphone`, `landing-platform-android`, `landing-privacy`, `landing-privacy-model`, `landing-privacy-history`, `landing-privacy-account`, `landing-footer`.
- **Dodane `element_id` chromu (4):** `preview-view-switch`, `preview-view-app`, `preview-view-landing`, `preview-download-notice`.
- **Usunięte / zmienione:** brak. `id_migrations: []`. Wszystkie ID, stany i zachowanie RC9 bez zmian.

### Instancje akcji (interactions)
| `element_id` | `component_id[wariant]` | `action` | `expected_effect` |
| --- | --- | --- | --- |
| `landing-hero-download-mac` | `landing-hero` | pobranie pliku | przeglądarka pobiera `VirtualTutor.dmg` (`[[VT_DOWNLOAD_URL_MAC]]`); w zakładce: `preview-download-notice` |
| `landing-download-mac` | `landing-platform-tile[available]` | pobranie pliku | jw. |
| `landing-platform-iphone`, `landing-platform-android` | `landing-platform-tile[soon]` | brak („Wkrótce”) | — |
| `landing-nav-brand` | `landing-nav` | kotwica | `#vtl-start` |
| `landing-nav-link-tutors` / `-features` / `-install` | `landing-nav` | kotwica | `#vtl-tutorzy` / `#vtl-co-potrafi` / `#vtl-jak-zaczac` |
| `landing-nav-download` | `landing-nav` | kotwica | `#vtl-pobierz` |
| `landing-hero-install-link` | `landing-hero` | kotwica | `#vtl-jak-zaczac` |
| `preview-view-app` / `preview-view-landing` | chrom | wybór widoku | ostatni stan aplikacji / `landing-default` |

---

# changelog — VT-VIRTUALTUTOR-PROTOTYPE-R11-RC9 (historia)

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC9` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC8` · **Status:** kandydat do odbioru, niezaakceptowany.
- **Brief:** `VT-DESIGNER-BRIEF-009` — dzienny limit czasu rozmowy widoczny w aplikacji. Bramka §4: 6 pytań, odpowiedzi Lidera 24.10.2026. D1–D4 `accepted`; D5 — propozycja Designera przyjęta jako kierunek, forma do oceny Użytkownika w tej paczce.
- **Korekta briefu (odpowiedź Lidera):** „Przetłumacz” korzysta z modelu przez serwer i zużywa limit — przy wyczerpanym limicie jest wyłączone; „Odsłuchaj” działa zawsze (głos systemowy).

## R11-RC9 (względem R11-RC8) — dzienny czas rozmowy

1. **Wskaźnik pozostałego czasu** (`time-left-indicator`, `composer-time-left`): „Zostało dziś: 1 h 52 min” w wierszu pod polem wiadomości, w prawej grupie razem z `composer-char-counter` (`data-part="composer-status"`); podpowiedź po lewej. Grupa zawija się pod podpowiedź, gdy brakuje miejsca. Gdy pole jest ukryte (oczekiwanie na tutora, błąd odpowiedzi), wskaźnik stoi sam pod wierszem trybu. Rozmowa główna i trening; nie w pytaniach o profil ani edycji profilu.
2. **Warianty:** `normal` (12,5/500 `#50565e`), `low` ≤ 10 min (12,5/700 `#8a4b00` + ikona `vt-clock`), `unknown` „Zostało dziś: —” (`#6d7178`, bez wymyślonej wartości i bez animacji), `exhausted` „Zostało dziś: 0 min”. Format: „1 h 20 min”, „45 min”, „mniej niż 1 min”.
3. **Zmiana wartości** wyłącznie po odpowiedzi tutora (prototyp: −3 min na odpowiedź; pierwsza odpowiedź dnia zamienia `unknown` na wartość). Odpowiedź rozpoczęta przed wyczerpaniem zawsze się kończy; po niej stan `exhausted`. Bez odliczania na żywo.
4. **Ogłoszenie „mało czasu”:** `composer-time-announcement` (`role="status"`, wizualnie ukryty) — jednorazowo „Zostało mniej niż 10 minut rozmowy na dziś.” Wskaźnik ma `role="timer"` i nazwę „Pozostały czas rozmowy dzisiaj: …”.
5. **Komunikat** `limit-notice` (`composer-limit-notice`) jako pierwszy element `composer-dock`: `personal-exhausted` „Dzisiejszy czas rozmowy się skończył. Limit odnowi się o północy.” (ikona `vt-moon`), `service-unavailable` „Rozmowy są dziś chwilowo niedostępne z powodu dużego zainteresowania. Spróbuj ponownie jutro.” (ikona `vt-info`; wskaźnik czasu ukryty). `role="status"`, bez akcji.
6. **Blokada przy wyczerpanym limicie** (osobistym i usługi): `chat-send` i `composer-mic-button` wyłączone; `chat-input-field` `aria-disabled="true"`, tło `#f8f5f1`, pole tylko do odczytu (szkic czytelny, do skopiowania, „Wyczyść” działa); podpowiedź ukryta; pigułka trybu i „Zmień temat” `aria-disabled="true"` (reguła RC8: niedostępne, gdy niedostępne jest wysłanie); „Przetłumacz” wyłączone (`aria-disabled="true"`, krycie 0,55), wczytane tłumaczenia widoczne; „Odsłuchaj” działa; historia widoczna.
7. **Limit usługi przy wysyłce:** wiadomość nie jest wysłana i zostaje w polu jako szkic (`chat-service-unavailable`).
8. **Odnowienie:** o północy czasu użytkownika aplikacja sama odblokowuje rozmowę, bez stanu przejścia (prototyp tego nie symuluje).

### Stany
| `state_id` | Zmiana |
| --- | --- |
| `chat-idle` | zmieniony: wskaźnik `normal` „Zostało dziś: 1 h 52 min” (fixture: `timeLeft`) |
| `chat-time-unknown` | **nowy**: „Zostało dziś: —”; wysyłanie aktywne (decyduje serwer) |
| `chat-time-low` | **nowy**: 8 min, `low`; tryb „Nauczyć się”, historia jednej wymiany |
| `chat-time-exhausted` | **nowy**: limit osobisty wyczerpany po odpowiedzi tutora; komunikat, kompozytor i sterowanie wyłączone; wczytane tłumaczenie widoczne |
| `chat-service-unavailable` | **nowy**: limit usługi; niewysłany szkic w polu, wskaźnik ukryty |
| `training-time-exhausted` | **nowy**: trening przy wyczerpanym limicie osobistym |
| pozostałe `chat-*` i `training-*` | wskaźnik `normal` 1 h 52 min (wartość domyślna stanu; fixture bez zmian) |

Stany: 80 → **85** (manifest: 86 wpisów z `catalog`); fixtures: 80 → 85.

### ID
- **Dodane `component_id`:** `time-left-indicator`, `limit-notice` (katalog: 37 → 39).
- **Dodane `element_id`:** `composer-time-left`, `composer-time-announcement`, `composer-limit-notice`, `composer-limit-notice-text`.
- **Dodane `state_id`:** 5 (tabela wyżej). **Zmienione:** `chat-idle` (fixture).
- **Zmienione warunki (semantyka bez zmian):** `chat-send`, `composer-mic-button` (+ `aria-disabled`), `chat-input-field` (+ `aria-disabled`, textarea `aria-readonly`), `conversation-mode-change`, `conversation-topic-change`, `*-translation-action` — dodatkowy warunek wyłączenia: wyczerpany limit.
- **Usunięte:** brak. `id_migrations: []`. Chronione ID RC8 bez zmian, wiersz trybu i tematu bez zmian zachowania.

### Instancje akcji
Nowe komponenty są nieinteraktywne (brak `action`). Instancje akcji RC8 bez zmian; w stanach wyczerpania: `action` bez skutku (`expected_effect`: brak — element `aria-disabled`/`disabled`).

---


- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC8` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC7` · **Status:** kandydat do odbioru, niezaakceptowany.
- **Źródło zmiany:** decyzje Użytkownika z przeglądu RC7 (24.10.2026), szkic „Propozycja tryb przy kompozytorze”. Brief `VT-DESIGNER-BRIEF-008` bez zmian (położenie i forma były po stronie Designera; D3 nadal spełnione).

## R11-RC8 (względem R11-RC7) — tryb i temat przy polu wiadomości

1. **Położenie:** pasek `conversation-focus` spod nagłówka rozmowy usunięty. Ten sam element jest teraz **wierszem w `composer-dock` nad polem wiadomości**. Widoczny tylko w rozmowie głównej — **nie w treningu** (`training-*`), pytaniach o profil ani edycji profilu. Wiersz zostaje widoczny (disabled), gdy pole wiadomości jest ukryte: oczekiwanie na tutora (`chat-typing`, `chat-mode-switch-sent`) i błąd odpowiedzi (`chat-reply-error`).
2. **Pigułka trybu (wariant A1):** `conversation-mode-change` łączy informację i przycisk — małe „chcę” (10,5/600 `#6d7178`) nad nazwą trybu małą literą (13/700), np. „chcę / nauczyć się”, chevron w górę. Kolor szary (`#f5f1ec`, ramka `#e3ddd5`), turkus (`#e0f3f4` / `#c2dedf`) tylko przy otwartej liście — celowo inaczej niż pigułka profilu, żeby nie konkurować z mikrofonem i „Wyślij”. Nieznany tryb: „tryb / jeszcze nieznany” (500, `#6d7178`). Nazwa dostępna: „Zmień tryb rozmowy. Obecny: …”.
3. **Lista trybów** (`conversation-mode-menu`) otwiera się **w górę**: wide — 340 px nad pigułką; telefon — na szerokość kolumny nad kompozytorem, wiersze 52 px. Pozycje, opisy i wysyłane wiadomości bez zmian, poza opisem niżej.
4. **Opis trybu „Wygadać się”:** „Tutor głównie słucha.” → **„Tutor rozumie, co czujesz.”** Wysyłana wiadomość EN bez zmian (tabela briefu §2).
5. **Temat:** 12/1.4 `#50565e`, EN, **do 2 wierszy z wielokropkiem**. Wide: między pigułką a „Zmień temat” (długi przykład mieści się w 1 wierszu). Telefon: pigułka i „Zmień temat” w pierwszym wierszu, temat pod nimi na całą szerokość. Skrócony temat (> 110 znaków na telefonie, > 260 na wide) jest przyciskiem `conversation-topic-expand` — kliknięcie pokazuje całość.
6. **„Zmień temat”** (`conversation-topic-change`): link tekstowy 12,5/600 `#286265` na końcu wiersza (był pigułką). Zachowanie bez zmian.

**ID:** usunięte (względem RC7, nieodebranej) `conversation-mode-label` („TRYB”) i `conversation-topic-label` („TEMAT”); dodane `conversation-mode-lead` („chcę” / „tryb”). `conversation-topic-expand` zmienia formę (link „Pokaż cały temat” → klikalny tekst tematu). Pozostałe ID RC7 i wszystkie ID RC6 bez zmian; `id_migrations: []` (RC7 nie została odebrana, więc ID z RC7 nie są publiczne).
**Stany:** te same 80 `state_id` i fixture co w RC7. **Bez zmian:** wyrównanie do 4 odstępstw z RC7, licznik znaków, karty poprawek, nagłówek z perspektywą.

---

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC7` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC6` · **Status:** kandydat do odbioru, niezaakceptowany.
- **Brief:** `VT-DESIGNER-BRIEF-008` — tryb i temat rozmowy widoczne i sterowane przez ucznia. Bramka §4: brak pytań.

## R11-RC7 (względem R11-RC6)

### 1. Tryb i temat rozmowy (nowe)
- **`conversation-focus-bar`** (`conversation-focus`): pasek pod `secondary-conversation-header`, nad historią, tylko w rozmowie głównej (nie w treningu, profilu i edycji profilu). Pokazuje „TRYB” + nazwę trybu i „TEMAT” + temat po angielsku (jak zapisuje tutor, `lang="en"`). Informacja, nie przełącznik.
- **Nieznany tryb/temat** (nowa rozmowa, przed pierwszą odpowiedzią tutora): osobny wygląd — „jeszcze nieznany” i „Tutor rozpozna tryb i temat po Twojej pierwszej wiadomości.” w kolorze `#6d7178`, zwykła grubość; akcje dostępne.
- **„Zmień tryb”** (`conversation-mode-change`, `conversation-steer-button[mode]`) obok nazwy trybu otwiera `anchored-menu[mode-menu]` (`conversation-mode-menu`) z 5 pozycjami `mode-option` (`conversation-mode-option-{vent|learn|sort|decide|chat}`), bieżąca oznaczona (`menuitemradio`, `aria-checked`, tło `#e0f3f4`, znacznik). Wybór innego trybu wysyła widoczny dymek ucznia z tekstem EN z briefu i zwykłe oczekiwanie na tutora; wybór bieżącego tylko zamyka listę. Wyświetlany tryb zmienia się po odpowiedzi tutora (podąża za rozpoznaniem).
- **„Zmień temat”** (`conversation-topic-change`, `conversation-steer-button[topic]`): wysyła dymek `I'd like to change the subject.`; tutor odpowiada pytaniem o nowy temat; wyświetlany temat zostaje do ponownego rozpoznania.
- **Długi temat:** do 2 wierszy z wielokropkiem (pełny tekst w `title` i nazwie dostępnej); na telefonie przy temacie > 72 znaków link „Pokaż cały temat” / „Zwiń temat” (`conversation-topic-expand`, `aria-expanded`), na wide > 200 znaków.
- **Dostępność akcji:** disabled (krycie 0,55, `aria-disabled="true"`) jak wysłanie wiadomości — oczekiwanie na tutora, nagrywanie, rozpoznawanie mowy, błąd odpowiedzi tutora z ukrytym polem (do „Popraw wiadomość”), reklama. Wybrana akcja nie czyści szkicu w polu wiadomości.
- **Prototyp:** poprzednia wymiana przechodzi do historii (`chat-history-learner-n`, `chat-history-tutor-n`) przy kolejnej wiadomości; „Wyczyść rozmowę” zeruje tryb, temat i historię.

### 2. Wyrównanie do aplikacji (zaakceptowane odstępstwa)
- **VT-CHAT-HEADER-PERSPECTIVE-001** — `chat-tutor-meta`: zamiast etykiety stylu opis perspektywy z kafelka tutora (w prototypie `personality` z karty `tutor-card`), 12,5 px, `#50565e`, do 2 wierszy z wielokropkiem; nagłówek 75 px. **Zmiana semantyki chronionego ID** (etykieta stylu → opis perspektywy); ID bez zmian.
- **VT-CHAT-BUBBLE-WIDTH-001** — dymki ucznia i tutora (z powitaniem i Please note, także historia) do 80% szerokości historii we wszystkich klasach; dymek tutora wypełnia swój blok; karty pomocnicze od +24 px do prawej krawędzi dymka. Pytania i odpowiedzi profilu bez zmian (748 px / dotychczasowe %).
- **VT-MESSAGE-LENGTH-LIMIT-001** — `char-counter` (`composer-char-counter`) „X / 9 000 znaków” po prawej w wierszu podpowiedzi, gdy w polu jest tekst; po przekroczeniu: czerwony licznik i obramowanie pola, `composer-length-error` „Wiadomość jest za długa — skróć ją” pod polem, `chat-send` wyłączony (także strażnik wysyłki). Tylko rozmowa główna.
- **VT-TUTORS-ARCHETYPES-R2-001-CORRECTION-KINDS** — `correction-card` z `data-option`: `grammar` „A small grammar correction” (dotychczasowy wygląd) i `natural` „A more natural way to say it” (`#e9f3e5`, ramka `#bcd9b4`, tekst `#2f6334`, ikona dymka `vt-speech`). Tytuł ma ID `correction-title`.

### 3. Stany
| `state_id` | Zmiana |
| --- | --- |
| `chat-idle` | zmieniony: pasek w stanie `unknown`, akcje dostępne, nagłówek z perspektywą |
| `chat-mode-topic-known` | **nowy**: tryb „Nauczyć się”, długi temat EN, historia jednej wymiany |
| `chat-mode-menu-open` | **nowy**: otwarta lista trybów, „Nauczyć się” oznaczony |
| `chat-mode-switch-sent` | **nowy**: po wyborze „Podjąć decyzję” — dymek `I need to make a decision. Help me weigh my options.`, „Tutor pisze”, akcje disabled, tryb nadal „Nauczyć się” |
| `chat-topic-change-sent` | **nowy**: dymek `I'd like to change the subject.` i odpowiedź tutora z pytaniem o temat; temat poprzedni |
| `chat-message-too-long` | **nowy**: przekroczony limit 9 000 znaków (9 015), krótki temat EN, tryb „Podjąć decyzję” |
| `chat-reply-correction-natural` | **nowy**: karta „A more natural way to say it” |
| `chat-reply-correction`, `chat-reply-plain` | zmienione: pasek (tryb „Pogadać”, temat), szerokości 80%, tytuł „A small grammar correction” |
| `chat-reply-plain-translation-*`, `chat-reply-dialect`, `chat-reply-both`, `chat-new-conversation-confirm`, `chat-profile-popover`, `chat-variant-switched`, `chat-ad-active` | fixture uzupełnione o rozpoznany tryb i temat (po odpowiedzi tutora) |
| `chat-typing`, `chat-recording`, `chat-processing`, `chat-transcribed`, `chat-reply-error`, `variant-popover-open` | pasek `unknown` (pierwsza wiadomość bez odpowiedzi); w `chat-typing`, `chat-recording`, `chat-processing`, `chat-reply-error` akcje disabled |

Stany: 74 → **80** (manifest: 81 wpisów z `catalog`); fixtures: 74 → 80.

### 4. ID
- **Dodane `element_id`:** `conversation-focus`, `conversation-mode-label`, `conversation-mode-value`, `conversation-mode-change`, `conversation-mode-menu`, `conversation-mode-menu-note`, `conversation-mode-option-vent`, `conversation-mode-option-learn`, `conversation-mode-option-sort`, `conversation-mode-option-decide`, `conversation-mode-option-chat`, `conversation-topic-label`, `conversation-topic-value`, `conversation-topic-expand`, `conversation-topic-change`, `composer-char-counter`, `composer-length-error`, `correction-title`, `chat-history-learner-{n}`, `chat-history-tutor-{n}` (+ pochodne `-replay`, `-actions`, `-translation-*`).
- **Dodane `component_id`:** `conversation-focus-bar`, `conversation-steer-button`, `mode-option`, `char-counter`. **Rozszerzone:** `anchored-menu` (wariant `mode-menu`), `supplemental-tutor-card[correction]` (`data-option` `grammar` | `natural`).
- **Dodane `state_id`:** 6 (tabela §3).
- **Zmieniona semantyka:** `chat-tutor-meta` (opis perspektywy zamiast etykiety stylu; `data-ui-variant` `meta` → `perspective`).
- **Usunięte:** brak. `id_migrations: []`.
- **Zachowane bez zmian semantyki:** wszystkie pozostałe `element_id`, `component_id` i `state_id` RC6, w tym `chat-input-field`, `chat-send` (nowy warunek wyłączenia: przekroczony limit — zgodnie z odstępstwem), `composer-mic-button`, `secondary-conversation-header`, `chat-messages`, `chat-bubble-user`, `chat-bubble-reply`, `chat-typing-indicator`, `correction-card`, `chat-reply-error`.

### 5. Instancje akcji
| `element_id` | `component_id`[wariant] | `action` | `expected_effect` | Rola · nazwa dostępna | Przełącznik |
| --- | --- | --- | --- | --- | --- |
| `conversation-mode-change` | `conversation-steer-button[mode]` | otwórz/zamknij listę trybów | `conversation-mode-menu` widoczne/ukryte; nic nie wysłane | button · „Zmień tryb rozmowy” (`aria-haspopup="menu"`, `aria-expanded`) | nie |
| `conversation-mode-option-vent` | `mode-option` | wybierz „Wygadać się” | dymek ucznia `I just need to talk something through. Please mostly listen.` → oczekiwanie na tutora; bieżący: tylko zamyka listę | menuitemradio · „Wygadać się” | nie |
| `conversation-mode-option-learn` | `mode-option` | wybierz „Nauczyć się” | dymek `I want to learn about something. Please explain and teach me.` → oczekiwanie | menuitemradio · „Nauczyć się” | nie |
| `conversation-mode-option-sort` | `mode-option` | wybierz „Poukładać myśli” | dymek `I want to sort out my thoughts. Help me put them in order.` → oczekiwanie | menuitemradio · „Poukładać myśli” | nie |
| `conversation-mode-option-decide` | `mode-option` | wybierz „Podjąć decyzję” | dymek `I need to make a decision. Help me weigh my options.` → oczekiwanie | menuitemradio · „Podjąć decyzję” | nie |
| `conversation-mode-option-chat` | `mode-option` | wybierz „Pogadać” | dymek `I'd just like to chat.` → oczekiwanie | menuitemradio · „Pogadać” | nie |
| `conversation-topic-change` | `conversation-steer-button[topic]` | zmień temat | dymek `I'd like to change the subject.` → oczekiwanie → tutor pyta o temat | button · „Zmień temat rozmowy” | nie |
| `conversation-topic-expand` | link tekstowy | rozwiń/zwiń temat | pełny / skrócony tekst tematu; nic nie wysłane | button · „Pokaż cały temat” / „Zwiń temat” (`aria-expanded`) | nie |

**Chronione bez zmian:** 5 targetów, kontrakt capture i `element.product-work-surface`, schema manifestu v3, `assets/voice_help/*`, wszystkie ekrany poza rozmową główną (kreator, profil, wybór tutora, trening).

---

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC6` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC5` · **Status:** kandydat do odbioru, niezaakceptowany.
- **Źródło zmiany:** decyzja Lidera z podglądu.

## R11-RC6 (względem R11-RC5) — widoczny przycisk „Ten głos mi odpowiada”

1. **`wizard-voice-m`** — fixture: zaznaczony głos `us-m-evan` (Evan, pierwszy wiersz listy, `data-ui-state="selected"`), bez odtwarzania próbki (`sampleVoiceId: null`). Dzięki temu na ekranie widoczny jest przycisk `voice-confirm` „Ten głos mi odpowiada”. Stan nadal przedstawia listę głosów męskich drugiego kroku głosu.
2. **Bez zmian:** `wizard-voice-f` pozostaje stanem początkowym listy bez zaznaczenia (bez przycisku); `wizard-voice-sample-playing` (odtwarzanie), `wizard-voice-sample-error`, `wizard-voice-unavailable-*`, `wizard-voice-change-only*` bez zmian.
3. **Numer rewizji** RC6 w `prototype-manifest.json`, komentarzu `REVISION` w `index.html`, `catalog.html`, `responsive-spec.md`, `README.md`, `changelog.md`. Kod i UI `index.html` bez zmian.

**Chronione bez zmian:** 74 `state_id`, 5 targetów, interakcje, publiczne `element_id` (`id_migrations: []`), `voice-download-callout`, `assets/voice_help/*`. Zmieniony jest wyłącznie fixture `fx-wizard-voice-m` w `assets/fixtures.json`.

---

- **Brief:** `VT-DESIGNER-BRIEF-006` — korekta katalogu po odbiorze R11-RC4.

## R11-RC5 (względem R11-RC4) — wyłącznie `catalog.html`

1. **Katalog listy głosów (`voice-selector` / `voice-option`):** usunięty przykład zbiorczej grupy „INNE AKCENTY ANGIELSKIE” z wierszem „Karen · akcent australijski”. Zamiast niego grupy per konkretny akcent, w zapisie ekranów produktu: „Głosy English UK” (Kate), „Głosy English Australia” (Karen, Charlotte), „Głosy English Ireland” (Moira), „Głosy English South Africa” (Tessa). „Głosy English India” nie jest pokazany — przykład katalogu używa głosów kobiecych, a w danych ten akcent występuje tylko dla głosów męskich.
2. **Nagłówki grup w katalogu** zapisane jak na ekranach produktu, bez drukowanych liter — także istniejący „GŁOSY ENGLISH US” → „Głosy English US”.
3. **Wiersze głosów w przykładzie** zawierają wyłącznie nazwę głosu i akcję „Odsłuchaj”, bez nazwy akcentu.
4. **Numer rewizji** zaktualizowany do RC5 w `catalog.html`, `prototype-manifest.json` (`revision_id`), `responsive-spec.md`, `README.md`, `changelog.md` oraz w komentarzu `REVISION` w `index.html` (decyzja Lidera: jedyna zmiana w `index.html`).

**Bez zmian:** kod, UI i zachowania `index.html`; `assets/fixtures.json`; `assets/voice_help/*`; 74 `state_id`, 5 targetów, interakcje; `voice-download-callout`; publiczne `element_id` (`id_migrations: []`). Zmiany R11-RC4 (`open-sound-input-settings`, `profile-menu-cancel`, usunięcie `wizard-step-label`) pozostają.

---


## R11-RC4 (względem R11-RC3) — 4 zmiany ze zgodności aplikacja ↔ prototyp

1. **`wizard-mic-error-no-signal`, `wizard-mic-error-too-quiet`** — dodany secondary button „Otwórz ustawienia wejścia dźwięku” (`primary-cta[secondary]`, nowe ID `open-sound-input-settings`) obok „Spróbuj ponownie” (`retry-check`, bez zmian). Akcja otwiera ustawienia wejścia dźwięku (w prototypie: przekazanie do ustawień systemowych, ekran błędu zostaje).
2. **Listy głosów z fallbackami** (`wizard-voice-*`) — usunięta zbiorcza grupa „Inne akcenty angielskie” (zastępuje zapis z R11-RC3). Głosy zastępcze grupowane nagłówkami per akcent: „Głosy English UK” / „Głosy English US” (drugi wariant), „Głosy English Australia”, „Głosy English Ireland”, „Głosy English South Africa”, „Głosy English India”. Wiersze głosów bez dopisanego akcentu. `voice-download-callout` bez zmian (pierwszy element listy; zwinięty przy dostępnym właściwym głosie, rozwinięty przy jego braku).
3. **`chat-profile-popover`** — dodany „Anuluj” (ID `profile-menu-cancel`, `primary-cta[secondary]`): zamyka popover bez zmiany profilu.
4. **Pasek postępu onboardingu** — usunięta etykieta „Krok n z 6”: z prawej strefy `top-header` (`wizard-step-label`, wide) i z podpisu `onboarding-stepper` (mobile/compact; podpis pokazuje tylko etykietę bieżącego etapu). Sam pasek etapów bez zmian. `catalog.html` i `responsive-spec.md` zaktualizowane.

**Chronione bez zmian:** 74 `state_id`, 5 targetów, manifest v3 (`id_migrations: []`), `assets/fixtures.json` (bez zmian względem R11-RC3), publiczne ID poza: dodane `open-sound-input-settings`, `profile-menu-cancel`; usunięte `wizard-step-label`.

---

- **Źródła zmian:** brief korekt R11 po audycie Lidera (pkt 1–13) oraz uwagi i decyzje Lidera z podglądu, które mają pierwszeństwo tam, gdzie się z briefem nie zgadzają. Rewizja R11-RC2 była wersją pośrednią; jej zmiany zawiera ta rewizja.
- **Paczka:** 10 plików — 8 plików standardu oraz `assets/voice_help/open-voice-details-compact.png` i `assets/voice_help/download-voice-compact.png` (decyzja Lidera). Wszystkie 9 plików poza `checksums.sha256` w checksumach.

## R11-RC3 (względem R11-RC1) — zmienione stany i elementy

### Wizard i ekran powitalny
- **`welcome`** — `welcome-subtitle`: złamanie po „—”, obie linie wyśrodkowane (wyjątek Lidera od pkt 13).
- **`wizard-choose-variant`** — `choose-variant-us` / `choose-variant-uk`: wspólny komponent `primary-cta[primary]` (turkusowy), dwa równorzędne przyciski; wcześniej lokalne `choice_card`. Dodane publiczne ID `choose-variant-heading`.
- **Wszystkie `wizard-*`, `wizard-summary`, `variant-gate`** — tytuły i opisy bez `max-width`; zdania opisów w osobnych wierszach. `max-width` zostaje dla list, fraz STT i szarego pola `wizard-stt-result-mismatch`.
- **Token `--width-copy-standard`**: 44ch → 55ch (globalnie). **Token `--width-heading-error`** usunięty — nieużywany (pkt 6 briefu w tej części nieaktualny).
- **Pigułka wariantu `variant-pill`** — nie występuje w żadnym stanie `wizard-*`, w pytaniach o profil (`profile-name*`, `profile-age*`, `profile-level`) ani w edycji profilu (`profile-edit-*`); na wyborze tutora tylko po zakończonym onboardingu (`characters-returning` tak, `characters` nie); w rozmowie i treningu bez zmian.

### Lista głosów (wszystkie 8 stanów `wizard-voice-*`)
- `voice-source-note` złamany po „—”; `voice-source-note` i `voice-unavailable-description` na tokenach opisu ekranu (`--type-screen-description-*`).
- `voice-list-scroll`: 338 → **368 px**. Strzałka przy „Przewiń listę, aby zobaczyć wszystkie głosy” usunięta; podpowiedź pod listą także przy scrollu wynikającym z rozwiniętego calloutu.
- **`voice-download-callout`** (element `voice-add-voices`): pierwszy zwykły element listy, bez `sticky`; rozwinięty domyślnie, gdy brak głosów wybranego wariantu, zwinięty, gdy istnieje pasujący głos. 4 kroki z briefu; ilustracje pod krokami 2 i 3 — pliki bez zmian, statyczne, nieklikalne, skalowane w CSS (`width: 100%; max-width: 560px`). Nowe ID: `voice-settings-steps`, `voice-help-open-details`, `voice-help-download`.
- **Grupa „Inne akcenty angielskie”** zawiera drugi wariant (dla US: głosy UK, „akcent brytyjski”; dla UK: głosy US, „akcent amerykański”) i pozostałe akcenty.
- **`wizard-voice-unavailable-with-fallbacks`** — fixture: brak głosów English US, lista: English UK + inne akcenty; callout rozwinięty.
- **`wizard-voice-unavailable-empty`** — fixture: brak jakichkolwiek głosów angielskich; pusta lista z komunikatem; callout rozwinięty.
- **`wizard-voice-sample-error`** — fixture: błąd próbki w pierwszym widocznym wierszu (`voice-option-us-f-ava`).

### Profil
- **`profile-level`, `profile-edit-level`** — `profile-level-legend`: `width: 100%; max-width: 900px`.
- **`profile-edit-level`** — wybór poziomu zapisuje od razu; `profile-edit-save` (`Zapisz`) usunięty; `profile-edit-cancel` jako obrysowana pigułka `primary-cta[secondary]`.

### Rozmowa i trening
- **`chat-reply-*`** — `translation-card` (loading, ready, error) z tą samą lewą krawędzią i szerokością co `supplemental-tutor-card[notice]` (24 px; w drugiej gałęzi było 48 px).
- **`chat-reply-both` i pozostałe stany rozmowy** — strumień `chat-messages` otwiera się przewinięty na sam dół: widoczny najniższy element ostatniej odpowiedzi (tłumaczenie, small correction, dialect).
- **`chat-reply-error`, `training-tutor-reply-error`, `chat-reply-plain-translation-error`** — jedna wizualna karta błędu (tło, obramowanie, typografia, główna akcja `primary-cta[primary]`). Tutor: „Nie udało się uzyskać odpowiedzi tutora.”, akcje „Spróbuj ponownie” i „Popraw wiadomość” (nowe ID `chat-reply-edit`, `training-tutor-reply-edit`). Tłumaczenie: „Nie udało się przetłumaczyć odpowiedzi.”, tylko „Spróbuj ponownie”. „Ponów” usunięty. W błędzie tutora i treningu composer ukryty; „Popraw wiadomość” ukrywa kartę i pokazuje composer z zachowaną treścią.
- **`characters-returning`** — plakietka `OSTATNIO` nad avatarem (`z-index`, w układzie wąskim `position: relative`).
- **`chat-variant-switched`** — fixture wskazuje English UK; pigułka w headerze: `English UK`.
- **Pkt 13** — opisy zawijają się naturalnie, bez dzielenia słów; ręczne łamania wyłącznie jako wyjątki Lidera powyżej.

### Otoczka podglądu (poza produktem i capture)
- Pole „Uwagi do stanu” (zapis w przeglądarce, „Kopiuj (N)”, „Wyczyść”); pasek w 2 wierszach; lewa krawędź ramki osiągalna przy oknie węższym niż target.

### Chronione bez zmian
74 `state_id`, 5 targetów (`mac-wide` 1440×900), schema manifestu v3 (`id_migrations: []` — żadne publiczne ID nie zostało zastąpione), `element.product-work-surface` i kontrakt capture. Zachowane publiczne ID m.in.: `choose-variant-us`, `choose-variant-uk`, `welcome-subtitle`, `voice-list-scroll`, `voice-add-voices`, `voice-add-voices-toggle`, `voice-settings-instruction`, `open-voice-settings`, `voice-list-scroll-hint`, `voice-list-empty`, `profile-level-legend`, `profile-edit-cancel`, `chat-reply-error`, `chat-reply-retry`, `training-tutor-reply-error`, `training-tutor-reply-retry`, `variant-pill`. Usunięte: `profile-edit-save` (pkt 7 briefu). Fixture zmienione wyłącznie w stanach: `wizard-voice-unavailable-with-fallbacks`, `wizard-voice-unavailable-empty`, `wizard-voice-sample-error`, `chat-variant-switched`.

---

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC1` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC2` (zaakceptowana) · **Status:** kandydat do odbioru, niezaakceptowany.

## R11-RC1 (względem R10-RC2) — rozmiar targetu `mac-wide`

1. **`mac-wide`: 1440×932 → 1440×900.** `target_id` bez zmian; bez `id_migrations`, aliasów i kompatybilności wstecznej (`id_migrations: []`). Pozostałe targety bez zmian.
2. **Zaktualizowane deklaracje rozmiaru:** `prototype-manifest.json → targets[mac-wide].height`, lista targetów otoczki podglądu w `index.html` (przycisk profilu i ramka), lista profili w `catalog.html`, komentarze referencyjne w `index.html`, `responsive-spec.md` (tabela profili, tabela obszaru roboczego, wysokość `product-surface`), `README.md`. Rozmiar anchoru w capture wynika z `targets[]` bez dodatkowych zmian kodu.
3. **Fit i scroll na `mac-wide` (pomiar DOM 74 stanów, 932 vs 900, bez zrzutów):** żaden stan nie zaczął się przewijać ani nie wychodzi poza obszar roboczy; brak przewijania dokumentu. 15 stanów przewijało się wewnętrznie już przy 932 i nadal się przewija — ich obszar przewijania jest o 32 px niższy, reguły responsywności bez zmian: `characters`, `characters-returning` (`characters-catalog`); `chat-reply-correction`, `chat-reply-dialect`, `chat-reply-both`, `chat-new-conversation-confirm`, `chat-profile-popover` (`chat-messages`); `wizard-voice-f`, `wizard-voice-m`, `wizard-voice-sample-playing`, `wizard-voice-sample-error`, `wizard-voice-unavailable-with-fallbacks`, `wizard-voice-unavailable-empty`, `wizard-voice-change-only`, `wizard-voice-change-only-selected` (`voice-list-scroll`). Lista głosów (`voice-list-scroll`, 336 px) i pozostałe stany bez zmiany geometrii w pionie poza wysokością kolumny.
4. **Bez zmian:** treść, komponenty, CSS, 74 stany, `assets/fixtures.json` (bajtowo), publiczne ID, zachowanie UI, kontrakt URL.

---


- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC2` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1` (`product/ux/prototypes/macos/r10-rc1/prototype/`) · **Status:** kandydat do odbioru, niezaakceptowany.
- **Brief:** `VT-DESIGNER-BRIEF-005`, standard `CANONICAL_PROTOTYPE_PACKAGE_STANDARD-V3`.

## R10-RC2 (względem R10-RC1) — migracja techniczna do standardu v3, bez zmiany designu

1. **Manifest v3.** `prototype-manifest.json` ma dokładnie pięć kluczy: `schema_version: 3`, `revision_id`, `id_migrations: []`, `targets[]` (`target_id`, `width`, `height`; 5 targetów, wymiary bez zmian) i `states[]` (`state_id`, `fixture_id`; 74 stany i fixture bez zmian, bez `target_ids` — wszystkie 370 par dozwolone). Usunięte: `base_revision`, `product`, `entry`, `profiles`, `screens`, `components`, `instances`, `state_templates`, `fixtures`, `responsive_rules`, `interactions`. Informacja o ekranach, komponentach, elementach i interakcjach pozostaje w DOM (`data-screen-id`, `data-state-id`, `data-component-id`, `data-element-id`, `data-interaction-id`), `catalog.html` i `responsive-spec.md`.
2. **Migracje ID.** Brak nowych migracji (`id_migrations: []`). Historia 39 migracji z R5–R10-RC1 — sekcja „Historia migracji ID” na końcu pliku.
3. **`index.html` — capture.** `index.html?mode=capture&state=<state_id>&target=<target_id>`: wymaga obu parametrów; każdy inny parametr, nieznany stan, nieznany target i para spoza `states[].target_ids` (gdy podane) dają jawny błąd (`data-prototype-state="__error__"`). Target z `targets[]`, rozmiar `element.product-work-surface` = `width × height`; dane z `assets/fixtures.json` przez `states[].fixture_id`. Manifest z `schema_version` innym niż 3 jest odrzucany.
4. **`index.html` — podgląd.** `mode=user_preview` (i brak `mode`) przyjmuje opcjonalnie kompletną parę `state` + `target` i otwiera otoczkę z tą parą wybraną; bez obu — jak dotąd (`mac-wide`, `welcome`). Tylko jeden parametr z pary, nieznany stan lub target → jawny błąd. Pozostałe parametry (także `embed`, `profile`, `fixture`) są ignorowane. Wygląd otoczki bez zmian.
5. **Dane wbudowane w `index.html`** zamiast usuniętych sekcji manifestu: stan startowy podglądu (`welcome`) i ścieżka `assets/fixtures.json`. Mapowanie stan → ekran (etykiety listy w podglądzie) i lista targetów otoczki były już wbudowane.
6. **Dokumentacja.** `README.md` przepisany; `responsive-spec.md` i `catalog.html`: wyłącznie korekty odwołań do usuniętych sekcji manifestu i numer rewizji. Treść projektowa bez zmian.
7. **Poprawka po zwrocie odbioru (gotowość na anchorze).** W `mode=capture` atrybut `data-prototype-ready="true"` jest ustawiany na jedynym anchorze `element.product-work-surface` dopiero po zakończeniu renderu (wcześniej tylko na `body`, przez co selektor gotowości wspólnego renderera nie był spełniony). Przy każdym ponownym renderze i błędzie anchor wraca do `false`. Atrybut na `body` pozostaje pomocniczo. UI, stany, fixture, ID, targety i manifest v3 bez zmian.
8. **Bez zmian:** wygląd, zachowanie, treści, CSS, assety, ikony, `assets/fixtures.json` (bajtowo), 74 `state_id`, publiczne ID DOM, targety i wymiary.

---

# changelog — VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1` · **base_revision:** `VT-VIRTUALTUTOR-PROTOTYPE-R9-RC2 (local working copy)` · **Status:** kandydat do odbioru.
- **Identyfikacja bazy:** lokalna kopia robocza `VirtualTutor-V4/product/ux/prototype/r9-rc2/` (jej `checksums.sha256` 7/7 zgodne z plikami folderu). Zawiera późniejsze uzgodnione decyzje względem historycznego ZIP-a R9-RC2 (SHA-256 `1a2c41ea…0650e7`): brak profilu technicznego `mac-canonical` oraz pięć profili produktu `mac-wide`, `ios-iphone-15`, `android-pixel-8`, `android-galaxy-s20`, `ios-iphone-16-pro-max`. Historycznego ZIP-a nie użyto; `mac-canonical` nie został przywrócony ani zmigrowany.
- **Brief:** `VT-DESIGNER-BRIEF-004`, standard `CLD-PROTOTYPE-TECHNICAL-STANDARD-004`. **Towarzyszy:** `virtualtutor-ui-assets-r9-RC1.zip` (bez zmian).

## R10-RC1 (względem R9-RC2 local working copy) — migracja techniczna, bez zmiany wyglądu i zachowania

1. **Kontrakt URL.** Dwa jawne tryby:
   - `entry.capture_url_template` = `index.html?mode=capture&state={state_id}&target={target_id}` — czysty produkt, bez otoczki, paska stanu i selektora targetu. `target_id` = `profiles[].profile_id`. Układ i rozmiar `element.product-work-surface` wynikają z `capture_surface.logical_size` jawnego targetu, nie z okna przeglądarki. Animacje nieskończone zatrzymane w klatce `components[].motion.capture_frame_ms` przed sygnałem gotowości.
   - `entry.user_preview_url` = `index.html?mode=user_preview` — otoczka ręcznego podglądu. Wybór stanu i targetu wewnątrz otoczki; adres pozostaje stały. Ramka podglądu otwiera `mode=capture` i wznawia ruch animacji. Brak `mode` = `user_preview` (np. podgląd w aplikacji Claude, który nie przekazuje parametrów); jawny `mode` jest zawsze respektowany, capture ustawia `mode=capture`.
    - Pasek `preview-profile` (otoczka, `product_ui: false`): ukryte napisy `PREVIEW-PROFILE · PRODUCT_UI: FALSE` i `state=…` (identyfikatory zostają w atrybutach `data-*`); dodany link „Katalog komponentów” → `catalog.html`; wszystkie napisy paska powiększone o 30%. Produkt i klatki capture bez zmian.
    - Manifest: dodana techniczna instancja `element.product-work-surface` (`product_ui: false`, bez interakcji, 74 stany × 5 targetów, zgodna z `profiles[].capture_surface`). Format instancji bez zmian (`configuration`, `visibility.state_ids/profile_ids`). HTML i UI bez zmian.
    - Paczka: katalog główny ZIP-a = `prototype/` (wcześniej `r10-out/prototype/`).
   - Usunięte adresy legacy `?embed=1&state=…&profile=…`, parametr `profile` oraz przepisywanie adresu przez otoczkę. `embed`, `profile`, `fixture`, nieznany `mode` dają ekran diagnostyczny (`data-prototype-state="__error__"`); w `mode=capture` także brak lub nieznany `state` / `target`.
2. **Fixture jako dane.** `assets/fixtures.json` = `{ "fixtures": { "<fixture_id>": { "data": { … } } } }`, 74 fixture 1:1 z 74 stanami (bez deduplikacji). Dane to dokładnie te wartości, które w R9-RC2 budowały wyrażenia `builder` / `shared` (ekran, konfiguracja dialektu, kreator, rozmowa, trening, otwarte warstwy, tryb kompozytora, poziomy miernika), zapisane jako JSON, bez zmiany treści. Usunięte z pliku: `schema`, `revision_id`, `note`, `shared` (kod), `builder` (kod), `state_id` i `catalog_data` (te same dane są w `components[].examples`). Opis karty tłumaczenia (`translation`: `state`, `target`) trzech stanów tłumaczenia przeszedł do danych jako `translationPanel`; `index.html` czyta stan karty z danych zamiast z mapy po `state_id`. HTML po wyborze stanu czyta manifest → `states[].fixture_id` → `fixtures[].path` → `fixtures[fixture_id].data`. Builderów stanów nie ma już w `index.html`.
3. **Manifest w kształcie standardu** (`schema_version: "1.0"`):
   - `product` = `{ product_id, name }` (usunięte `status`, `language`).
   - `entry` = `path`, `catalog`, `responsive_spec`, `initial_state`, `capture_url_template`, `user_preview_url`, `ready_selector`. Usunięte `fixtures_file`, `url_contract`, `technical_ui`, `keyboard`, `transport`, `capture_surface`, `motion_capture`, `assets` (opis przeniesiony do README i `responsive-spec.md`; klawisz Escape pozostaje w `interactions[]`).
   - `screens[]` = `screen_id`, `title`, `selector`; `states[]` = `state_id`, `screen_id`, `description`, `fixture_id`, `selector` (usunięte `open_url`, `visibility`, `base_state_id`, `state_ids`).
   - `instances[]`: jeden kształt `screen_id`, `state_id`, `component_id`, `element_id`, `variant`, `configuration`, `visibility: { state_ids, profile_ids }`, `interaction_id` (także `null`), `product_ui`. Dawne formy `states`/`profiles: "all"`, `by_layout`/`profiles_by_layout`, `template_id`, `after_interaction`, `technical` zamienione na jawne listy stanów i targetów zgodne z renderem. Warunek z szablonu jest opisem w `configuration.template_condition`, pojawienie po interakcji w `configuration.appears_after_interaction`, element otoczki w `configuration.technical`, notatki w `configuration.note` / `visibility_note`.
   - `state_templates[]` = `template_id`, `parameters`, `instances` (+ opis). Usunięte `states` i `visibility_rules`, żeby widoczność miała jeden zapis — w `instances[]`.
   - `fixtures[]` = `{ fixture_id, path }` (usunięte `state_id`, `default`, `open_url`, `data`).
   - `responsive_rules` = `{ path, rules[] }`; progi klas i przypisanie targetów przeniesione do reguły `rr-layout-classes`.
   - `components[app-shell].configuration` = `?mode=capture|user_preview`, `?state`, `?target`; `preview-profile.configuration` = `target`, `state` (usunięte `?profile`, `?embed`, `?fixture`); opisy `ix-preview-profile` i `ix-preview-state-select` mówią o ramce `mode=capture`, nie o adresie otoczki.
   - Uzupełnienia wykryte pełną kontrolą R10 (bez zmiany renderu; braki odziedziczone z R9-RC2): `interactions[]` + `ix-chat-input-field` (pole wiadomości, `textbox`; instancja `chat-input-field` dostaje `interaction_id`) — 65 → 66 interakcji; `visibility.state_ids` karty tłumaczenia zgodne z renderem stanów fixture: `{{ it.trCardId }}` = `chat-reply-plain-translation-loading|ready|error`, `{{ it.trToggleId }}` i `{{ it.trDetailId }}` = `-ready`, `{{ it.trRetryId }}` = `-error`.
   - `components[].motion.capture` opisuje klatkę `mode=capture`; miernik pokazuje w capture dane fixture.
4. **Migracje ID profili** (`id_migrations[]`, wymiary, układ i wygląd bez zmian): `ios-iphone-15` → `iphone-16` (393×852), `android-pixel-8` → `pixel-8` (412×915), `android-galaxy-s20` → `galaxy-s20` (360×800), `ios-iphone-16-pro-max` → `iphone-16-pro-max` (440×956). `mac-wide` (1440×932) bez migracji. Pozostałe publiczne ID bez zmian.
5. **Wewnętrzny mechanizm `index.html`** (bez wpływu na render): każdy stan = stan początkowy komponentu + dane fixture (bez resztek poprzedniego stanu); usunięty alias `window.__vtDebug`. `window.VirtualTutorPrototype` pozostaje pomocniczym, niekontraktowym API.
6. **Dokumenty:** `catalog.html` (baner rewizji, URL, klatka capture, nazwy targetów), `responsive-spec.md` (target, anchor, URL), README z raportem kontroli.

## R9-RC2 (względem R9-RC1) — bez zmiany wyglądu i zachowania (historia)

Zmiany techniczne, konieczne do przejścia pełnej kontroli 370/370:

1. **Manifest — listy widoczności zgodne z renderem.** Render się nie zmienił; manifest pomijał stany, w których elementy są widoczne:
   - `app-shell`, `product-surface`, `top-header`, `back-button`, `app-wordmark`, `variant-pill`, `voice-change-entry`, `chat-secondary-header`, `chat-tutor-identity`, `chat-header-avatar`, `chat-tutor-name`, `chat-tutor-meta`, `learner-profile-summary`, `new-conversation-button`, `chat-finish`, `chat-messages`, `text-composer`, `composer-hint`, `{{ it.id }}`, `{{ it.actionsId }}`, `{{ it.replayId }}`, `{{ it.trActionId }}`: `visibility.states` + `chat-reply-plain-translation-loading`, `-ready`, `-error`.
   - `voice-confirm`: + `wizard-voice-change-only`.
   - `state_templates[tpl-tutor-reply].states` + `chat-ad-active`, trzy stany tłumaczenia, `variant-popover-open` (18 → 23); `state_templates[tpl-composer].states` + stany odpowiedzi, dialogów i menu rozmowy (30 → 41).
   - `new-conv-confirm-title`: `by_layout.mobile` / `compact` = `chat-new-conversation-confirm` (tytuł jest w popoverze także na telefonie).
   - `tutor-tile-{{ char.id }}-last-used-badge`, `continue-conversation-{{ char.id }}`: `by_layout.mobile` / `compact` = tylko `characters-returning` (w `characters` — pierwsza wizyta — nie ma ostatnio używanego tutora na żadnym profilu).
2. **Bez zbędnych żądań przy starcie.** Nierozwiązane szablony adresów były pobierane jako pliki: `{{ shellSrc }}` (ramka powłoki) i `{{ char.photoSrc }}` / `{{ selChar.photoSrc }}` (portrety), a `image-slot.js` odczytywał nieistniejący `.image-slots.state.json`. Poprawki: `loading="lazy"` na ramce powłoki; w osadzonym `image-slot.js` pominięcie `src` zawierającego `{{` i brak odczytu sidecara. Portrety i powłoka wyglądają i działają jak w R9-RC1.
3. **Identyfikatory rewizji** w manifeście, `fixtures.json`, `index.html`, `catalog.html`, `responsive-spec.md`, README.

## R9-RC1 (względem R8-RC2) — zmienia widoczne UI (historia)

1. **Reklama (`chat-ad-active`, bez nowego ekranu).** `advertisement-screen[fullscreen]`: warstwa na cały obszar roboczy, bez `top-header` i rozmowy; wiersz „REKLAMA” (lewo) / „Zamknij reklamę” (prawo, sam tekst, `primary-cta[text]`), pod nim neutralny slot od krawędzi do krawędzi. Zachowanie opisane w `components[advertisement-screen].behavior`: próg co 10. pełnej tury (globalny, z treningiem), zapis i licznik przed reklamą, tylko „Zamknij reklamę” i `Escape` (nowy klawisz), fokus uwięziony na przycisku, powrót w to samo miejsce bez zmiany danych, TTS po zamknięciu, bez odliczania. Poprawka: fixture `fx-chat-ad-active` odwoływał się do nieistniejącego `AD_CONFIG.firstAfterTurns` (licznik `undefined`); teraz `AD_CONFIG.everyTurns` (10).
2. **Komponenty dynamiczne.** Nowe `components[]`: `recording-indicator`, `processing-indicator`, `typing-indicator`, `voice-preview-indicator` (33 zamiast 29) z ID, wariantami, konfiguracją, `motion` (klatki, czas, krzywa, opóźnienia, `capture_frame_ms`, plik Lottie) i `examples` (sekwencje danych). `audio-level-meter`: warianty danych `silence` / `quiet` / `normal` / `clipping`, reguła wysokości, próbki; przesterowanie (≥ 40 px) `#bd413f`. Miernik odtwarza deterministyczne próbki zamiast losowych.
3. **Nowe ID instancji (6):** `mic-check-recording-indicator`, `speech-check-recording-indicator`, `composer-recording-indicator`, `composer-processing-indicator`, `chat-typing-dots`, `voice-option-{{ v.id }}-preview-indicator`. Istniejące ID bez zmian; brak migracji.
4. **Capture.** `VirtualTutorPrototype.setMotionFrame('capture' | ms | null)`: zatrzymuje animacje w jawnej klatce komponentu i ustawia próbkę miernika. Coverage (zrzuty) nie wykonano — decyzja właściciela produktu z 27.09.2026; patrz README.
5. **Ikony.** Wszystkie znaki Unicode i emoji w UI produktu (←, ▾, ⌄, ⌃, ✓, ▶, ■, 🎙, ⚠, ＋, ◎, 文, 🌐, ↻, ✎, ◆, △, ~, !, 🇺🇸, 🇬🇧) zastąpione własnymi ikonami SVG `vt-*`, osadzonymi inline w `index.html` i `catalog.html`. Pliki, licencja projektu i reguły: `virtualtutor-ui-assets-r9-RC1.zip`.
6. **Usunięte wejście ekranu** (`vt-fade` 0,4–0,5 s na powitaniu, wyborze tutora, kreatorze i bramce wariantu).
7. **Dane katalogowe** (nie stany): długie imię, długa odpowiedź tutora, długa nazwa głosu — `components[].examples`, `assets/fixtures.json → catalog_data`, `catalog.html` §9. Stanów nadal 74, fixture 74 (1:1).
8. **Portrety Tutorów:** bez zmian wyglądu; w `entry.assets` i `ASSETS.md`: źródło „generowanie AI dla VirtualTutor”, licencja projektu, zastosowanie „portrety Tutorów”.
9. **Dokumentacja:** `responsive-spec.md` §5.13 (reklama), §5.16 (ruch), §5.17 (ikony), §8; `catalog.html` §9 (komponenty dynamiczne, warianty miernika, dane, ikony) i nowy wpis `advertisement-screen`.

## R8-RC2 (względem R8-RC1) — historia

Tylko dowód visual capture. UI, zachowanie, stany, fixture, komponenty, interakcje, publiczne ID i `capture_surface` bez zmian.

- Nowe coverage: 370 zrzutów (74 stany × 5 profili), każdy dokładnie w granicy `element.product-work-surface` i o rozmiarze `capture_surface.logical_size`. Osobne archiwum w dwóch częściach, każda z pełnym `index.csv` i `checksums.sha256`.
- `README.md`: raport kontroli uzupełniony o wynik coverage 370/370 oraz o sumy kontrolne archiwów.
- Pozostałe pliki: zmieniony wyłącznie identyfikator rewizji (`revision_id` / `base_revision` w manifeście, `fixtures.json`, banner `catalog.html`, komentarz `index.html`, nagłówek `responsive-spec.md`).
- Coverage R7-RC4 przestaje być dowodem odbioru.

## R8-RC1 (względem R7-RC5)

Dostosowanie do wspólnego standardu prototypów i visual capture. To nie jest zmiana projektu UI: widoczne UI, zachowanie, profile, stany, fixture, komponenty, instancje, interakcje i publiczne ID pozostają jak w R7-RC5.

- `profiles[]`: każdy z 5 profili `role: product` ma `capture_surface` z `element_id: "element.product-work-surface"`, selektorem `[data-element-id="element.product-work-surface"]` i `logical_size`: `mac-wide` 1440×932, `ios-iphone-15` 393×852, `android-pixel-8` 412×915, `android-galaxy-s20` 360×800, `ios-iphone-16-pro-max` 440×956.
- `entry.capture_surface`: opis anchoru (nieinteraktywny, `product_ui: false`, jeden na stan, poza `components[]`, `instances[]` i `interactions[]`).
- `index.html`: w dokumencie produktu (`?embed=1`) dochodzi jeden element `data-element-id="element.product-work-surface"` (`position: fixed; inset: 0`, przezroczysty, `pointer-events: none`, `aria-hidden="true"`). Bez wpływu na układ.
- `README.md`: poprawiony nieaktualny opis fixture tłumaczenia (każde z trzech należy do własnego stanu, zgodnie z manifestem od R7-RC4). Nowy raport kontroli anchoru.
- `responsive-spec.md`: nowa §1.1 z rozmiarem powierzchni roboczej osobno dla każdego profilu. §4 i §8 wskazują anchor jako obszar porównania.
- Liczby bez zmian: 74 stanów · 30 ekranów · 29 komponentów · 195 instancji · 65 interakcji · 74 fixture · 35 migracji ID. Brak nowych migracji ID.
- Coverage: bez nowych zrzutów. Archiwum coverage R7-RC4 pokazuje niezmienione UI, ale jego kadr ma szerokość profilu i wysokość ok. 540 px, a nie `capture_surface.logical_size`. Nie jest więc zrzutem powierzchni roboczej w rozumieniu nowego standardu. Porównanie z aplikacją wykonuje się na kadrze anchoru (patrz README, raport kontroli).

## R7-RC5 — historia

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5` · **Status:** zastąpiona przez R8-RC1.
- **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R6-RC3`, ZIP `virtualtutor-prototype-r6-RC3.zip`, SHA-256 `1d2b32cf1f252ed9db353cb9373642534990c623d31cdb57618244c9014e2f27`.
- **Źródła:** `VT-DESIGNER-BRIEF-003` (z poprawioną tabelą `replay-button`), `VT-DESIGNER-QUESTIONS-003` (odpowiedzi Lidera A1–A4, B1–B8), `CANONICAL_PROTOTYPE_PACKAGE_STANDARD.md`. Status decyzji prowadzi Lider w rejestrze decyzji.
- **Zakres:** migracja kontraktu paczki. Wygląd, copy, podróże, zachowanie przycisków, semantyka mikrofonu, dostępność i geometria responsywna pozostają jak w RC3.

### R7-RC5 (względem R7-RC4)

Poprawka dokumentacji w `catalog.html`. UI, zachowanie, stany, fixture, ID i coverage bez zmian.

- `app-shell` i `preview-profile`: wejście opisane jako `?state=<state_id>` + opcjonalny `&profile=<profile_id>`. `embed=1` to techniczny tryb kadru produktu. Usunięto aktywny opis parametru `?fixture=`.
- `translation-card`: zamiast przykładu `?state=chat-idle&fixture=translation-error` katalog wskazuje trzy jawne stany `chat-reply-plain-translation-loading`, `-ready`, `-error`.
- W `catalog.html` nie ma już żadnego wystąpienia `?fixture=` / `&fixture=`. Wzmianki o parametrze `fixture` w starszych sekcjach tego changelogu i w `id_migrations[]` są historyczne (stan sprzed R7-RC4).
- Coverage: bez nowych zrzutów. Obowiązuje archiwum coverage R7-RC4 (SHA-256 `1510f50724674247b282e58d5e6b3190aecf420e6b5313c8a56cc4b9aba47abc`), bo UI i zachowanie się nie zmieniły.

## Zmienione

1. **Układ paczki:** dokładnie `prototype/` z 8 plikami kanonicznymi. Katalogi `assets/fonts` i `assets/tutors` oraz pliki `support.js`, `image-slot.js`, `VALIDATION.md` i `coverage/` nie wchodzą do ZIP-a.
2. **Zasoby wbudowane (A1–A2):** fonty (Plus Jakarta Sans, Lora, Lora Italic), 12 portretów oraz skrypty runtime prototypu są w `index.html` i `catalog.html` jako `data:` URI.
3. **React i ReactDOM wbudowane.** Przy migracji wykryłem, że runtime RC3 ładował React 18.3.1 i ReactDOM 18.3.1 z `unpkg.com` w czasie działania. RC3 nie działał więc w pełni offline, a kontrola RC3 tego nie wykryła, bo sprawdzała wyłącznie odwołania w plikach HTML. W R7 obie biblioteki są w paczce. Pomiar `performance.getEntriesByType('resource')`: 0 żądań poza `localhost`.
4. **Portrety przeskalowane z 1254×1254 do 384×384 px**, bo po wbudowaniu `index.html` przekraczał limit 20 MB na plik (34 MB). Największy rozmiar wyświetlania to 124 px CSS, więc 384 px pokrywa DPR 3. Regresja wizualna: `welcome` na mac-wide różni się od RC3 o 0,37% pikseli (miękkość portretów). Pozostałe ekrany bez różnic powyżej 0,3%.
5. **Manifest kanoniczny:** 15 sekcji w kolejności standardu. Pola RC3 (`elements`, `visibility_rules`, `url_contract`, `layout_classes`, `technical_ui`, `keyboard`, `removed_states`, `id_migrations_r6`, `element_id_migrations` i inne) przeniesione do sekcji kanonicznych. Bez drugiego formatu i bez adaptera.
6. **Stan → `fixture_id` (B1):** [stan R7-RC1; od R7-RC2: 71 stanów, 74 fixture] każdy z 72 stanów ma jedno główne `fixture_id` (`fx-<state_id>`). Dane fixture (wyrażenia budujące stan + wspólne definicje) są w `assets/fixtures.json`. Parametr `?fixture=` przyjmuje wyłącznie fixture zadeklarowaną dla danego stanu; każda inna kombinacja otwiera ekran diagnostyczny. Na `product-surface` dodano `data-fixture-id`.
7. **Wzorce list (B2):** 44 instancje z `configuration.pattern: true`, `pattern_parameter` i jawną listą `pattern_values`. Jedna interakcja na wzorzec.
8. **Mikrofon kompozytora (B3):** wpis RC3 `{{ micButtonTestId }}` zastąpiony dwiema instancjami i dwoma kontraktami: `composer-mic-button` (start / anuluj przetwarzanie — stan R7-RC1; od R7-RC2 processing jest nieaktywny, bez anulowania) i `composer-stop-recording` (stop i rozpoznaj). ID w HTML bez zmian.
9. **„Odsłuchaj” (B4):** kontrakt `replay`: kliknięcie w trakcie odtwarzania odtwarza wypowiedź ponownie od początku. Zachowanie RC3 bez zmian.
10. **Powłoka podglądu (B6):** komponent techniczny `preview-profile` (`product_ui: false`) z instancjami `preview-profile` i `preview-state-select`, opisany też w `entry.technical_ui` i `entry.url_contract`.
11. **Szablon błędu (B7):** `tpl-system-error` z parametrem `presentation: panel | inline-card`, bez zmiany wyglądu.
12. **API prototypu:** `VirtualTutorPrototype.setState(stateId, fixtureId?)`, nowe `listFixtures(stateId)` i `getFixture()`.
13. **Katalog:** sekcja „Katalog = components[] manifestu R7”. 20 wpisów obejmuje wszystkie 29 `component_id`.

## Dodane ID

- **Fixture:** `fx-<state_id>` × 72 (stan R7-RC1; od R7-RC2: 71 + 3 nakładki = 74).
- **Szablony stanów:** `tpl-system-error`, `tpl-profile-question`, `tpl-tutor-reply`, `tpl-composer`, `tpl-bottom-sheet` (nieużywany od R7-RC2).
- **Reguły responsywne:** `rr-*` × 14.
- **Interakcje:** `interaction_id` `ix-*` × 62.
- **Instancje obecne w HTML RC3, pominięte w manifeście RC3** (HTML bez zmian): `preview-state-select`, `voice-add-voices`, `voice-list-empty`, `voice-sample-status`, `chat-tutor-name`, `profile-level-legend`.

## Zmienione ID (`id_migrations[]`, revision = R7-RC1)

| from | to | reason |
| --- | --- | --- |
| `translation-loading` (fixture) | `fx-chat-reply-plain-translation-loading` | B1 |
| `translation-ready` (fixture) | `fx-chat-reply-plain-translation-ready` | B1 |
| `translation-error` (fixture) | `fx-chat-reply-plain-translation-error` | B1 |
| `{{ micButtonTestId }}` (wpis manifestu RC3) | `composer-mic-button` + `composer-stop-recording` | B3 |

Pozostałe wpisy `id_migrations[]` (20) to scalone listy migracji z R5.13, R5.x i R6. Publiczne `screen_id`, `state_id`, `component_id` i `element_id` z HTML RC3 są niezmienione.

## Usunięte ID (w R7-RC1)

W R7-RC1 nie usunięto żadnego stanu, ekranu, komponentu ani elementu HTML. Usunięcia z R7-RC2 opisuje sekcja „Usunięte i połączone ID w R7-RC2” niżej.

## Liczby względem kryteriów migracji (B5) — do odbioru

- **Instancje: 192 zamiast 185** = 185 z manifestu RC3 − 1 (`{{ micButtonTestId }}`) + 2 (B3) + 6 elementów HTML, które manifest RC3 pomijał. Pełna zgodność HTML ↔ manifest: 0 rozbieżności.
- **Interakcje: 62 zamiast 59** = 59 z RC3 + 1 (podział B3) + 2 techniczne powłoki (`preview-state-select`, `preview-profile`, `product_ui: false`). **Interakcji produktowych jest 60.** Liczby 59 nie da się zachować przy jednoczesnym spełnieniu B3 i reguły „każda interaktywna instancja wskazuje jeden wpis `interactions[]`”.
- **Instancje nieosiągalne w zadeklarowanych stanach** (zachowane dla stabilności ID, opisane w `visibility.note`): `voice-sample-status`, `voice-sample-error` (martwy kod od RC2) oraz `voice-list-empty` (brak fixture z pustą listą głosów w RC3).


## VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 — zmiany po przeglądzie użytkownika (26.09)

Źródło: uwagi z przeglądu prototypu przekazane 26.09. Status decyzji prowadzi Lider w rejestrze decyzji. Każda pozycja poniżej zmienia widoczne UI lub zachowanie względem RC3/R7-RC1, więc wymaga odbioru.

### Mikrofon: jeden element, cztery stany
1. **Jeden przycisk mikrofonu z tekstem wszędzie** (kompozytor rozmowy, treningu, pytań profilu i testy onboardingu): pigułka z ikoną w okręgu przy lewej krawędzi i wyśrodkowanym napisem. Stany: gotowy „🎙 Rozpocznij nagrywanie” (turkus) · nagrywanie „■ Zakończ nagrywanie” (czerwony, pulsujący pierścień) · rozpoznawanie „Rozpoznaję mowę” (jasna pigułka, ciemnoturkusowy okrąg z falującymi kreskami, **nieaktywny**) · wyłączony (wyszarzony). Powód: jeden wygląd dla tej samej czynności; tekst mówi, co się stanie.
2. **„Nagraj ponownie”** ma ten sam wygląd co „Rozpocznij nagrywanie” (wcześniej przezroczysty przycisk z obrysem).
3. **Brak „Anuluj rozpoznawanie” i ✕.** Aplikacja nie ma akcji anulowania rozpoznawania.
4. **Usunięte z kompozytora:** napis „Słucham” (`composer-recording-status`), miernik poziomu (`composer-meter`) i status z kropkami „Rozpoznaję mowę…” (`composer-processing-status`). Stan pokazuje sam przycisk. Miernik zostaje w testach onboardingu.
5. **Podpowiedź przy rozpoznawaniu:** „Zamieniam Twoją wypowiedź na tekst…”.
6. **Stała wysokość kompozytora** we wszystkich stanach (Mac 91 px; telefon: rozmowa i trening 210 px, pytania profilu 162 px). Wcześniej rozpoznawanie było niższe, bo znikała podpowiedź.
7. **Telefon:** przycisk mikrofonu obok „Wyślij” ma szerokość według treści (siatka `minmax(0, max-content) 1fr auto`, bez stałych szerokości, napis zawija się przy długich tłumaczeniach); pełna szerokość tylko dla przycisku, który stoi w wierszu sam. Na telefonie nagrywanie i rozpoznawanie są wyśrodkowane w pionie w obszarze kompozytora.

### Rozmowa
8. **Stan `chat-sending` połączony z `chat-typing`.** Jedno zapytanie HTTP do modelu: od kliknięcia „Wyślij” wiadomość ucznia jest od razu w rozmowie, a tutor „pisze” do czasu odpowiedzi. Stany: 72 → 71. Usunięte: `chat-sending`, `chat-sending-status`, `fx-chat-sending`.
9. **Menu i potwierdzenia na telefonie otwierają się pod wyzwalaczem** (wariant języka, profil ucznia, „Wyczyść rozmowę”, „Zakończ rozmowę”) zamiast dolnego arkusza z przyciemnieniem. Zamykanie: ponowny klik wyzwalacza, Escape, wybór opcji, „Anuluj”. Przyciski w menu i potwierdzeniach mają 44 px na telefonie. Powód: palec jest przy wyzwalaczu u góry ekranu. Instancje arkusza (`variant-sheet-scrim`, `variant-sheet-close`, `sheet-scrim`, `sheet-close`, `{{ c6.sheetId }}`) zostają w HTML jako nieosiągalne, ID zachowane.
10. **Potwierdzenie „Zakończyć rozmowę z …?” ma opis:** „Historia tej rozmowy zostanie zapisana i w dowolnym momencie możesz do niej wrócić.” (nowy `finish-conv-confirm-body`). W aplikacji tego tekstu nie ma, więc to nowa treść do wdrożenia.
11. **Zmiana głosu z rozmowy:** lista otwiera się z zaznaczonym bieżącym głosem i widocznym „Ten głos mi odpowiada” (także w `wizard-voice-change-only`).
12. **Poprawka przyciemnienia:** menu wariantu na telefonie było pod przyciemnieniem (stare `z-index`). Błąd był już w RC3.

### Trening (wersja 1c)
13. **Nagłówek treningu bez zielonego tła** (ciepła biel, jak w rozmowie) z plakietką **TRENING** (`training-mode-badge`) zamiast „Correcting:”.
14. **Przypięta karta „ĆWICZYSZ ZDANIE”** pod nagłówkiem, ze zdaniem do przećwiczenia (`training-task-sentence`, dawniej `training-header-correction`) i „Odsłuchaj wzór” (`training-task-replay`).
15. **Tło sceny treningu** `#f3f9f1`, zielona ramka pola i podpowiedź w pustym polu: „Powiedz albo wpisz zdanie…”. Zmienia to wymaganie z briefu R6 („zielony drugi nagłówek”).

### Pozostałe
16. `characters-returning` (telefon): plakietka „OSTATNIO” była ucięta od góry; teraz stoi w całości w karcie nad portretem.
17. `variant-gate` i błędy uprawnień (telefon): „Zostań przy English US” i „Sprawdź ponownie” mają pełną szerokość, jak przycisk główny.
18. **Powłoka podglądu** (`product_ui: false`): przyciski ‹ / › i klawisze ← / → do przechodzenia po stanach (`preview-state-prev`, `preview-state-next`, `preview-state-counter`). Pasek ma 2 wiersze.
19. **Katalog:** wpis `mic-control` pokazuje wyłącznie ostateczne wersje (5 pigułek); przykład kompozytora zgodny z ekranami.

### Liczby R7-RC2
71 stanów · 30 ekranów · 29 komponentów · 195 instancji · 65 interakcji (61 produktowych + 4 techniczne) · 74 fixture · 31 migracji ID.


## VT-VIRTUALTUTOR-PROTOTYPE-R7-RC3 — poprawka dowodów odbioru (bez zmian UI i zachowania)

Źródło: uwagi Lidera do odbioru R7-RC2 z 26.09. Decyzje produktowe z RC2 bez zmian.

### Usunięte i połączone ID w R7-RC2 (zapis skonsolidowany)

| from | to | rodzaj | powód |
| --- | --- | --- | --- |
| `chat-sending` (state_id) | `chat-typing` | stan połączony | Jedno zapytanie HTTP do modelu: od kliknięcia „Wyślij” jedno oczekiwanie (tutor pisze). Stany: 72 → 71. |
| `chat-sending` (screen_id) | `chat-typing` | ekran połączony | Jak wyżej. Ekrany: 31 → 30. |
| `fx-chat-sending` (fixture_id) | — | fixture usunięta | Fixture usuniętego stanu. Fixture: 75 → 74. |
| `chat-sending-status` (element_id) | — | element usunięty | Napis „Wysyłanie wypowiedzi” należał wyłącznie do usuniętego stanu. Nie miał kontraktu interakcji. |
| `composer-processing-status` | `composer-mic-button[data-state=processing]` | element zastąpiony | Napis „Rozpoznaję mowę” jest w przycisku mikrofonu. |
| `composer-recording-status` | — | element usunięty | „Słucham” usunięte; nagrywanie pokazuje „Zakończ nagrywanie”. |
| `composer-meter` | — | element usunięty | Miernik usunięty z kompozytora (zostaje w onboardingu). |
| `training-header-correction` | `training-task-sentence` | element przeniesiony | Zdanie w przypiętej karcie zadania (tryb treningu 1c). |

Wszystkie te wpisy są też w `prototype-manifest.json → id_migrations[]` z `revision: VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2`. Pozostałe usunięte stany, ekrany, komponenty ani kontrakty interakcji nie występują.

### Zmiany w R7-RC3
1. **Rewizja** `VT-VIRTUALTUTOR-PROTOTYPE-R7-RC3` w README, manifeście, changelogu, `index.html` i `catalog.html`. Treść UI i zachowanie są identyczne z R7-RC2.
2. **Nowe, pełne coverage RC3** poza kanonicznym ZIP-em: 71 stanów × 5 profili = 355 zrzutów (`virtualtutor-prototype-r7-RC3-coverage.zip`). Zastępuje coverage RC1 jako dowód wizualny.
3. **Przegląd ręczny** wykonany i opisany w README → „Raport kontroli”.
4. Ten skonsolidowany zapis migracji `chat-sending` usuwa sprzeczność z sekcją „Usunięte ID” z R7-RC1.


## VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5 — ujednolicenie dokumentacji i jawne stany tłumaczenia (bez zmian UI)

Źródło: uwagi Lidera do RC3 z 26.09 oraz decyzja B1. Status decyzji prowadzi Lider w rejestrze decyzji.

### Stany i fixture
| from | to | powód |
| --- | --- | --- |
| `fx-chat-reply-plain-translation-loading` (nakładka na `chat-reply-plain`, `&fixture=`) | stan `chat-reply-plain-translation-loading` + ta sama `fixture_id` | B1: każdy stan używany w odbiorze otwieralny przez `?state=` |
| `fx-chat-reply-plain-translation-ready` (nakładka) | stan `chat-reply-plain-translation-ready` + ta sama `fixture_id` | jw. |
| `fx-chat-reply-plain-translation-error` (nakładka) | stan `chat-reply-plain-translation-error` + ta sama `fixture_id` | jw. |
| parametr URL `fixture` | usunięty; obecność → ekran diagnostyczny | jeden sposób wyboru stanu |

Stany: 71 → 74 (nowe są stanami ekranu `chat-reply`, nie osobnymi ekranami; `chat-reply-plain` pozostaje stanem bez otwartego tłumaczenia). Fixture: 74, po jednym na stan. Ekrany: 30. `fixture_id` nie zmieniły się. Wpisy są w `id_migrations[]` z `revision: VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5`. API: `setState(stateId)`; opcjonalny drugi argument przyjmuje wyłącznie własne fixture stanu.

### Dokumentacja ujednolicona z faktycznym RC3
1. **Menu i potwierdzenia:** we wszystkich klasach popover pod wyzwalaczem (spec §5.4, §5.5, `rr-overlays`, komponenty `anchored-menu` i `confirmation-dialog`, skutki `variant-pill`, `learner-profile-summary`, `new-conversation-button`, katalog). Szablon `tpl-bottom-sheet` i elementy `*-sheet-*` opisane jako nieużywane od R7-RC2 (ID zachowane).
2. **„Rozpoznaję mowę”:** nieaktywny, bez ✕ i bez anulowania (spec §5.7, §5.14, `mic-control`, katalog). Nazwa dostępna `composer-stop-recording` = „Zakończ nagrywanie”.
3. **Liczby i URL:** 74 stany, 74 fixture, 30 ekranów; `?state=` bez `fixture` (README, manifest `url_contract`, komentarz `index.html`).
4. **Opisy odziedziczone z RC1/RC2 usunięte lub oznaczone jako historyczne:** `Więcej` i `conversation-more` (katalog), zielony nagłówek treningu `#e8f4e6` (spec §5.2), miernik i „Słucham” w kompozytorze, `chat-sending` w warunkach zajętości i skutku „Wyślij”, arkusze w safe area i promieniach, liczby 72/75 i anulowanie w sekcjach R7-RC1 changelogu (adnotacje „stan R7-RC1”).
5. **Narzędzia podglądu:** `technical_ui` obejmuje `preview-state-prev`, `preview-state-next`, `preview-state-counter`.

## Historia migracji ID (R5–R10-RC1)

Przeniesione z `id_migrations[]` manifestu R10-RC1. Manifest v3 zawiera wyłącznie migracje bieżącej rewizji.

| from | to | rewizja | powód |
| --- | --- | --- | --- |
| `progress` | `conversation-complete` | R5.13 | semantic_correction: Stan oznacza zakończenie rozmowy, nie wyliczony postęp. Brak aliasu renderującego stary stan. |
| `wizard-voice-f-missing` | `wizard-voice-unavailable-with-fallbacks` | R5.13 | screen_consolidation: Jeden ekran wizard-voice-unavailable; roznica to fixture fallbackVoices. |
| `wizard-voice-handoff` | `wizard-voice-unavailable-empty` | R5.13 | screen_consolidation: Jak wyzej; pusty fixture nie renderuje listy. |
| `voice-error-missing` | `voice-unavailable-icon` | R5.x | migracja elementu R5.x |
| `voice-settings-handoff` | `voice-unavailable-icon` | R5.x | migracja elementu R5.x |
| `voice-missing-heading` | `voice-unavailable-heading` | R5.x | migracja elementu R5.x |
| `voice-settings-handoff-heading` | `voice-unavailable-heading` | R5.x | migracja elementu R5.x |
| `voice-missing-body` | `voice-unavailable-description` | R5.x | migracja elementu R5.x |
| `voice-settings-handoff-body` | `voice-unavailable-description` | R5.x | migracja elementu R5.x |
| `conversation-complete (state_id)` | `null` | R6 | usunięty; po potwierdzeniu zakończenia → characters-select |
| `conversation-complete-*` | `null` | R6 | R6: zmiana semantyki lub usunięcie |
| `header-progress, header-progress-dot-1..4` | `null` | R6 | R6: zmiana semantyki lub usunięcie |
| `debug-state-picker, debug-state-select` | `preview-state-select (powłoka, product_ui:false)` | R6 | R6: zmiana semantyki lub usunięcie |
| `mic-icon` | `mic-check-stop[data-state=recording]` | R6 | R6: zmiana semantyki lub usunięcie |
| `speech-icon` | `speech-check-stop[data-state=recording]` | R6 | R6: zmiana semantyki lub usunięcie |
| `voice-sample-status` | `voice-option-<id>-preview (voice-preview-state)` | R6 | R6: zmiana semantyki lub usunięcie |
| `voice-download-more-link` | `voice-add-voices-toggle + open-voice-settings` | R6 | R6: zmiana semantyki lub usunięcie |
| `correction-wrong` | `null` | R6 | R6: zmiana semantyki lub usunięcie |
| `welcome-wordmark` | `null` | R6 | duplikat app-wordmark |
| `conversation-more (tylko wcześniejszy kandydat R6)` | `new-conversation-button + chat-finish widoczne bezpośrednio na mobile` | R6 | R6: zmiana semantyki lub usunięcie |
| `translation-loading (fixture)` | `fx-chat-reply-plain-translation-loading` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC1 | B1: każda kombinacja stanu i nakładki ma własne fixture_id |
| `translation-ready (fixture)` | `fx-chat-reply-plain-translation-ready` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC1 | B1 |
| `translation-error (fixture)` | `fx-chat-reply-plain-translation-error` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC1 | B1 |
| `{{ micButtonTestId }} (wpis manifestu RC3)` | `composer-mic-button + composer-stop-recording` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC1 | B3: dwa publiczne ID mikrofonu kompozytora, dwa kontrakty interakcji; ID w HTML bez zmian |
| `chat-sending` | `chat-typing` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Jedno zapytanie HTTP do modelu: od kliknięcia Wyślij do odpowiedzi trwa jedno oczekiwanie (tutor pisze); aplikacja nie ma zdarzenia rozdzielającego „wysyłanie” od „pisania”. |
| `chat-sending-status` | `null` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Usunięty razem ze stanem chat-sending (napis „Wysyłanie wypowiedzi”). |
| `fx-chat-sending` | `null` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Fixture usuniętego stanu chat-sending. |
| `composer-processing-status` | `composer-mic-button[data-state=processing]` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Napis „Rozpoznaję mowę” jest w przycisku mikrofonu (nieaktywny w processing); bez osobnego statusu z kropkami. |
| `training-header-correction` | `training-task-sentence` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Tryb treningu 1c: zdanie przeniesione z nagłówka do przypiętej karty zadania; w nagłówku plakietka TRENING (training-mode-badge). |
| `composer-recording-status` | `null` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Napis „Słucham” usunięty; stan nagrywania pokazuje przycisk „Zakończ nagrywanie”. |
| `composer-meter` | `null` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC2 | Miernik poziomu usunięty z kompozytora (zostaje w testach onboardingu). |
| `fx-chat-reply-plain-translation-loading (nakładka na chat-reply-plain, &fixture=)` | `chat-reply-plain-translation-loading (state_id) + fx-chat-reply-plain-translation-loading` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5 | Decyzja B1 (po RC3): każdy stan używany w odbiorze otwieralny przez ?state=; jedno fixture na stan. fixture_id bez zmian. |
| `fx-chat-reply-plain-translation-ready (nakładka na chat-reply-plain, &fixture=)` | `chat-reply-plain-translation-ready (state_id) + fx-chat-reply-plain-translation-ready` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5 | Decyzja B1 (po RC3): każdy stan używany w odbiorze otwieralny przez ?state=; jedno fixture na stan. fixture_id bez zmian. |
| `fx-chat-reply-plain-translation-error (nakładka na chat-reply-plain, &fixture=)` | `chat-reply-plain-translation-error (state_id) + fx-chat-reply-plain-translation-error` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5 | Decyzja B1 (po RC3): każdy stan używany w odbiorze otwieralny przez ?state=; jedno fixture na stan. fixture_id bez zmian. |
| `parametr URL fixture` | `null` | VT-VIRTUALTUTOR-PROTOTYPE-R7-RC5 | Usunięty; stan wybiera wyłącznie ?state=<state_id>. |
| `ios-iphone-16-pro-max` | `iphone-16-pro-max` | VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1 | profile_rename: profile_id = target_id nazywa konkretne urządzenie używane do capture; wymiary, układ i wygląd bez zmian. |
| `ios-iphone-15` | `iphone-16` | VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1 | profile_rename: profile_id = target_id nazywa konkretne urządzenie używane do capture; wymiary, układ i wygląd bez zmian. |
| `android-pixel-8` | `pixel-8` | VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1 | profile_rename: profile_id = target_id nazywa konkretne urządzenie używane do capture; wymiary, układ i wygląd bez zmian. |
| `android-galaxy-s20` | `galaxy-s20` | VT-VIRTUALTUTOR-PROTOTYPE-R10-RC1 | profile_rename: profile_id = target_id nazywa konkretne urządzenie używane do capture; wymiary, układ i wygląd bez zmian. |