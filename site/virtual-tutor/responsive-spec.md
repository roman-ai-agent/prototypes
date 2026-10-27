# VirtualTutor V4 — responsive-spec R10 (R11-RC10)

- **Rewizja:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC10` · **Baza:** `VT-VIRTUALTUTOR-PROTOTYPE-R11-RC9` (geometria targetów bez zmian; RC7: §5.2 meta, §5.7 licznik, §5.8 szerokości; RC8: §5.18 tryb i temat w kompozytorze; RC9: §5.19 dzienny czas rozmowy; RC10: §5.20 strona WWW `landing`) · **Status:** kandydat do odbioru.
- **Target (R10):** `target_id` = `targets[].target_id`. W `mode=capture` klasa układu (§2) wynika z `targets[].width` jawnego targetu, nie z okna przeglądarki; środowisko capture ustawia viewport na `targets[].width × height` targetu.
- **Zakres R9:** (1) `advertisement-screen` pełnoekranowy (§5.13); (2) komponenty dynamiczne z regułami ruchu i klatką capture (§5.16); (3) własne ikony SVG zamiast znaków Unicode/emoji (§5.17); (4) usunięte wejście ekranu (fade 0,4–0,5 s). Pozostała geometria, kolejność, widoczność i minima bez zmian względem R8-RC2. `capture_surface` bez zmian (§1.1).
- **Targety:** `targets[]` manifestu zawiera pięć targetów produktu (`target_id`, `width`, `height`).
- **Towarzyszy:** `catalog.html` (katalog komponentów, `data-component-id`) i `index.html`. `preview-profile` (otoczka, tylko w `mode=user_preview`) przełącza target ramki podglądu.

Wartości w px to px CSS przy DPR 1. Wartość bez zmiany w kolumnie mobile oznacza „jak wide”.

---

> **Stan faktyczny (od R7-RC2, bez zmian UI w R7-RC3/RC4; R7-RC4 dodaje 3 jawne stany tłumaczenia `chat-reply-plain-translation-*` ekranu `chat-reply`):** na wszystkich klasach `anchored-menu` i `confirmation-dialog` są popoverami pod wyzwalaczem (na telefonie szerokość kolumny, przyciski 44 px); dolnego arkusza i przyciemnienia nie ma. `text-composer`: jeden `mic-control[labeled]` we wszystkich stanach, stała wysokość (wide 91 px; telefon: czat i trening 210 px, profil 162 px). `processing` = nieaktywny przycisk „Rozpoznaję mowę”, bez ✕ i bez anulowania. Trening: nagłówek bez zielonego tła, plakietka `TRENING`, przypięta karta zadania, tło sceny `#f3f9f1`.

## 1. Profile

| `profile_id` | Viewport | Klasa układu | Rola |
| --- | ---: | --- | --- |
| `mac-wide` | 1440×900 | `wide` | `role: product`. Wymagany profil odbioru. |
| `iphone-16` | 393×852 | `mobile` | `role: product`. Wymagany. |
| `pixel-8` | 412×915 | `mobile` | `role: product`. Wymagany. |
| `galaxy-s20` | 360×800 | `compact` | `role: product`. Wymagany. |
| `iphone-16-pro-max` | 440×956 | `mobile` | `role: product`. Wymagany. |

### 1.1 Powierzchnia robocza produktu (`capture_surface`)

Przedmiotem porównania aplikacji z prototypem jest powierzchnia robocza produktu, a nie całe okno, pasek debug, selektor profilu ani chrome hosta. Każdy profil produktu ma jeden stały rozmiar logiczny tej powierzchni (px CSS, DPR 1). Aplikacja ustawia swój odpowiadający obszar (AX identifier `element.product-work-surface`) na identyczny rozmiar.

| `target_id` | Klasa | `width × height` (szer. × wys.) | Co obejmuje |
| --- | --- | ---: | --- |
| `mac-wide` | `wide` | **1440 × 900** | Cały obszar roboczy okna produktu. Tło `#fffcf9` od krawędzi do krawędzi, w nim wyśrodkowana kolumna `product-surface` 1180 px o wysokości 900 px. Pasek tytułu okna macOS i chrome hosta nie wchodzą. |
| `iphone-16` | `mobile` | **393 × 852** | Cały ekran aplikacji; `product-surface` = 393 × 852. Safe area jest wewnątrz powierzchni (w profilu przeglądarkowym 0). |
| `pixel-8` | `mobile` | **412 × 915** | Cały ekran aplikacji; `product-surface` = 412 × 915. |
| `galaxy-s20` | `compact` | **360 × 800** | Cały ekran aplikacji; `product-surface` = 360 × 800. |
| `iphone-16-pro-max` | `mobile` | **440 × 956** | Cały ekran aplikacji; `product-surface` = 440 × 956 (kolumna treści 440 px, poniżej progu 560 px). |

