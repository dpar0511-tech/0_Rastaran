# 10 · Tuning, otwarte decyzje i ryzyka

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md).
> Pełna analiza prawna: `../01_Koncepcja_produktu.md` §6. Tutaj wyłącznie **przełożenie na
> wymagania produktowe** — nie powtarzamy analizy, nie rozszerzamy jej.

---

## 1. Jak używać tego dokumentu

| Sekcja | Kiedy sięgać |
|---|---|
| **§2 · `TUN-*`** | Gdy szukasz, co poprawić w wyglądzie albo w logice biznesowej. Każdy wpis ma wariant obecny, alternatywę, sposób pomiaru i szacowany wpływ |
| **§3 · `DEC-*`** | Gdy potrzebujesz wiedzieć, co jeszcze nie jest rozstrzygnięte i co od tego zależy |
| **§4 · `LEG-*`** | Gdy projektujesz coś, co dotyka pieniędzy, alkoholu, danych osobowych albo paragonów |
| **§5 · Ryzyka** | Przy planowaniu wydania i przy rozmowie z inwestorem |

**`TUN-*` to nie lista błędów.** To miejsca, w których podjęliśmy **świadomą decyzję z realną
alternatywą** i której nie da się rozstrzygnąć od biurka — trzeba zmierzyć.

---

## 2. Kandydaci do tuningu

### 2.1. Ranking

Uporządkowane wg stosunku wpływu do nakładu. Górne pięć to te, którymi warto zająć się najpierw.

| ID | Co | Rodzaj | Wpływ | Nakład | Wydanie |
|---|---|---|---|---|---|
| `TUN-007` | Kolejność i copy opcji podziału rachunku | Logika | **Bardzo wysoki** — bezpośrednio marża | Niski | v1 |
| `TUN-004` | Moment upsellu | Logika | **Bardzo wysoki** — +8–12% rachunku | Średni | v1 |
| `TUN-001` | Pozycja „Zamów to samo" u powracającego | Wygląd | **Wysoki** — cel 8 s | Niski | v0.1 |
| `TUN-005` | Presety napiwków | Logika | **Wysoki** — +15% napiwków | Bardzo niski | v0.2 |
| `TUN-002` | Nawigacja menu: przewijanie vs zakładki | Wygląd | Wysoki — konwersja skan→zamówienie | Średni | v0.1 |
| `TUN-014` | Baza naliczania napiwku | Logika | Średni | Bardzo niski | v0.2 |
| `TUN-021` | Sposób podawania ETA | Logika | Średni — zaufanie i wezwania kelnera | Niski | v0.1 |
| `TUN-003` | Alergeny w liście pozycji | Wygląd | Średni | Niski | v0.1 |
| `TUN-012` | Sortowanie tablicy stolików | Wygląd | Średni — adopcja przez kelnerów | Niski | v0.1 |
| `TUN-015` | Próg alertu „rachunek bez płatności" | Logika | Średni | Bardzo niski | v0.1 |
| `TUN-006` | Wyzwalacz trybu ciemnego u gościa | Wygląd | Niski | Bardzo niski | v0.1 |
| `TUN-008` | Karta pozycji: zdjęcie vs kompakt | Wygląd | Niski | Niski | v0.1 |
| `TUN-011` | KDS: kolumny stacji vs jedna kolejka | Wygląd | Niski | Średni | v0.1 |
| `TUN-018` | Domyślny tryb serwowania | Logika | Niski | Bardzo niski | v1 |
| `TUN-022` | Próg oceny do kierowania na Google | Logika | Niski | Bardzo niski | v1 |
| `TUN-023` | Zawartość planu Menu (0 zł) | Logika | Wysoki — konwersja na plany płatne | Niski | v0.1 |
| `TUN-024` | Stawki prowizji wg metody płatności | Logika | **Bardzo wysoki** — zależny od `DEC-009` | Niski | v0.2 |

---

### 2.2. Tuning logiki biznesowej

#### `TUN-007` · Kolejność i copy opcji podziału rachunku

**Problem.** Podział rachunku to nasza kluczowa funkcja — i jednocześnie ta, która pracuje
przeciwko marży. Koncepcja sama to wykazuje (§7.3 p.2): cztery płatności zamiast jednej dają
ten sam przychód brutto, ale wyższy koszt PSP na składowej stałej. Marża na składowej stałej
jest cienka (~0,13 zł), więc każdy dodatkowy split ją zjada.

**Obecnie.** `ZAPŁAĆ I WYJDŹ` jako główne wezwanie, `PODZIEL RACHUNEK` jako drugorzędne
(`SCR-G-07`).

**Warianty do zmierzenia:**

