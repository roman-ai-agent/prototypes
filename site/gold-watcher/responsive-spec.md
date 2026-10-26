# Gold Watcher r3.4 — specyfikacja responsywności

Układ wybiera jawny `target_id`: `mac-wide` to profil szeroki (od 900 px), cztery telefony to profil kompaktowy. W prototypie telefony mają zawsze dotyk jako główne wejście. Strona przewija się pionowo w jednym kontenerze `main`, pod stałym nagłówkiem. Jedna kolumna na wszystkich targetach.

## Kolejność (wszystkie targety)
1. `app-header`
2. `data-state` (tylko `stale`: nad wstępem; `loading`/`unavailable`: pod wstępem, zamiast reszty)
3. `intro` — tytuł i jedno zdanie na całą szerokość
4. `signal-status` — karta „Sygnał na dziś” na całą szerokość
5. `status-chart` — jeden wykres ceny na całą szerokość
6. sekcje rozwijane, domyślnie zwinięte: Po co mi Strażnik Trendu → Jak działa algorytm → Jak algorytm radzi sobie w różnych sytuacjach → Sprawdź dowolny rok kupna złota → Statystyka → Historia sygnałów → Złoto fizyczne

## mac-wide 1440×900
- **Nagłówek:** 60 px, padding 12/24 px. Logo, „Gold Watcher” i kurs „kurs N $ za uncję” (złoty, 14 px) w jednym rzędzie; po prawej świeżość danych w jednym wierszu i „Odśwież” 36 px.
- **Treść:** padding 20/24/40 px, odstęp kart 16 px, odstęp sekcji rozwijanych 12 px.
- **Karta sygnału:** padding 20/22 px; pigułka 28 px; zdanie znaczenia 16 px z marginesem 8 px nad i pod (z odstępem karty razem ok. 18 px); trzy statusy w rzędzie (`repeat(3, 1fr)`); objaśnienie ✓ / ✕ / ○; jedno–dwa zdania o ostatnim sygnale i 18 miesiącach; rozwijane „Liczby, na których opiera się sygnał” (5 akapitów).
- **Wykres ceny:** tytuł „Co widać na wykresie — …” jako nagłówek; okresy (8 przycisków, 36 px) i warstwy (5 pigułek) w jednym rzędzie z zawijaniem; pod nimi ◀ [od] [do] ▶; pole danych 290 px, oś Y 46 px, pasek kierunku 9 px; etykiety osi X co kwartał (okno < 1,6 roku) albo co 1/2/5 lat; wskazówka obsługi myszą; legenda z próbkami w rzędzie i objaśnieniami 1–3 zawsze widocznymi.
- **Sekcje:** teksty na całą szerokość karty (bez limitu długości wiersza). Tabela sytuacji 5 kolumn; kalkulator tabela 5 kolumn; statystyka 3 karty w rzędzie; historia sygnałów tabela 7 kolumn bez przewijania.