- **Anchor:** `element.product-work-surface`, selektor `[data-element-id="element.product-work-surface"]`. W każdym z 85 stanów (`index.html?mode=capture&state={state_id}&target={target_id}`) jest dokładnie jeden. Ma `position: fixed; top: 0; left: 0` i szerokość oraz wysokość ustawione z `targets[].width × height` targetu. Jest przezroczysty, bez ramki, z `pointer-events: none` i `aria-hidden="true"`. Po zakończeniu renderu ma `data-prototype-ready="true"`. Nie przyjmuje fokusu i nie ma treści ani akcji. Nie jest komponentem ani elementem UI produktu, więc nie ma go w katalogu komponentów. Jego rozmiar wynika z `targets[]` manifestu v3.
- Anchor nie zmienia układu ani wyglądu (jest poza przepływem, bez tła). `product-surface` pozostaje selektorem stanu (`data-screen-id`, `data-state-id`, `data-fixture-id`).
- W `mode=user_preview` anchor jest wewnątrz ramki produktu (`mode=capture`) o rozmiarze targetu. `preview-profile` i pasek wyboru stanu leżą poza nim.

## 2. Klasy układu (reguły według szerokości)

| Klasa | Szerokość viewportu | Kolumna treści | Poziomy margines treści |
| --- | --- | --- | --- |
| `wide` | ≥ 900 px | `product-surface` max 1180 px, wyśrodkowany | 48 px (nagłówki, historia, kompozytor) |
| `mobile` | 375–899 px | 100% do 560 px, powyżej wyśrodkowana kolumna 560 px | 20 px |
| `compact` | ≤ 374 px | 100% | 16 px |

**Uzasadnienie progów.** 900 px zostaje z briefu: poniżej tej szerokości nie mieszczą się w jednym wierszu nagłówek rozmowy z trzema pigułkami (profil ~230 px, `Wyczyść rozmowę`, `Zakończ rozmowę`) ani siatka 3 kart tutorów. 374/375 px oddziela 360 px od 393 px. Wcięcie 24 px kart pomocniczych (F4) i trzy linie kompozytora (E5) zostawiają na 360 px zbyt mało miejsca na układ `mobile`. Wyśrodkowanie do 560 px od 440 px zapobiega rozciąganiu dymków i przycisków na tabletach i szerokich oknach mobilnych.

Wszystkie klasy mają tę samą podróż użytkownika, te same stany, `state_id` i `element_id`. Klasa zmienia tylko `data-variant` layoutu komponentu.

## 3. Reguły globalne

1. **Bez skalowania.** Nie ma `transform: scale`, `zoom` ani przeliczania rozmiaru czcionek względem viewportu. Zmiany typografii wymienia wyłącznie §6.
2. **Bez obcinania.** Etykiety przycisków i pigułek nie mają `ellipsis`, `nowrap` z `overflow: hidden` ani stałej szerokości. Za brak miejsca odpowiada kontener: zawija, przenosi do kolejnego wiersza albo do `anchored-menu` (E1).
3. **Bez ukrywania treści wymaganej.** Treść może zmienić pozycję, ale nie znika. (Etykieta `Krok n z m` usunięta w R11-RC4 — nie występuje w żadnej klasie.)
4. **Cele dotykowe na mobile i compact** mają co najmniej 44×44 px. Jeśli element jest wizualnie mniejszy (`replay-button` 25 px, `pill-button` 32 px), obszar trafienia rozszerza się niewidoczną ramką. Sąsiednie obszary trafienia nie mogą się nakładać, więc odstęp między nimi wynosi co najmniej 8 px.
5. **Tło.** `product-surface` = `#fffcf9` od krawędzi do krawędzi we wszystkich klasach. Nie ma szarego tła, karty, ramki, cienia ani rogów (brief §6.1).
6. **Scroll.** Każdy ekran ma jeden pionowy obszar przewijania. Nagłówki i kompozytor są `sticky` (§5). Na mobile nigdzie nie ma poziomego scrolla.
7. **Safe area** (mobile/compact). Górny nagłówek dostaje `padding-top: env(safe-area-inset-top)`, a kompozytor `padding-bottom: env(safe-area-inset-bottom)`. W profilach przeglądarkowych wartość wynosi 0.

## 4. `app-shell`, `product-surface`, `preview-profile`

| Element | `wide` | `mobile` | `compact` |
| --- | --- | --- | --- |
| `app-shell` | Viewport 100%×100%. Tło = `#fffcf9`. | jw. | jw. |
| `product-surface` | Szerokość `min(100%, 1180px)`, wyśrodkowany, wysokość = viewport. | 100%. Treść w kolumnie `min(100%, 560px)`. | 100% |
| `preview-profile` | Pod ekranem, poza `product-surface`, `product_ui: false`. Zmienia wyłącznie szerokość i wysokość ramki ekranu. | jw. | jw. |

`preview-profile` nigdy nie wchodzi do capture ekranu produktu. Obszar porównania to `[data-element-id="element.product-work-surface"]` o rozmiarze z §1.1. Stan identyfikuje `[data-component-id="product-surface"]`.

## 5. Komponenty zmieniające układ

Kolumny: **kolumny/szerokości · kolejność/pozycja · widoczność/wariant · scroll/sticky · minima**.