| Wariant | Opis | Ryzyko |
|---|---|---|
| A (obecny) | Zapłata całości pierwsza, podział drugi | Neutralny |
| B | Podział z podpowiedzią: `Jedna osoba płaci, resztę rozliczcie BLIK-iem między sobą` | Może być odebrane jako unikanie funkcji, którą reklamujemy |
| C | Równorzędne, bez sugestii | Najuczciwsze, najgorsze dla marży |

**Pomiar:** udział rachunków dzielonych, średnia liczba płatności na rachunek, marża na rachunek.

⚠️ **Rozwiązanie techniczne jest ważniejsze od tuningu interfejsu.** Jeśli PSP zgodzi się
pobierać składową stałą **raz z rachunku**, a nie z każdego udziału (`DEC-009b`), problem znika
i wariant C staje się bezpieczny. Negocjacje z PSP mają tu wyższy priorytet niż testy A/B.

---

#### `TUN-004` · Moment upsellu

**Obecnie.** Rekomendacja w koszyku (`SCR-G-04`), przed złożeniem zamówienia.

**Warianty:**

| Wariant | Moment | Ocena |
|---|---|---|
| A (obecny) | W koszyku, przed zamówieniem | Sprawdzony, ale gość jest już zdecydowany |
| B | Przy dodawaniu pozycji, jako sugestia | Wyższy kontekst, ryzyko przerwania ścieżki |
| C | **Push 8–10 minut po podaniu** | **Niewykorzystany w koncepcji.** Zerowy koszt tarcia — nie przerywa niczego. Gość już je, jest zadowolony, ma otwarty rachunek |
| D | Na ekranie rachunku | Za późno — gość jest w trybie wyjścia |

**Wariant C jest najciekawszy i nie występuje u nikogo z konkurencji.** Klasyczne „a może
jeszcze deser?", które kelner zadaje osobiście — tyle że zadane dokładnie wtedy, kiedy kelner
jest zajęty gdzie indziej. Zgodne z zasadą Z2: nie odbiera kelnerowi upsellu, tylko domyka te
sytuacje, do których i tak by nie dotarł.

⚠️ **Ograniczenie:** push do gościa wymaga zgody. Powiadomienie o **statusie zamówienia** mieści
się w realizacji umowy, ale powiadomienie **z ofertą** to marketing i wymaga zgody z art. 398 PKE
(`LEG-007`). Wariant C bez zgody jest niedozwolony — albo prosimy o zgodę, albo pokazujemy
sugestię pasywnie na otwartym ekranie statusu.

**Pomiar:** średni rachunek, współczynnik przyjęcia sugestii, częstość wyłączania powiadomień.
**Cel:** +8–12% średniego rachunku.

---

#### `TUN-005` · Presety napiwków

**Obecnie.** `Bez napiwku` · `5%` · `10%` · `Inna kwota` (`SCR-G-10`).

**Podstawa:** badanie MFR 2025 — 89% polskich gości zostawia napiwki, typowy rozmiar **5–10%**.
Amerykańskie presety 15/20/25 są w Polsce odbierane jako nachalne i **obniżają** odsetek napiwków.

**Warianty:**

| Wariant | Presety | Hipoteza |
|---|---|---|
| A (obecny) | 0 / 5% / 10% / własna | Zgodny z normą rynkową |
| B | 0 / 5% / 10% / 15% / własna | Czy górna kotwica podnosi średnią, czy zniechęca |
| C | Kwoty zamiast procentów: 0 / 5 zł / 10 zł / własna | Przy małych rachunkach procent daje śmieszne kwoty (5% z 24 zł = 1,20 zł) |
| D | Adaptacyjne wg wysokości rachunku | Procenty przy dużych, kwoty przy małych |

**Wariant D jest prawdopodobnie najlepszy** i najtańszy w implementacji. Warto zacząć od
zmierzenia rozkładu wysokości rachunków w pilocie.

⚠️ **Czego tuningować nie wolno:** `Bez napiwku` musi zostać pełnoprawnym, pierwszym kafelkiem.
Żaden preset nie może być zaznaczony domyślnie. To nie jest kwestia optymalizacji — napiwek
obowiązkowy przestaje być napiwkiem i staje się częścią ceny usługi z VAT 8% (`LEG-005`).

**Cel:** +15% napiwków na kelnera.

---

#### `TUN-014` · Baza naliczania napiwku

**Pytanie.** Napiwek 10% — od czego dokładnie?

| Wariant | Podstawa | Uwagi |
|---|---|---|
| A (obecny) | Od kwoty brutto rachunku | Najprostsze i najbardziej zrozumiałe dla gościa |
| B | Od kwoty netto | Kelner dostaje mniej, gość nie rozumie różnicy |
| C | Przy podziale — od **własnej części** | Obecne. Alternatywa: od całości, ale to zawyża |
| D | Z wyłączeniem alkoholu | Bar straciłby najwięcej. Odrzucone |