## Telefony: iphone-16 393×852, pixel-8 412×915, galaxy-s20 360×800, iphone-16-pro-max 440×956
- **Nagłówek:** 64 px, padding 10/14 px. Pierwszy wiersz: „Gold Watcher” i kurs „N $/oz” (13 px); drugi: świeżość danych. Tekst świeżości zawija się tylko przed datą („Nieodświeżone ·” / „Dane do 15.10.2026”), więc nie jest obcinany i nie wchodzi pod „Odśwież” (44 px).
- **Treść:** padding 14/12/32 px, odstęp kart 12 px, sekcji 10 px.
- **Karta sygnału:** padding 16 px, pigułka 18 px, statusy w jednej kolumnie. Etykieta, znak, zdanie i trzy statusy mieszczą się na pierwszym ekranie w `ready` (pomiar w README).
- **Wykres ceny:** „Co widać na wykresie — …” jest przyciskiem 44 px nad wykresem; rozwija objaśnienia legendy pod wykresem (domyślnie zwinięte, rozwinięte w `details-expanded`). Okresy i warstwy 44 px, zawijane. Nawigacja w siatce 2 kolumn: [od] [do], pod nimi ◀ ▶ na pół szerokości. Pole danych 230 px, oś Y 40 px, co druga etykieta kwartału. Wskazówka dotykowa. Podpowiedź dnia zajmuje całą szerokość pola.
- **Tabela sytuacji:** bez poziomego przewijania; nagłówek grupy w osobnym wierszu; trzy kolumny postaci; próg paniki 44 px.
- **Wykres wartości i bólu:** 170 + 110 px, oś Y 38 px; przy ponad 6 etykietach lat co druga.
- **Kalkulator:** `select` 44 px, przyciski czasu 44 px; tabela jako karty postaci.
- **Statystyka:** karty jedna pod drugą; etykieta horyzontu 52 px.
- **Historia sygnałów:** tabela 7 kolumn przewija się poziomo we własnym kontenerze (min. 640 px); strona się nie przewija.

## Interakcje wykresu ceny
- **Mysz:** najechanie pokazuje podpowiedź dnia (data i cena z jednostką) albo sygnału ▲/▼; kliknięcie skraca okno o połowę z klikniętym dniem na środku (min. 21 dni notowań); przeciągnięcie przesuwa okno.
- **Dotyk:** dotknięcie skraca okno jak kliknięcie; dotknięcie ▲/▼ (promień 24 px) pokazuje podpowiedź sygnału i nie skraca okna, drugie dotknięcie ją chowa; przesunięcie palcem przesuwa okno.
- **Pola „od”/„do”:** Enter albo wyjście z pola; błąd formatu, zakresu, kolejności lub długości pokazuje komunikat pod polami bez zmiany okna; data „do” po ostatnim dniu przechodzi na ostatni dzień.
- **◀ ▶:** przesunięcie o połowę okna, do brzegu danych.
- **Warstwy:** średnie domyślnie włączone do 10 lat, dołki do 3 lat; wybór ręczny trwa do zmiany długości okna. Znacznik pierwszego kupna na wykresie wartości jest zawsze widoczny. Rozmiar znaczników zależy od ich gęstości (min. 50%).

## Stany
- **ready / ready-in-market:** sekcje zwinięte; `ready-in-market` pokazuje „TRZYMAJ ZŁOTO” i trwające trzymanie złota (bez pasa kupna).
- **details-expanded:** „Liczby…”, objaśnienia wykresu i wszystkie 7 sekcji rozwinięte; kalkulator 2000 / 10 lat z wykresem typowego dnia decyzji.
- **loading:** karta `data-state` (560 px na `mac-wide`, cała szerokość na telefonie) z tekstem „Pobieranie historii cen 34 z 94”; warianty tekstu tego samego układu: „Pobieranie danych…”, „Liczenie trendu długoterminowego…”. „Pobieranie…” zajęty.
- **refreshing:** dane widoczne; „Odświeżanie…” zajęty; nagłówek „Odświeżanie… · Dane do …”.
- **stale:** baner `data-state` na całą szerokość nad wstępem; nagłówek „Nieodświeżone · Dane do …”.
- **unavailable:** karta „Brak danych o cenie złota” ze „Spróbuj ponownie”.

## Minima
- Tekst co najmniej 11,5 px (osie), treść 12,5–16,5 px.
- Cele dotyku na telefonach co najmniej 44 px; nagłówki sekcji rozwijanych 48 px.

## Reguły bez zmian względem r3.3
- Odstęp 16 px między polem wykresu a sąsiednim tekstem (r3.2).
- `chart-legend`: ramka #e2e8f0, tło #f6f8fb (`pain`: #fbf6f4), promień 10 px, próbki w rzędzie z zawijaniem (odstęp 6/14 px) — także w legendach statystyki.
- Etykiety osi Y z tej samej skali co serie (r3.3).