### 5.1 `top-header`
| Oś | `wide` | `mobile` / `compact` |
| --- | --- | --- |
| Kolumny | Trzy strefy 110 px \| auto \| 110 px: `Wstecz`, wordmark, prawa strefa. | Trzy strefy auto \| 1fr \| auto, wysokość 56 px. |
| Kolejność | Wiersz pigułek konwersacji (wariant, głos) wyśrodkowany pod wordmarkiem. | Bez zmian. Wiersz pigułek zawija się (`flex-wrap`, gap 8). |
| Widoczność | Prawa strefa pusta we wszystkich ekranach (bez etykiety kroku (brak kropek postępu, brief §6.2). Wariant `advertisement` nie ma wiersza pigułek. | Bez zmian. |
| Scroll | `sticky` top. | `sticky` top. |
| Minima | — | `Wstecz` ma zawsze ikonę i tekst. Cel dotykowy 44 px. |

### 5.2 `secondary-conversation-header` i `training-header`
| Oś | `wide` | `mobile` | `compact` |
| --- | --- | --- | --- |
| Kolumny | Jeden wiersz, wysokość 75 px: `tutor-identity-block` (1fr) \| pigułki (auto, gap 10). | Dwa wiersze: tożsamość (avatar 36), potem pigułki. | jw. |
| Kolejność | Profil → `Wyczyść rozmowę` → `Zakończ rozmowę`. | Wiersz 2: profil (`state`). Wiersz 3: `Wyczyść rozmowę` i `Zakończ rozmowę` bezpośrednio (zamiast `Więcej`; wymaganie przekazane 26.09). | jw., pigułki zawijają się. |
| Widoczność | `training`: tło nagłówka jak rozmowa (`#fffcf9`), plakietka `TRENING` przy nazwie tutora, zdanie w przypiętej karcie zadania pod nagłówkiem (`training-task-sentence`), tylko profil i `Zakończ trening` (`training-finish`), bez `Wyczyść rozmowę`. `profile-edit`: bez pigułek. | `training`: `Zakończ trening` zostaje widoczną pigułką (E1). | jw. |
| Scroll | `sticky` pod `top-header`. | jw. | jw. |
| Minima | Meta = opis perspektywy z kafelka (RC7, VT-CHAT-HEADER-PERSPECTIVE-001): do 2 linii z wielokropkiem, 12,5 px `#50565e`; wysokość 75 px. | jw. Zdanie treningu w karcie zadania zawsze w całości. | jw. |

### 5.3 `pill-button`
Geometria we wszystkich klasach: wysokość 32 px, padding 6/14, promień 24, 12.5/600, szerokość wynika z treści. Na mobile i compact obszar trafienia ma 44 px wysokości. Nie ma wariantu mobilnego: zmienia się wyłącznie kontener (§5.2).

### 5.4 `anchored-menu` (wariant, profil)
| Oś | `wide` | `mobile` / `compact` |
| --- | --- | --- |
| Pozycja | Popover zakotwiczony 6 px pod wyzwalaczem, prawa krawędź = prawa krawędź wyzwalacza (profil 266 px, wariant min 220 px). | Popover bezpośrednio pod wyzwalaczem, szerokość kolumny (wariant: pod pigułką w `top-header`; profil: 6 px pod pigułką profilu). Bez arkusza i bez przyciemnienia. |
| Scroll | Bez scrolla. | Bez scrolla wewnętrznego na profilach docelowych. |
| Minima | Wiersze opcji 40 px. | Wiersze opcji i przyciski 44 px. |

Zamykanie w obu układach: ponowny klik wyzwalacza, wybór opcji, `Esc`. Fokus wraca do wyzwalacza. Elementy `variant-sheet-*` / `sheet-*` z R6 nie renderują się w żadnym stanie (ID zachowane).

### 5.5 `confirmation-dialog`
Popover pod wyzwalaczem we wszystkich klasach. Wide: 326 px przy `Zakończ rozmowę` / `Wyczyść rozmowę`, akcje wyrównane do prawej. Mobile/compact: szerokość kolumny, 8 px pod wierszem akcji, przyciski 44 px. Oba dialogi mają tytuł i opis. Zamykanie: `Anuluj`, ponowny klik wyzwalacza, `Esc`. Dialog pokazuje się natychmiast po kliknięciu, niezależnie od TTS. W treningu dialogu nie ma (C6).

### 5.6 `primary-cta`
Wide: szerokość z treści, padding 16/32. Warianty `primary`, `secondary` (stop testu obsługuje `mic-control[labeled]`, §5.14). Mobile/compact: szerokość 100% kolumny, min. wysokość 52 px, etykieta zawija się, wyśrodkowana. Usunięto `white-space: nowrap` z R5.17, bo np. `Sprawdź rozpoznawanie mowy — English UK` na 360 px wymaga zawinięcia.

### 5.7 `text-composer` (`chat`, `training`, `profile`)
| Oś | `wide` | `mobile` / `compact` |
| --- | --- | --- |
| Kolumny | Jeden wiersz: `mic-control[labeled]` (szerokość z treści: „Rozpocznij nagrywanie” / „Zakończ nagrywanie” / „Rozpoznaję mowę” / „Nagraj ponownie”) \| pole 1fr \| `Wyślij`. Wysokość całego kompozytora 91 px. | Czat i trening: pole (3 linie) nad wierszem akcji; wiersz akcji = siatka `minmax(0, max-content) 1fr auto` (mikrofon z treści \| odstęp \| `Wyślij`). Profil: pole 1 linia. |
| Kolejność | Mikrofon → pole → wyślij. | Pole → [mikrofon \| `Wyślij`] → podpowiedź. W `recording` pole i `Wyślij` znikają, a `Zakończ nagrywanie` stoi sam, na pełną szerokość, wyśrodkowany w pionie. W `processing` tak samo nieaktywny „Rozpoznaję mowę” i podpowiedź „Zamieniam Twoją wypowiedź na tekst…”. Bez miernika, bez „Słucham”, bez ✕. |
| Widoczność | Podpowiedź pod wierszem, 12.5 px. `Wyczyść` w polu, gdy jest tekst. | Podpowiedź pod wierszem B (wszystkie klasy mobile). |
| Scroll | Tekst dłuższy niż pole przewija się wewnątrz pola. | Pole przewija się wewnętrznie po 3 liniach (E5). |
| Sticky | Dół `product-surface`. | `sticky` bottom z safe area. |
| Minima | Wysokość stała we wszystkich trybach (91 px). | Wysokość stała we wszystkich trybach: czat i trening 210 px, profil 162 px (idle, recording, processing, transcribed, error). |

**`char-counter` (RC7, VT-MESSAGE-LENGTH-LIMIT-001):** we wszystkich klasach po prawej w wierszu podpowiedzi (`margin-left: auto`, 12,5 px, cyfry tabelaryczne), tylko w rozmowie głównej i tylko gdy w polu jest tekst. Przekroczenie 9 000 znaków: licznik 700 `#bd413f`, obramowanie pola `#bd413f`, `composer-length-error` w osobnym wierszu pod polem (wysokość kompozytora rośnie o ten wiersz), `Wyślij` wyłączony.

Wariant `profile`: **jeden wiersz** w stanach idle / recording / processing (mikrofon → pole/status → wyślij); w transcribed / error na mobile pole w pełnym wierszu, pod nim `Nagraj ponownie` + `Wyślij`, pole 52 px na mobile, pod spodem podpowiedź i `Anuluj` (tylko edycja).

### 5.8 Strumień rozmowy: `tutor-bubble`, `learner-bubble`, karty pomocnicze
| Oś | `wide` | `mobile` | `compact` |
| --- | --- | --- | --- |
| Szerokość tury tutora | RC7 (VT-CHAT-BUBBLE-WIDTH-001): 80% szerokości historii, dymek wypełnia blok. Pytania profilu: 70%, maks. 748 px | 80%. Pytania profilu: 96% | jw. |
| `learner-bubble` | Maks. 80% historii, do prawej (odpowiedzi profilu: 748 px) | Maks. 80% (profil: 85%) | Maks. 80% (profil: 88%) |
| Karty pomocnicze | Start 24 px od lewej krawędzi dymka źródłowego, koniec na jego prawej krawędzi, odstęp 10 px (wide) / 8 px (mobile). | jw. | jw. — 24 px bez zmian (F4) |
| Kolejność pod jedną odpowiedzią | `translation-card` → `A small grammar correction` / `A more natural way to say it` → `Dialect difference` (F1) | jw. | jw. |
| `Please note` | Karta pod swoim dymkiem powitania. Może mieć własną `translation-card` (F1). | jw. | jw. |
| `tutor-utterance-actions` | Wiersz w dymku, do prawej. | jw. Na compact, jeśli się nie mieści, zawija się do 2 wierszy do prawej. | jw. |

Na 360 px: kolumna 328 px → tura 262 px → karta pomocnicza 238 px (RC7). Przy tej szerokości `A small correction` mieści obie akcje (`Odsłuchaj komunikat korekty` 196 px, `Potrenuj z tutorem` 148 px) tylko w dwóch wierszach. Wiersz akcji zawija się, wyrównany do prawej.

### 5.9 `tutor-card` (ekran `characters-select`)
| Oś | `wide` | `mobile` / `compact` |
| --- | --- | --- |
| Kolumny | Siatka 3 × `minmax(0,1fr)`, gap 20, maks. 1000 px. | 1 kolumna, gap 12. |
| Wariant | `tile`: pionowo, avatar 120 px. | `row`: avatar 72 px po lewej, nazwa/opis/tag po prawej, CTA na pełną szerokość pod spodem. |
| Kolejność | Fixture `characters-returning`: ostatnio używany pierwszy, z plakietką `OSTATNIO`. | jw. |
| Minima | Opis: `min-height` = 3 linie (4.5em), bez `line-clamp`. Wiersz siatki wyrównuje wysokość kart (`align-items: stretch`), CTA przyklejone do dołu (`margin-top: auto`). | CTA 48 px. Opis zawija się bez limitu linii, bez rezerwy (każda karta to osobny wiersz). |

### 5.10 `onboarding-stepper`
Wide: 6 segmentów (znacznik 18 px + etykieta 11.5/700) w wierszu z zawijaniem, gap 14. **Mobile i compact:** 6 znaczników 18 px połączonych linią 2 px, pod nimi jedna linia podpisu z etykietą bieżącego etapu (bez `Krok n z 6`). Odpowiedź E4 dotyczyła compact. Rozszerzam to na całe mobile, bo pełna lista etykiet (ok. 540 px) nie mieści się także w 393–440 px. **Do akceptacji.**

### 5.11 `system-check-panel`, `audio-level-meter`
Panel: jedna kolumna wyśrodkowana, tekst wyśrodkowany, maks. szerokość treści 54ch (wide) / 100% (mobile). Nagłówek 21 → 20 px (compact). Start/stop testu: `mic-control[labeled]` (§5.14). Miernik: 20 słupków stale; szerokość słupka 5 px (wide, mobile) / 4 px (compact); gap 4. Ten sam komponent obsługuje wariant `error` (B6).

### 5.12 `voice-selector`, `voice-option`
Viewport listy ma **368 px wysokości we wszystkich klasach** (E3). Szerokość: wide `min(100%, 560px)`, mobile/compact 100% kolumny. Odtwarzanie i błąd próbki zmieniają tylko wiersz (`voice-preview-state`), nigdy wysokość listy. Wiersz `voice-option` ma min. 56 px (wide) / 60 px (mobile). Na compact nazwa zawija się, a `voice-preview-state` zostaje po prawej. Pierwszym zwykłym elementem listy (bez `sticky`) jest `voice-download-callout` (`voice-add-voices`): domyślnie rozwinięty, gdy brak głosu wybranego wariantu, zwinięty, gdy pasujący głos istnieje; rozwinięcie przesuwa głosy w dół. Ilustracje kroków 2 i 3: `width: 100%; max-width: 560px; height: auto`. Podpowiedź przewijania pod listą także wtedy, gdy scroll wynika z rozwiniętego calloutu.

### 5.13 `advertisement-screen` (R9: pełny ekran)
Wszystkie klasy tak samo. Warstwa zajmuje cały obszar roboczy (`element.product-work-surface`, §1.1) i przykrywa wszystko: nie ma `top-header` (znak VIRTUALTUTOR, `Wstecz`, pigułki) ani rozmowy.

| Oś | Wszystkie klasy |
| --- | --- |
| Kolumny | Wiersz 48 px: `REKLAMA` (11/700, tracking .1em, `#6d7178`) po lewej \| `Zamknij reklamę` po prawej. Pod nim slot `1fr`. |
| Pozycja | Margines poziomy wiersza = margines treści (wide 48, mobile 20, compact 16 px). Na telefonie wiersz pod `safe-area-inset-top`. Slot od krawędzi do krawędzi, bez proporcji i bez zaokrągleń. |
| Wariant | `fullscreen`. `Zamknij reklamę` = `primary-cta[text]`: sam tekst 14/600 `#286265`, bez tła; hover podkreślenie, fokus obrys 2 px. |
| Scroll | Brak. |
| Minima | Cel dotykowy „Zamknij reklamę” 44 px wysokości. |

Zachowanie: reklama pojawia się po zapisaniu odpowiedzi tutora kończącej co 10. pełną turę; licznik globalny, liczą się też tury treningu; zapis i zwiększenie licznika przed pokazaniem. Dostępne wyłącznie „Zamknij reklamę” i `Escape`; fokus startuje na przycisku i nie wychodzi poza niego. Zamknięcie natychmiast wraca do tej samej rozmowy albo treningu w tym samym miejscu, bez zmiany danych i licznika, bez nowej odpowiedzi; wypowiedź tutora odtwarza się dopiero po zamknięciu. Brak odliczania. Slot to neutralny placeholder `#f5f1ec` bez kreacji.

### 5.14 `mic-control`
Jedna pigułka z etykietą (`labeled`) we wszystkich kontekstach: kompozytor, test mikrofonu, test STT. Skutki różne (patrz `data-interaction-id` w DOM i `catalog.html`). Zwykły `button`, bez `aria-pressed`. Ikona w okręgu przy lewej krawędzi, napis wyśrodkowany w pozostałej części. Szerokość: z treści, gdy obok stoi inny przycisk; 100% kolumny tylko, gdy przycisk stoi w wierszu sam (np. `Zakończ nagrywanie`, testy onboardingu na mobile). Etykieta się zawija. Stany: `ready` (teal 🎙) · `recording` (czerwony ■ z pulsującym pierścieniem) · `processing` („Rozpoznaję mowę”, jasne neutralne tło, trzy falujące kreski w okręgu; **nieaktywny**: disabled, aria-busy; bez ✕ i bez akcji anulowania) · `disabled`. `Nagraj ponownie` ma wygląd `ready`. Testy zachowują ID `mic-check-start/stop` i `speech-check-start/stop` oraz miernik w onboardingu; w kompozytorze miernika nie ma.

### 5.15 `profile-level`
Wybór poziomu: wide 4 opcje w siatce 2×2 (maks. 560 px), mobile/compact 1 kolumna, opcje 56 px.

### 5.16 Komponenty dynamiczne (R9)
Ruch w prototypie = CSS; ten sam ruch jako Lottie w `virtualtutor-ui-assets-r9-RC1.zip → animations/` (60 fps, próbkowane z CSS). Tylko elementy wspierane przez pakiet `lottie` we Flutterze (macOS, web, Android, iOS): kształty, skala, obrót, przesunięcie, krycie, kolor, klatki kluczowe; bez wyrażeń, efektów, masek i tekstu. Wymiary jak w prototypie we wszystkich klasach.

| Komponent | Gdzie | Ruch | Klatka capture |
| --- | --- | --- | --- |
| `recording-indicator` | okrąg 52 px w `mic-control[recording]` (kompozytor, test mikrofonu, test STT) | `vt-ring` 1200 ms ease-out: pierścienie 0→12 px (krycie ,75→0) i 0→22 px (,35→0) w 0–60 % | 300 ms (Lottie 18/72) |
| `processing-indicator` | okrąg 52 px w `mic-control[processing]` | `vt-wave` 1000 ms ease-in-out: scaleY ,35→1→,35; opóźnienia 0/150/300 ms | 500 ms (30/60) |
| `typing-indicator` | kropki w `tutor-bubble[typing]` | `vt-dot` 1200 ms ease: krycie ,3→1, y 0→−2 px w 40 %; 0/150/300 ms | 500 ms (30/72) |
| `voice-preview-indicator` | słupki „Odtwarzam” w `voice-option` | `vt-dot` 900 ms ease; 0/200/400 ms | 450 ms (27/54) |
| `audio-level-meter` | tylko onboarding | bez animacji w pliku: wysokość = 6 + poziom × 34 px co 150 ms; ≥ 40 px = przesterowanie `#bd413f` | próbka `normal` (`quiet` w stanie „zbyt cicho”) |

W `mode=capture` prototyp zatrzymuje ruch w tych klatkach przed sygnałem gotowości; miernik pokazuje poziomy z danych fixture stanu. Szczegóły i sekwencje danych: `catalog.html` §9 (dane ruchu wbudowane w `index.html`).

### 5.17 Ikony (R9)
Znaki Unicode i emoji (←, ▾, ⌄, ⌃, ✓, ▶, ■, 🎙, ⚠, ＋, ◎, 文, 🌐, ↻, ✎, ◆, △, ~, !, 🇺🇸, 🇬🇧) zastąpione własnymi ikonami SVG `vt-*` (siatka 24, obrys 2 px, `currentColor`; flagi 24×16). Rozmiary jak dotychczasowe glify (10–26 px, diagnostyka 38 px), kolor z kontekstu. Zmiana wyglądu wyłącznie w obrębie glifu; układ i odstępy bez zmian. Pliki i reguły: `virtualtutor-ui-assets-r9-RC1.zip`, `ASSETS.md`.

### 5.18 `conversation-focus-bar` (wiersz w kompozytorze), `conversation-steer-button`, `anchored-menu[mode-menu]` (RC8, brief 008)
Wiersz jest pierwszym elementem `composer-dock`, nad polem wiadomości. Tylko rozmowa główna; brak w treningu, pytaniach o profil i edycji profilu. Zostaje widoczny (disabled), gdy pole jest ukryte (oczekiwanie na tutora, błąd odpowiedzi). Nie przewija się z historią.

| Oś | `wide` | `mobile` | `compact` |
| --- | --- | --- | --- |
| Kolumny | Siatka `auto minmax(0,1fr) auto`: pigułka \| temat \| „Zmień temat”; gap 14. Wysokość wiersza 36 px (temat 1–2 wiersze). | Siatka `auto 1fr`, 2 wiersze: pigułka · „Zmień temat” (do prawej) / temat na całą szerokość; gap 10/6. Wysokość ok. 36 + 6 + 34 px. | jw. |
| Pigułka (A1) | min. 36 px, padding 4/10/5/14, promień 14: „chcę” 10,5/600 `#6d7178` nad nazwą trybu 13/700 małą literą + chevron 12 px. Szara `#f5f1ec`/`#e3ddd5`; otwarta lista: `#e0f3f4`/`#c2dedf`, chevron obrócony. Nieznany: „tryb” / „jeszcze nieznany” 500 `#6d7178`. | jw. | jw. |
| Temat | 12/1.4 `#50565e`, EN, do 2 wierszy z wielokropkiem; > 260 znaków — tekst klikalny (rozwiń/zwiń). | do 2 wierszy z wielokropkiem; > 110 znaków — tekst klikalny. | jw. |
| Lista trybów | Popover 340 px nad pigułką (`bottom: 100%`, 6 px), lewa krawędź = lewa krawędź pigułki; wiersze 44 px; nakłada się na historię. | Popover na szerokość kolumny (margines 20 px) nad kompozytorem; wiersze 52 px. | margines 16 px; wiersze 52 px. |
| Disabled | krycie 0,55, bez otwierania listy — warunki jak „Wyślij” (`catalog.html#conversation-focus-bar`). | jw. | jw. |
| Minima | „Zmień temat” 32 px wysokości; obszar trafienia pigułki i linku 44 px na telefonie (§3.4). | jw. | jw. |

### 5.19 `time-left-indicator`, `limit-notice` (RC9, brief 009)
Rozmowa główna i trening (`chat-*`, `training-*`); brak w pytaniach o profil i edycji profilu. Bez stałych szerokości i wysokości — teksty zawijają się (aplikacja docelowo wielojęzyczna).

| Oś | `wide` | `mobile` | `compact` |
| --- | --- | --- | --- |
| Wskaźnik — położenie | Wiersz pod polem: podpowiedź po lewej, grupa `composer-status` (`margin-left:auto`, flex-wrap, gap 4/14) po prawej: wskaźnik, potem licznik znaków. Zwykle 1 linia. | Grupa zawija się pod podpowiedź, do prawej. +ok. 18 px wysokości kompozytora, gdy podpowiedź zajmuje całą szerokość. | jw. |
| Wskaźnik — pole ukryte | Oczekiwanie na tutora, błąd odpowiedzi: wskaźnik sam w wierszu pod wierszem trybu, do prawej. | jw. | jw. |
| Wskaźnik — typografia | 12,5/1.4, cyfry tabelaryczne; `low`: 700 `#8a4b00` + ikona 13 px, gap 5. | jw. | jw. |
| Komunikat — położenie | Pierwszy element `composer-dock` (nad wierszem trybu i polem), pełna szerokość; padding 12/16, promień 14, ikona 34 px. Tekst 14/1.5, zwykle 1 linia. | 1–3 linie. Kompozytor rośnie o wysokość komunikatu + 12 px; historia odpowiednio niższa. | jw. |
| Kompozytor przy blokadzie | Układ bez zmian (§5.7); pole `#f8f5f1` / ramka `#e3ddd5`, mikrofon szary (`#f5f1ec`, tekst `#6d7178`), „Wyślij” krycie 0,5; podpowiedź ukryta. | jw. | jw. |

### 5.20 Strona WWW — ekran `landing`, stan `landing-default` (RC10, brief 010)
Strona nie korzysta z klas układu §2 ani z atrybutów profilu. Jedynym wejściem jest **szerokość kontenera** `#vt-landing` (`container: vtl / inline-size`, reguły `@container vtl (max-width: …)`): w zakładce — szerokość powierzchni targetu, po publikacji — szerokość okna. Progi: **> 1000 px** szeroki, **601–1000 px** pośredni, **≤ 600 px** telefon. Strona przewija się w pionie wewnątrz `product-surface`; `element.product-work-surface` ma rozmiar targetu. Kolumna: `min(1180px, 100% − 48px)`, telefon `100% − 32px`. Bez poziomego przewijania na żadnym profilu.

| Sekcja / komponent | `mac-wide` 1440 (> 1000) | 601–1000 | `iphone-16` 393 · `pixel-8` 412 · `galaxy-s20` 360 · `iphone-16-pro-max` 440 (≤ 600) |
| --- | --- | --- | --- |
| `landing-nav` | sticky 64 px; marka, Tutorzy, Co potrafi, Jak zacząć, „Pobierz” | marka + „Pobierz” | 60 px; marka + „Pobierz” |
| `landing-hero` | wyśrodkowany; wachlarz 7 + 5 portretów 118 px; h1 `clamp(38px, 4.6cqi, 64px)` (64 px); przyciski obok siebie 56 px | portrety 88 px | portrety 52 px; h1 33/1.12; przyciski jeden pod drugim na pełną szerokość |
| `landing-app-preview` + `landing-showcase-captions` | okno Maca max 1040 px na przejściu tła w `#1f5053`; pełny ekran rozmowy (powitanie, uczeń, tutor, tłumaczenie, korekta, wiersz trybu, mikrofon, pole, „Wyślij”); objaśnienia 1–4 w 4 kolumnach | padding okna 24 px; akcje rozmowy pod nazwą tutora; objaśnienia w 2 kolumnach | układ mobilny aplikacji w ramce telefonu (max 420 px): bez paska okna, powitania i tłumaczenia; pole nad mikrofonem i „Wyślij”; objaśnienia w 1 kolumnie |
| `landing-tutor-card` | 4 kolumny, portret 112 px, opis i styl | 3 kolumny | 2 kolumny; portret 76 px; imię + styl (bez opisu) |
| `landing-feature-card` | 3 kolumny; grafika 176 px | 1 kolumna | grafika 150 px; fala skrócona do 12 słupków |
| `landing-install-step` | obok nagłówka (0.8fr / 1.2fr) | pod nagłówkiem | numer 36 px; ścieżka zawija się |
| `landing-platform-tile` | 3 kolumny, wyrównanie do góry | macOS na całą szerokość, „Wkrótce” 2 × | 1 kolumna; parametry jeden pod drugim |
| `landing-privacy` | 3 kolumny | 1 kolumna | 1 kolumna |
| `landing-footer` | 1 wiersz | zawija się | jeden pod drugim |
| Sekcje | padding 96 px, `scroll-margin-top` 64 px | jw. | 64 px, 60 px |

Wysokość strony (sprawdzona, z przykładami chromu): 1440 → 6392 px; 393 → 8428 px; 412 → 8277 px; 360 → 8780 px; 440 → 8129 px. Tryb ciemny: brak. Ruch: tylko przejścia hover przycisków (wyłączone przy `prefers-reduced-motion`).

## 6. Typografia zmieniana responsywnie

| Styl | `wide` | `mobile` | `compact` |
| --- | ---: | ---: | ---: |
| Hero (`welcome-headline`) | 42 | 30 | 28 |
| Tytuł ekranu | 28 | 24 | 22 |
| Tytuł panelu | 21 | 20 | 20 |
| Tekst dymka | 15/1.55 | 15/1.55 | 15/1.55 |
| Pozostałe | bez zmian | bez zmian | bez zmian |

## 7. Macierz ekranów → komponenty zmieniające układ

| Rodzina (brief §5) | Zmienia układ przez |
| --- | --- |
| `welcome` | typografia §6, `primary-cta` §5.6 |
| `characters-select` | `tutor-card` §5.9 |
| `wizard-*` (wariant, mic, STT, błędy, podsumowanie) | `top-header` §5.1, `onboarding-stepper` §5.10, `system-check-panel` §5.11, `primary-cta` §5.6 |
| `wizard-voice-select`, `wizard-voice-change`, `wizard-voice-unavailable` | `voice-selector` §5.12, `primary-cta` |
| `variant-gate` | `system-check-panel`, `primary-cta` |
| `profile-ask-*`, `profile-edit-*` | strumień §5.8, `text-composer[profile]` §5.7 |
| `profile-level`, `profile-edit-level` | §5.15 |
| `chat-*`, `training-*` | `top-header`, `secondary-conversation-header` / `training-header` §5.2, `conversation-focus-bar` w kompozytorze §5.18 (tylko `chat-*`, nie `training-*`), `time-left-indicator` / `limit-notice` §5.19, strumień §5.8, `text-composer` §5.7 |
| `chat-mode-menu` (`chat-mode-menu-open`) | `anchored-menu[mode-menu]` §5.18 |
| `chat-profile-popover`, `variant-popover-open` | `anchored-menu` §5.4 |
| `chat-new-conversation-confirm` | `confirmation-dialog` §5.5 |
| `chat-ad-active` | `advertisement-screen[fullscreen]` §5.13 |
| `landing` (`landing-default`) | wyłącznie szerokość kontenera `#vt-landing` §5.20 (bez klas §2) |

## 8. Capture przy odbiorze

Kadr każdego zrzutu = prostokąt `element.product-work-surface` o rozmiarze `width × height` targetu (§1.1). Klatka ruchu ustalona przez `mode=capture` (§5.16).

- `mac-wide`: każdy stan.
- `landing-default`: wszystkie pięć targetów (strona przewijana — kadr capture to pierwszy ekran; całość strony oceniana w zakładce „Strona WWW”).
- `iphone-16`, `pixel-8`, `galaxy-s20`, `iphone-16-pro-max`: stan bazowy każdej rodziny oraz każdy stan z różnicą układu z §5 (np. `chat-profile-popover` → popover pod pigułką, `chat-transcribed` → pole nad wierszem akcji, `training-active` → karta zadania i `Zakończ trening` widoczne).

## 9. Punkty kandydata (wymagają odbioru)

1. `onboarding-stepper[marks]` na całym mobile (< 900 px) (§5.10).
2. Akcje rozmowy na mobile widoczne bezpośrednio (`Wyczyść rozmowę`, `Zakończ rozmowę`), profil w osobnym wierszu (§5.2). Bez `Więcej`.
3. `text-composer` na mobile: stała wysokość (czat i trening 210 px, profil 162 px); pole nad wierszem mikrofon + `Wyślij`; w `recording`/`processing` sam przycisk mikrofonu na pełną szerokość (§5.7).
4. Tura tutora 96% na mobile i compact (§5.8).
5. `tutor-card[row]` na mobile (§5.9).
6. Reklama na pełnym obszarze roboczym, slot bez proporcji, „Zamknij reklamę” jako tekst w prawym górnym rogu (§5.13, R9).
7. `mic-control[labeled]` jako jedyny wariant: kompozytor i testy onboardingu, ze skutkami zależnymi od kontekstu; `processing` nieaktywny bez anulowania (§5.14).
8. `voice-selector`: pierwszy element listy `voice-download-callout` (`voice-add-voices`) z instrukcją i ilustracjami, bez `sticky` (§5.12).
9. `welcome-tutors`: 12 portretów w układzie 7 + 5 (124 px wide, 56/50 px mobile).
10. Menu i potwierdzenia na telefonie jako popovery pod wyzwalaczem, bez arkusza i przyciemnienia (§5.4, §5.5).
11. Dzienny czas rozmowy (RC9): wskaźnik w prawej grupie wiersza pod polem (z licznikiem znaków), komunikat o wyczerpaniu jako pierwszy element `composer-dock`; na telefonie komunikat zabiera wysokość historii (§5.19). Forma D5 do oceny Użytkownika.
12. Strona WWW (RC10): kolejność sekcji i podgląd aplikacji wg D3 (zaakceptowane 26.10.2026); układ telefonu ≤ 600 px, pośredni ≤ 1000 px (§5.20).