**Rozstrzygnięcie:** A + C. Prosto, uczciwie i zrozumiale.
**Wymaga zapisania w regulaminie**, bo dotyczy kwoty przekazywanej osobie trzeciej.

---

#### `TUN-021` · Sposób podawania ETA

**Napięcie.** ETA optymistyczne buduje zadowolenie i pęka. ETA bezpieczne odstrasza.

| Wariant | Sposób | Skutek |
|---|---|---|
| A (obecny) | Mediana rzeczywistych czasów z `PrepTimeLog` | Trafia w 50% przypadków — połowa gości czeka dłużej niż obiecano |
| B | Percentyl 75 | Rzadziej zawodzi, wygląda wolniej |
| C | Widełki: `12–18 minut` | Uczciwe, ale gość zapamiętuje dolną granicę |
| D | Bez ETA, tylko etapy | Zwiększa liczbę wezwań kelnera „gdzie moje danie" |

**Rekomendacja:** wariant B. Lepiej podać 18 minut i podać w 14, niż podać 12 i podać w 17.
Zawiedziona obietnica kosztuje więcej niż ostrożna.

**Pomiar:** liczba wezwań kelnera w kategorii „status zamówienia", ocena wizyty a rozbieżność ETA.

---

#### `TUN-023` · Zawartość planu Menu (0 zł)

**Napięcie.** Plan darmowy istnieje **wyłącznie po to, żeby doprowadzić lokal do planu Pay** —
bo marża z płatności jest mniej więcej równa abonamentowi (§7.3 koncepcji). Jeśli będzie zbyt
bogaty, lokal na nim zostanie. Zbyt ubogi — nie wciągnie.

| Wariant | Zawartość | Ryzyko |
|---|---|---|
| A (obecny) | Menu QR, 4 języki, alergeny, lista 86, wezwanie kelnera. **Bez zamawiania** | Lokal może uznać, że to wystarczy — ale bez zamawiania nie ma dowodu ROI, więc raczej awansuje |
| B | + zamawianie z limitem 50/miesiąc | ⚠️ Sprzeczne z naszym własnym argumentem sprzedażowym „bez limitów". Nie robić |
| C | Pełne zamawianie przez 30 dni, potem tylko menu | Silniejszy haczyk, wymaga jasnej komunikacji, żeby nie wyglądać na pułapkę |

**Rekomendacja:** A, z okresem próbnym C nałożonym na 30 dni. Limity odpadają — atakowanie
UpMenu (6,70 zł za zamówienie ponad limit) i GoPOS (od 0,40 zł) za karanie wzrostu przestaje
działać, jeśli sami je stosujemy.

---

#### `TUN-024` · Stawki prowizji wg metody płatności

⚠️ **To nie jest tuning, tylko plan awaryjny dla całej ekonomii.**

Model zakłada 1,9% + 0,30 zł all-in. Przy karcie koszt własny wynosi ~2,5–3,5%, więc
**transakcja kartowa jest stratna**. Model spina się wyłącznie przy dominacji BLIK.

| Miks BLIK/karta | Działanie |
|---|---|
| 70/30 lub lepiej | Bez zmian. Jedna stawka, bez gwiazdek — to nasz najmocniejszy argument w segmencie, gdzie wszyscy ukrywają koszty |
| 55–70% BLIK | Podniesienie do ~2,1% albo minimalna kwota transakcji kartowej |
| Poniżej 55% BLIK | Osobne stawki per metoda **albo** 2,3% flat. Utrata argumentu „jedna cyfra" |

**Zależy w całości od `DEC-009`.** Do czasu uzyskania rzeczywistych stawek od PSP to jest
scenariusz, nie prognoza. **Mierzyć od pierwszego dnia v0.2.**

---

#### Pozostałe parametry logiki

| ID | Parametr | Obecnie | Do rozważenia |
|---|---|---|---|
| `TUN-015` | Alert „rachunek bez płatności" | 25 min | Zależny od typu lokalu. W barze 15 min, w restauracji 35 min. Kandydat na ustawienie per lokal |
| `TUN-016` | Sesja porzucona | 30 min bez zamówienia | W ogródku piwnym za długo — stolik jest zajmowany wirtualnie |
| `TUN-017` | Przywracanie listy 86 | Start kolejnego dnia serwisowego | Alternatywa: pytanie do kuchni przy otwarciu zmiany. Lista, która się nie czyści, przestaje odpowiadać rzeczywistości |
| `TUN-018` | Domyślny tryb serwowania | `Wszystko razem` | W restauracji naturalniejsze byłyby etapy. Ustawienie per lokal |
| `TUN-019` | Eskalacja wezwania kelnera | 90 s | Za krótkie w pełnej sali, za długie w pustej. Kandydat na próg zależny od obłożenia |
| `TUN-020` | Ranking napiwków | Włączony | Manager może wyłączyć — w części zespołów grywalizacja psuje atmosferę i działa wbrew zasadzie Z2 |
| `TUN-022` | Próg kierowania na Google | Ocena 4–5 | Wariant: tylko 5. Mniej opinii, ale wyższa średnia |

---

### 2.3. Tuning wyglądu

#### `TUN-001` · Pozycja „Zamów to samo" u powracającego gościa

**Obecnie.** Karta ponowienia **nad zgięciem**, przed kategoriami (`SCR-G-01`).

**Uzasadnienie.** Budżet 8 s nie zostawia miejsca na przewijanie. To najczęściej używana funkcja
w barach — „jeszcze jedno piwo".

**Warianty:** (A) karta nad zgięciem — obecny · (B) przyklejony pasek u dołu ·
(C) pierwsza pozycja w liście menu · (D) osobny ekran startowy dla powracających.

**Pomiar:** czas od skanu do złożenia zamówienia u powracających, udział ponowień.
**Cel:** < 8 s.

---

#### `TUN-002` · Nawigacja menu: przewijanie vs zakładki

**Obecnie.** Ciągłe przewijanie z przyklejonymi nagłówkami sekcji. Znaczniki kategorii
przewijają do sekcji, nie zmieniają ekranu.

**Uzasadnienie.** Gość w barze **przegląda**, nie nawiguje — nie zna karty.

**Kiedy obecne rozwiązanie zawiedzie:** przy karcie ponad ~80 pozycji przewijanie staje się
męczące. Wtedy zakładki przełączające widok mogą wygrać.

**Wariant hybrydowy do rozważenia:** przewijanie do 60 pozycji, automatyczne przełączenie na
zakładki powyżej. Karta lokalu jest znana w momencie publikacji, więc wybór można podjąć
automatycznie.

**Pomiar:** konwersja skan → zamówienie, głębokość przewijania, użycie wyszukiwarki.
**Cel:** konwersja > 55%.

---

#### Pozostałe parametry wyglądu

| ID | Element | Obecnie | Alternatywa |
|---|---|---|---|
| `TUN-003` | Alergeny w liście pozycji | Widoczne w karcie na liście | Tylko na ekranie pozycji. Formalnie wystarczy (`LEG-009`), ale filtr `F-G-009` zyskuje na wcześniejszej widoczności. Kompromis: pokazywać tylko przy aktywnym filtrze alergenowym |
| `TUN-006` | Wyzwalacz trybu ciemnego | Systemowy + wymuszenie po zmroku wg godzin lokalu | Tylko systemowy. Wymuszenie może zaskoczyć gościa, który świadomie wybrał jasny |
| `TUN-008` | Karta pozycji | Zdjęcie + opis + alergeny | Lista kompaktowa bez zdjęć przy karcie > 60 pozycji. Większość lokali i tak nie ma kompletu zdjęć |
| `TUN-009` | Pasek koszyka | `3 pozycje · 62,00 zł` | Sama kwota jako główna. Kwota jest tym, na co gość patrzy |
| `TUN-010` | Ekran statusu | Pasek postępu + lista pozycji | Sam pasek. Lista pozycji jest cenna, gdy część czeka na potwierdzenie wieku |
| `TUN-011` | Układ KDS | Kolumny wg stacji | Jedna kolejka chronologiczna. W małej kuchni z jedną stacją kolumny to zbędna złożoność. Kandydat na ustawienie per lokal |
| `TUN-012` | Sortowanie tablicy stolików | Wg pilności | Wg numeru stolika. ⚠️ Sortowanie wg pilności jest tym, co zamienia listę w narzędzie — ale część kelnerów przyzwyczajonych do układu sali może preferować numery. Kandydat na przełącznik |
| `TUN-013` | Onboarding | Swobodny checklist | Kreator liniowy krok po kroku. Checklist jest lepszy dla wracających, kreator dla pierwszego uruchomienia |

---

## 3. Otwarte decyzje

### 3.1. Blokujące — przed pierwszą złotówką

| ID | Decyzja | Od czego zależy | Kto |
|---|---|---|---|
| `DEC-001` | Umowa z PSP w trybie split — **pisemne potwierdzenie, że środki gościa nie trafiają na nasze konto**, wraz ze schematem przepływu | Cały model płatności. Bez tego grozi wymóg licencji MIP/KIP (`LEG-001`) | Prawnik + PSP |
| `DEC-002` | Jednostronna umowa agencyjna z lokalem — agent **wyłącznie** lokalu, pod art. 6 pkt 2 UUP | Wyłączenie spod reżimu usług płatniczych. ⚠️ Nie wolno być agentem obu stron | Prawnik |
| `DEC-003` | **ORD-IN:** moment wystawienia paragonu przy przedpłacie przez PSP | Architektura fiskalna, granica S1, treść SLA w umowie (`LEG-003`) | Doradca podatkowy |
| `DEC-004` | **ORD-IN:** napiwki — PIT i ZUS przy bezpośrednim splicie na konto kelnera | **Cała funkcja napiwków.** Błąd = obciążenie lokalu ~40% od każdego napiwku (`LEG-006`) | Doradca podatkowy |
| `DEC-006` | Umowa powierzenia przetwarzania (art. 28 RODO) — szablon | Każda umowa z lokalem (`LEG-008`) | Prawnik |
| **`DEC-009`** | **Wybór PSP i uzyskanie rzeczywistych stawek** | **Najbardziej krytyczny punkt dla ekonomii** | Zarząd |

#### `DEC-009` — trzy pytania, które trzeba zadać każdemu PSP

| # | Pytanie | Dlaczego to rozstrzyga |
|---|---|---|
| **a** | Stawka **osobno dla BLIK** i **osobno dla kart** | Przy 1,9% + 0,30 zł karta jest stratna. Cały model zakłada dominację BLIK, co dla Polski jest realistyczne — ale **musi być zweryfikowane przed startem, nie po** |
| **b** | Czy można pobierać **składową stałą raz z rachunku**, a nie z każdego udziału podziału | Rozstrzyga, czy podział rachunku — nasza kluczowa funkcja — nie zjada marży (`TUN-007`) |
| **c** | Czy obsługiwany jest **BLIK-split na wielu odbiorców jednocześnie** | **Rozstrzyga, czy model napiwków jest w ogóle możliwy.** Największa niewiadoma projektu |

Kandydaci: Przelewy24, Tpay, Autopay, Stripe Connect, Adyen for Platforms.

⚠️ **Odpowiedź „nie" na pytanie (c) oznacza konieczność przeprojektowania funkcji napiwków** —
a to jest fundament całej strategii dystrybucji (zasada Z2). Dlatego to pytanie zadajemy jako
pierwsze, przed jakąkolwiek pracą nad `MOD-tips`.

### 3.2. Produktowe i techniczne

| ID | Decyzja | Kontekst |
|---|---|---|
| `DEC-005` | **HUB Paragonowy vs POS — kto wysyła e-paragon?** | Jeśli fiskalizuje POS, to POS ma dane paragonu. Trzy scenariusze poniżej |
| `DEC-007` | Wybór pierwszej integracji POS | Dotykačka (najbardziej otwarte API, już pełni rolę huba dla konkurencji) vs GoPOS (najszerzej rozpowszechniony, przejrzyste warunki partnerskie). Wymaga dostępu do obu API i wyceny pracochłonności |
| `DEC-008` | Status druku sejmowego nr 2358 (alkohol) + projekt MZ o ograniczeniach nocnych 22:00–6:00 | Może wymusić zmiany w `F-G-032` i `F-K-008` przed startem |
| `DEC-010` | **10 wywiadów pogłębionych: 5 właścicieli, 5 kelnerów** | Weryfikacja hipotezy „kelner jest beneficjentem" — założenia, na którym stoi cała dystrybucja. **Powinno się zdarzyć przed v0.1, nie po** |
| `DEC-011` | Czy przesunąć rezerwacje (`F-S-005`) do v1 | 78% oceny gości — najwyżej oceniana technologia w Polsce. Nie broni fosy, ale może domykać transakcje sprzedażowe |
| `DEC-012` | DAC7 przy ewolucji w marketplace | Obowiązek raportowania do Szefa KAS, kara do 1 mln zł. Dotyczy `F-X-002` (`LEG-013`) |
| `DEC-013` | Własna kasa GUM — profil odpowiedzialności prawnej | Wejście w rolę dostawcy urządzenia fiskalnego. Wymaga osobnej analizy przed v3 |
| `DEC-014` | **Własne repozytorium git** | `0_Rastaran` leży obecnie wewnątrz repozytorium projektu Drukarnia ERP — innego produktu. Do rozwiązania przed pierwszym commitem kodu |
| `DEC-015` | P2B (UE) 2019/1150 | Przejrzystość warunków dla użytkowników biznesowych, 15 dni na powiadomienie o zmianach, wewnętrzny system skarg. Często pomijane przez startupy (`LEG-015`) |

#### `DEC-005` — trzy scenariusze e-paragonu

| Scenariusz | Kto wysyła do HUB | Zalety | Wady |
|---|---|---|---|
| **A** | POS lokalu | Zgodne z tym, kto faktycznie fiskalizuje. Zero ryzyka rozjazdu danych | Zależne od tego, czy dany POS to umie. **Przestaje być naszym wyróżnikiem** |
| **B** | My, na podstawie potwierdzenia z POS | Wyróżnik techniczny zostaje przy nas. Działa niezależnie od możliwości POS-a | Ryzyko rozjazdu z danymi POS-a. Wymaga potwierdzenia dopuszczalności |
| **C** | Hybryda — POS, jeśli umie, my w pozostałych przypadkach | Największe pokrycie | Największa złożoność, dwie ścieżki do utrzymania |

⚠️ Twierdzenie „MF potwierdził możliwość integracji aplikacji zewnętrznych" pochodzi z FAQ
Ministerstwa Finansów i **wymaga bezpośredniej weryfikacji przed decyzjami architektonicznymi**.
Wpływa też na tryb bez POS (`P2`), gdzie nie ma POS-a, który mógłby cokolwiek wysłać.

---

## 4. Ograniczenia prawne jako wymagania produktowe

> Przełożenie `01_Koncepcja_produktu.md` §6 na wymagania. **Nie jest to porada prawna.**
> Pozycje oznaczone `[PRAWNIK]` wymagają potwierdzenia przez polskiego prawnika lub doradcę
> podatkowego, część — indywidualnej interpretacji podatkowej (ORD-IN).

| ID | Ograniczenie | Wymaganie produktowe | Realizacja |
|---|---|---|---|
| `LEG-001` | **Czerwona linia.** Środki gościa nie mogą trafiać na nasze konto bez licencji MIP/KIP. Art. 150 ust. 1 UUP: do 5 mln zł kary lub 2 lata pozbawienia wolności | Split wykonywany **w bramce płatniczej**. Nigdy własny rachunek zbiorczy. Nie wolno być agentem obu stron | `I10`, [`04`](04_Architektura_Moduly.md) §7.2 W1, `DEC-001` |
| `LEG-002` | Gastronomia ma bezwzględny obowiązek kasy (art. 111 ust. 1 VAT). Limit 20 tys. zł nie działa | **Fiskalizuje lokal**, my jesteśmy dostawcą oprogramowania i agentem. W trybie bez POS obowiązek zostaje po stronie lokalu — komunikowany wprost | `MOD-fiscal`, `SCR-K-06` |
| `LEG-003` | Paragon nie później niż w chwili przyjęcia zapłaty (art. 111 ust. 3a pkt 1). **Przy przedpłacie obowiązek podatkowy powstaje w momencie zapłaty** | Fiskalizacja **synchroniczna**, SLA < 5 s jako punkt umowy. Brak potwierdzenia → rejestr niezgodności + alert, **nigdy cofnięcie płatności**. Asynchroniczna fiskalizacja „na koniec zmiany" jest ryzykiem podatkowym klienta | Granica S1, `RULE-022`, `E4`, `SCR-P-08`, `DEC-003` |
| `LEG-004` | Sprzedawcą alkoholu jest **lokal** — posiadacz zezwolenia. Przyjęcie pieniędzy we własnym imieniu za pozycję alkoholową to przestępstwo z art. 43 ust. 1 uwt | We wszystkich dokumentach i w interfejsie sprzedawcą jest lokal. Nigdy my | `05` §11.4, `SCR-K-05` |
| `LEG-005` | Obowiązkowy service charge to część ceny usługi: VAT 8%, obowiązkowo na paragonie. Napiwek musi być **zawsze opcjonalny, nigdy domyślny** | `Bez napiwku` jako pełnoprawny pierwszy wybór. Żaden preset niezaznaczony wstępnie. Service charge jako osobne pole rachunku z VAT | `RULE-004`, `RULE-005`, `SCR-G-10` |
| `LEG-006` | **Najbardziej wrażliwa strefa.** Napiwek przekazany przez podmiot organizujący płatności bezgotówkowe jest — wg interpretacji KIS — przychodem ze stosunku pracy. Naiwna implementacja tworzy lokalowi obciążenie PIT+ZUS ~40% | Trzy warunki **kumulatywnie**: (1) napiwek nie trafia ani na konto lokalu, ani na nasze — PSP wysyła wprost do kelnera; (2) lokal nie ma władztwa nad tymi środkami; (3) zapisane w regulaminie, umowie i interfejsie. **Zakaz puli wspólnej (poolingu)** | `RULE-004`, `RULE-020`, `I10`, `SCR-G-10`, `SCR-P-05`, `DEC-004` |
| `LEG-007` | PKE art. 398 (od 10.11.2024): komunikacja marketingowa wyłącznie za uprzednią zgodą. **Nie wolno nawiązać kontaktu, żeby dopiero poprosić o zgodę** | Moment składania zamówienia to **jedyne legalne okno**. Osobne pola per kanał, bez zaznaczenia wstępnego, jednoznaczny administrator, wersjonowana treść | `RULE-023`, `SCR-G-16`, `TUN-004` wariant C |
| `LEG-008` | Jesteśmy **procesorem** danych gości lokalu. Obowiązkowa umowa powierzenia (art. 28 RODO). Zbieranie bazy „dla siebie" narusza art. 28 ust. 10 | Dane gościa należą do lokalu, eksportowalne i usuwalne. To jednocześnie nasz najmocniejszy argument przeciw Wolt | `RULE-024`, zasada Z4, `SCR-P-12`, `DEC-006` |
| `LEG-009` | Rozporządzenie (UE) 1169/2011 art. 44: informacja o 14 alergenach dla żywności nieopakowanej, **bez progu wielkości lokalu**. Sama informacja ustna nie wystarcza. Musi być dostępna **przed** zamówieniem | Alergeny **na ekranie pozycji, nad przyciskiem koszyka**. Legenda na tym samym ekranie. Piktogram + etykieta tekstowa. Publikacja zablokowana bez kompletu danych. Odpowiedzialność za treść — lokal (art. 8 FIC), za wyświetlenie — my | `RULE-010`, `I8`, `SCR-G-03`, `SCR-P-01`, `SCR-P-03` |
| `LEG-010` | Weryfikacja wieku przy alkoholu — art. 15 ust. 2 uwt. Samodeklaracja „mam 18+" nie wystarcza. Brak obowiązującego standardu weryfikacji cyfrowej (przepisy wykreślone z projektu w związku z pracami nad eIDAS2 / EU Digital Identity Wallet do końca 2026) | Potwierdzenie przez personel **przy podaniu**, z zapisem w logu o wartości dowodowej. Osobny stan pozycji zamówienia | `RULE-008`, `I5`, `SCR-K-05`, `SCR-G-05`, `DEC-008` |
| `LEG-011` | Ustawa z 26.04.2024 (Dz.U. 2024 poz. 731), od 28.06.2025. Standard EN 301 549 / WCAG 2.1 AA, z wyraźnym wymogiem dostępności **funkcji płatności**. Sankcje do 10-krotności średniej płacy, nie więcej niż 10% obrotu | WCAG 2.1 AA wpisane w tokeny **od pierwszego dnia**. Trzy obowiązki formalne: sekcja „Dostępność" w regulaminie, zgłoszenie do Ministra Cyfryzacji przy niezgodności, udokumentowana ocena adekwatności przy wyłączeniu z art. 21 | `05` §6 |
| `LEG-012` | Art. 59ea UUP: lokal nie może uzależnić umowy od płatności bezgotówkowej. Tryb „tylko aplikacja" jest niezgodny z prawem | `Zapłacę u kelnera` **zawsze widoczne i osiągalne**, nigdy ukryte pod „więcej opcji". Do zakomunikowania klientom | `RULE-009`, `SCR-G-07`, `SCR-G-09` |
| `LEG-013` | DAC7 — przy ewolucji w marketplace obowiązek raportowania do Szefa KAS, kara do 1 mln zł. W modelu SaaS ryzyko niskie | Pozostajemy przy modelu SaaS. Przed `F-X-002` — analiza | `DEC-012` |
| `LEG-014` | KSeF (nasz własny billing wobec lokali): przyjmowanie e-faktur od 01.02.2026, wystawianie od 01.04.2026 (MŚP). B2C w gastronomii **nie wchodzi** do KSeF — gość dostaje paragon, nie e-fakturę | Integracja KSeF dla własnego billingu w v1 | `01` §4.4 |
| `LEG-015` | P2B (UE) 2019/1150: przejrzystość warunków dla użytkowników biznesowych, 15 dni na powiadomienie o zmianach, wewnętrzny system rozpatrywania skarg | Regulamin, procedura zmian, kanał skarg | `DEC-015` |

---

## 5. Ryzyka

| Ryzyko | Prawdopod. | Skutki | Jak zdejmujemy |
|---|---|---|---|
| **Sabotaż personelu** | Wysoka — główna przyczyna niepowodzeń wdrożeń QR | Krytyczne | Cała powierzchnia Kelner Pro (zasada Z2): napiwki wprost na konto, ranking, osobiste kody. Kelner zarabia więcej i staje się adwokatem. Weryfikacja hipotezy: `DEC-010` |
| **⚠️ Miks płatności gorszy od zakładanego** | Średnia | **Krytyczne** — przy 1,9% + 0,30 zł transakcja kartowa jest stratna | `DEC-009a` **przed** startem. Plan awaryjny: `TUN-024` |
| **⚠️ BLIK-split na wielu odbiorców niedostępny** | Średnia | **Krytyczne** — funkcja napiwków niewykonalna, upada fundament dystrybucji | `DEC-009c` jako pierwsze pytanie do PSP, przed pracą nad `MOD-tips` |
| **Błąd w konstrukcji napiwków** | Średnia | Ciężkie — obciążenie lokalu PIT+ZUS ~40%, utrata zaufania rynku | ORD-IN przed uruchomieniem funkcji (`DEC-004`). Zero poolingu. Split wprost na konto kelnera |
| **Moment fiskalizacji przy przedpłacie** | Średnia | Ciężkie — ryzyko podatkowe klienta | ORD-IN (`DEC-003`). Fiskalizacja synchroniczna z SLA. Zero fiskalizacji asynchronicznej |
| **Wolt Storefront za darmo** | Wysoka | Wysokie | Trzy argumenty: baza gości jest wasza, nie marketplace'u; my umiemy salę (podział, napiwki, dozamawianie, coursing); darmowe przestaje być darmowe, gdy staje się jedynym kanałem (prowizje marketplace'ów w PL: 20–30%) |
| **Rozdrobnienie POS** — żaden gracz > 30% rynku | Wysoka | Średnie | Przyjmujemy jako daną: 6–8 integracji dla ~60% pokrycia, warstwa antykorupcyjna od v0. **Tryb bez POS** (`P2`) zdejmuje zależność w pilocie. v3 — własna kasa |
| **Podział rachunku zjada marżę** | Wysoka — to nasza kluczowa funkcja | Średnie | `DEC-009b` (składowa stała raz z rachunku) ma priorytet nad tuningiem interfejsu (`TUN-007`) |
| **Goście nie skanują** | Średnia | Wysokie | Pozycjonowanie „pomiń kolejkę", nie „nasze menu". Kelner aktywnie promuje. Dobre stojaki. Papier zostaje. **v0.1 mierzy dokładnie to ryzyko przed wydaniem budżetu na płatności** (`P1`) |
| **Presja cenowa** (GoPOS: QR za 49 zł) | Wysoka | Średnie | Plan Menu 0 zł. Zarabiamy na płatnościach, nie na menu (`TUN-023`) |
| **LSI Gastro włącza własny moduł** | Średnia | Średnie | Szybkość wydań i doświadczenie gościa. Gracze z długiem technologicznym nie przegrają nas na UX. Jesteśmy też dostępni dla lokali spoza LSI |
| **Długi cykl sprzedaży w sieciach** | Średnia | Niskie | Beachhead to lokale niezależne z szybką decyzją. Sieci po 300+ logotypach |
| **Zależność od jednego PSP** | Średnia | Wysokie | `PaymentProvider` za interfejsem od pierwszego dnia ([`04`](04_Architektura_Moduly.md) §7.2). Zmiana PSP musi być zmianą adaptera, nie przepisaniem |

---

## 6. Czego nie używać w rozmowach z inwestorami jako faktu

Przeniesione z `01_Koncepcja_produktu.md` §10, bo dotyczy każdego materiału opartego na tej
dokumentacji:

- **26% (QR) i 72% (kioski) w jednym zestawieniu** — to dwa różne wydania raportu MFR, różne
  próby, różne pytania. Przytaczać wyłącznie rozdzielnie
- Liczba „70% restauracji zrezygnowało z QR" — brak źródła pierwotnego, to twierdzenie dostawcy
- Skala deklarowana przez konkurencję: LSI Gastro 18 000, Restaumatic 4 500–5 000+,
  UpMenu 3 200+ / 47 krajów, Choice QR 7 500 (2022), WMenu 570+
- „Choice QR nie ma polskiej integracji fiskalnej" — formułować jako **„niepotwierdzone"**,
  nie „nie ma"
- Finansowanie konkurencji: Sunday €18 mln Series B, Qlub $72 mln — źródła wtórne
- „MF potwierdził możliwość integracji aplikacji zewnętrznych z HUB Paragonowy" — z FAQ,
  **zweryfikować bezpośrednio przed decyzjami architektonicznymi** (`DEC-005`)
- „~1 mln Ukraińców w PL", „NTAG213 ≈ 1,50 zł", „sankcje KSeF nie stosowane do 31.12.2026" —
  prawdopodobne, nieprzeweryfikowane osobno

---

## 7. Powiązane dokumenty

- Zakres i wydania → [`01_Produkt_Zakres_Roadmapa.md`](01_Produkt_Zakres_Roadmapa.md)
- Reguły biznesowe realizujące `LEG-*` → [`03_Model_Domenowy.md`](03_Model_Domenowy.md) §7
- Wymagania wobec PSP i POS → [`04_Architektura_Moduly.md`](04_Architektura_Moduly.md) §7
- Ekrany, których dotyczą `TUN-*` → [`06`](06_Ekrany_Gosc.md), [`07`](07_Ekrany_Kelner_KDS.md), [`08`](08_Ekrany_Panel.md)
