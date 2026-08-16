# 09 · Ekrany i funkcje v2–v3

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md), katalog funkcji z [`01`](01_Produkt_Zakres_Roadmapa.md) §4.5–4.6.

**Poziom szczegółowości:** ekrany kluczowe i przepływy, **bez pełnego rozpisania każdego stanu**.
Uzasadnienie: projektowanie wszystkich stanów ekranu, który powstanie za 12–18 miesięcy, to praca
do wyrzucenia. Pokrycie produktowe, domenowe i architektoniczne pozostaje pełne — bo od niego
zależą decyzje podejmowane **dziś**.

**Najważniejsza część tego dokumentu to §8** — ograniczenia, które te funkcje nakładają na
architekturę v0/v1. Jeśli czytasz tylko jeden fragment, czytaj tamten.

---

## Spis ekranów

Identyfikatory zarezerwowane teraz, żeby nie kolidowały z v0/v1 ([`06`](06_Ekrany_Gosc.md),
[`07`](07_Ekrany_Kelner_KDS.md), [`08`](08_Ekrany_Panel.md)) i żeby dało się je przywołać
w zadaniu, zanim powstanie pełna makieta.

| ID | Ekran | Funkcja | Sekcja |
|---|---|---|---|
| `SCR-G-20` | Rezerwacja — wybór terminu i stolika | `F-S-005` | [§1](#1-f-s-005--rezerwacja-stolika-z-wyborem-miejsca) |
| `SCR-P-14` | Kalendarz rezerwacji (panel) | `F-S-005` | [§1](#1-f-s-005--rezerwacja-stolika-z-wyborem-miejsca) |
| `SCR-G-21` | Karta lojalnościowa gościa | `F-S-002` | [§2](#2-f-s-002--lojalność-i-stemple) |
| `SCR-P-15` | Konfiguracja programu lojalnościowego | `F-S-002` | [§2](#2-f-s-002--lojalność-i-stemple) |
| `SCR-P-16` | Kampanie automatyczne | `F-P-003` | [§3](#3-f-p-003--autokampanie) |
| `SCR-P-17` | Pulpit sieci | `F-P-008` | [§4](#4-f-p-008--multilokacja-i-tryb-sieci) |
| `SCR-G-22` | Zamówienie na wynos z numerem odbioru | `F-S-001` | [§5](#5-f-s-001--kawiarnie--na-wynos-i-kolejka-z-numerem) |
| `SCR-D-04` | Ekran nad ladą — numery do odbioru | `F-S-001` | [§5](#5-f-s-001--kawiarnie--na-wynos-i-kolejka-z-numerem) |
| `SCR-G-23` | Food court — wiele kuchni, jeden rachunek | `F-S-003` | [§6](#6-f-s-003--ogródki-piwne-food-courty-eventy) |
| `SCR-G-24` | Hotel — zamawianie z pokoju | `F-S-004` | [§7](#7-f-s-004--hotele-room-service-spa) |
| `SCR-P-18` | Konfiguracja integracji PMS | `F-S-004` | [§7](#7-f-s-004--hotele-room-service-spa) |

⚠️ Ekrany v3 (`F-X-001` … `F-X-003`) **nie mają zarezerwowanych identyfikatorów** — ich kształt
zależy od rozstrzygnięć `DEC-012` i `DEC-013`, więc nadawanie numerów teraz byłoby zgadywaniem.

---

## 1. `F-S-005` · Rezerwacja stolika z wyborem miejsca

**Wydanie:** v2 · **Ekrany:** `SCR-G-20`, `SCR-P-14`

> ⚠️ **Najwyżej oceniana technologia przez polskich gości: 78%** (MFR 2025/2026) — wyżej niż
> kioski samoobsługowe (72%). Jest w v2 **wyłącznie dlatego, że nie broni naszej fosy** —
> rezerwacje umie każdy, fiskalizacji i napiwków nie umie prawie nikt.
>
> Jeśli sprzedaż pokaże, że rezerwacja domyka transakcje, przesunięcie do v1 jest uzasadnione (`DEC-011`).

### Ekran gościa — wybór miejsca

```
┌───────────────────────────────────────────┐
│  ←   Rezerwacja · Bar Zdrój               │
├───────────────────────────────────────────┤
│  ┌─────────┐ ┌─────────┐ ┌─────────┐      │
│  │ pt 16.08│ │ sb 17.08│ │ nd 18.08│      │
│  └─────────┘ └─────────┘ └─────────┘      │
│                                           │
│  Osób: [ − ]  4  [ + ]                    │
│                                           │
│  GODZINA                                  │
│  ┌──────┐┌──────┐┌──────┐┌──────┐         │
│  │18:00 ││18:30 ││19:00 ││19:30 │         │
│  └──────┘└──────┘└──────┘└──────┘         │
│   wolne   wolne   1 stol.  brak           │
│                                           │
│  WYBIERZ STOLIK                           │
│  ┌─────────────────────────────────────┐  │
│  │  ┌──┐ ┌──┐ ┌──┐    ┌────┐  ┌────┐   │  │
│  │  │▒▒│ │ 2│ │▒▒│    │  7 │  │▒▒▒▒│   │  │
│  │  └──┘ └──┘ └──┘    └────┘  └────┘   │  │
│  │   zaj.  wolny zaj.   WYBRANY  zaj.  │  │
│  │                                     │  │
│  │  ┌──┐ ┌──┐         ┌──────────┐     │  │
│  │  │ 4│ │ 5│         │    12    │     │  │
│  │  └──┘ └──┘         └──────────┘     │  │
│  │   okno  okno        przy barze      │  │  ← charakterystyka miejsca
│  └─────────────────────────────────────┘  │
│                                           │
│   Stolik 7 · 4 osoby · przy oknie         │
├───────────────────────────────────────────┤
│         REZERWUJĘ NA 19:00                │
└───────────────────────────────────────────┘
```

**Kluczowa różnica wobec konkurencji:** gość wybiera **konkretny stolik**, nie tylko godzinę.
To jest źródło oceny 78% — poczucie kontroli, nie sama możliwość rezerwacji.

### Domknięcie pętli produktowej

Rezerwacja tworzy `TableSession` w stanie `Zarezerwowana`, powiązaną z `GuestProfile` (szczebel T3).
Przy przyjściu gość skanuje kod stolika i **od razu jest rozpoznany** — bez ponownego podawania
danych. To spina rezerwację z CRM (`F-P-001`) i „smak pamięta" (`F-G-015`) w jedną całość,
której nie da się skopiować bez posiadania obu końców.

### Nowe encje

`ENT-Reservation` — `venue_id`, `table_id`, `guest_profile_id`, `party_size`, `starts_at`,
`duration_minutes`, `state`, `source`, `no_show_at`
`ENT-TableAvailabilityRule` — reguły blokowania, minimalne odstępy, bufory między rezerwacjami

⚠️ **Nowy stan w `TableSession`:** `Zarezerwowana` → `Otwarta`. Wymaga rozszerzenia maszyny
stanów z [`03`](03_Model_Domenowy.md) §4.1 — dlatego stan musi być przewidziany w projekcie od v0
(patrz §8).

---

## 2. `F-S-002` · Lojalność i stemple

**Wydanie:** v2 · **Ekrany:** `SCR-G-21`, `SCR-P-15`

```
┌───────────────────────────────────────────┐
│  ←   Twoja karta · Bar Zdrój              │
├───────────────────────────────────────────┤
│              ☕ ☕ ☕ ☕ ☕                  │
│              ☕ ☕ ○ ○ ○                   │
│                                           │
│         7 z 10 · jeszcze 3 kawy           │
│           i dziesiąta gratis              │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  🎁 Masz nagrodę do odebrania!       │  │
│  │  Deser gratis — ważne do 30.08       │  │
│  │              [ ODBIERAM ]            │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  HISTORIA                                 │
│  16.08  Kawa + sernik        +1 stempel   │
│  12.08  Latte                +1 stempel   │
└───────────────────────────────────────────┘
```

**Warunek działania:** wymaga tożsamości na szczeblu **T3** (zweryfikowany telefon) z drabiny
w [`03`](03_Model_Domenowy.md) §5. Token urządzenia nie wystarczy — ginie w trybie prywatnym,
a gość, który straci 7 stempli, przestanie ufać całemu systemowi.

**Segment docelowy:** kawiarnie i fast casual, gdzie częstotliwość wizyt jest wysoka,
a wartość pojedynczej niska. W restauracji full-service stemple nie mają sensu.

---

## 3. `F-P-003` · Autokampanie

**Wydanie:** v2 · **Ekran:** `SCR-P-16`

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Kampanie automatyczne                                  [ + NOWA KAMPANIA ]│
├────────────────────────────────────────────────────────────────────────────┤
│  Nazwa              Wyzwalacz             Wysłane  Powrócili   Przychód    │
│  ────────────────────────────────────────────────────────────────────────  │
│  ● Tęsknimy         30 dni bez wizyty       284      41 (14%)   3 480 zł   │
│  ● Urodziny         dzień urodzin            62      38 (61%)   4 120 zł   │
│  ● Tylko na lunch   3 wizyty tylko 12–15     97      12 (12%)   1 240 zł   │
│  ○ Wieczór piątkowy piątek 16:00             —        —            —       │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  ⓘ Wysyłamy wyłącznie do gości ze zgodą na dany kanał.               │  │
│  │    Ze zgodą na SMS: 1 402 · na e-mail: 1 118                         │  │
│  │    Zgody zbierane przy zamówieniu — to jedyne legalne okno.          │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

⚠️ **Ograniczenie prawne definiuje tę funkcję.** Art. 398 PKE (od 10.11.2024) wymaga uprzedniej
zgody i **zabrania nawiązywania kontaktu, żeby dopiero o zgodę poprosić**. Nie da się wysłać
kampanii „reaktywacyjnej" do gości bez zgody — nawet jeśli mamy ich numer z rachunku.

**Konsekwencja dla v1:** ekran zgód (`SCR-G-16`) jest jedynym źródłem zasilania tej funkcji.
Jeśli zaprojektujemy go źle, cała funkcja v2 będzie miała pustą bazę odbiorców. Dlatego zgody
są w v1, a nie razem z kampaniami.

**Kolumna „Przychód" jest obowiązkowa** — kampania bez przypisanego przychodu to koszt, którego
właściciel nie obroni (zasada Z5).

---

## 4. `F-P-008` · Multilokacja i tryb sieci

**Wydanie:** v2 · **Ekran:** `SCR-P-17` · **Najdroższy segment**

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Sieć Zdrój · 6 lokali                       [ sierpień 2026 ▾ ]           │
├────────────────────────────────────────────────────────────────────────────┤
│  Lokal            Sprzedaż   Rotacja   Przez QR   Śr.rach.   Ocena         │
│  ────────────────────────────────────────────────────────────────────────  │
│  Kraków Rynek    186 400 zł  1h 04m      54%       92 zł      4,7          │
│  Kraków Kazim.   142 100 zł  1h 12m      47%       87 zł      4,6          │
│  Wrocław Rynek   138 900 zł  1h 09m      51%       89 zł      4,8          │
│  Gdańsk Długa    121 200 zł  1h 21m      38%       81 zł      4,4          │
│  Warszawa Hoża    98 400 zł  1h 33m      29%       76 zł      4,2          │
│  Poznań Stary     87 100 zł  1h 28m      31%       74 zł      4,3          │
│  ────────────────────────────────────────────────────────────────────────  │
│  RAZEM           774 100 zł  1h 18m      42%       83 zł      4,5          │
│                                                                            │
│  ⓘ Warszawa Hoża: najniższy udział QR i najdłuższa rotacja.                │
│    Zwykle oznacza to, że personel nie promuje systemu.  [ ZOBACZ LOKAL ]   │
│                                                                            │
│  WSPÓLNE MENU                                                              │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Karta sieciowa · 96 pozycji · wersja 8                              │  │
│  │  Ceny ustalane osobno per lokal.        [ EDYTUJ KARTĘ SIECIOWĄ ]    │  │
│  │  Burger Zdrój:  Kraków 38 zł · Warszawa 44 zł · Poznań 36 zł         │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Wspólny katalog, lokalne ceny** (`RULE-011`) — sieć dzieli pozycje, nie cenniki. Warszawa ma
inne czynsze niż Poznań i to musi być widoczne w modelu, nie obchodzone ręcznie.

**Diagnostyka po udziale QR** to najcenniejsza kolumna dla właściciela sieci. Niski udział przy
identycznym produkcie prawie zawsze oznacza sabotaż personelu — dokładnie to ryzyko, przeciw
któremu zbudowana jest cała powierzchnia Kelner Pro (zasada Z2).

---

## 5. `F-S-001` · Kawiarnie — na wynos i kolejka z numerem

**Wydanie:** v2 · **Ekrany:** `SCR-G-22`, `SCR-D-04`

```
   EKRAN GOŚCIA                        EKRAN NAD LADĄ
┌──────────────────────┐    ┌──────────────────────────────────┐
│  Zamówienie #47      │    │        ODBIERZ ZAMÓWIENIE        │
│                      │    │                                  │
│      ┌──────┐        │    │   GOTOWE            W PRZYGOT.   │
│      │  47  │        │    │   ┌────┐ ┌────┐    ┌────┐ ┌────┐│
│      └──────┘        │    │   │ 44 │ │ 45 │    │ 47 │ │ 48 ││
│                      │    │   └────┘ └────┘    └────┘ └────┘│
│   Gotowe za ~4 min   │    │   ┌────┐                        │
│                      │    │   │ 46 │                        │
│  ● ─── ● ─── ○       │    │   └────┘                        │
│  Przyj. W przyg. Got.│    │                                  │
│                      │    └──────────────────────────────────┘
│  Powiadomimy Cię.    │
│  Możesz usiąść.      │       Numery 96px. Czytelne z 5 m.
└──────────────────────┘
```

**Zmiana modelu:** zamówienie nie jest związane ze stolikiem, tylko z **numerem odbioru**.
`TableSession` istnieje, ale `table_id` jest puste, a rolę identyfikatora przejmuje
`pickup_number`. Dlatego relacja sesja–stolik musi być **opcjonalna od początku** (patrz §8).

Dochodzi `F-S-002` (stemple) i „stała kawa w jednym tapnięciu" — w kawiarni gość zamawia
to samo w 90% przypadków, więc `F-G-007` staje się funkcją główną, nie pomocniczą.

---

## 6. `F-S-003` · Ogródki piwne, food courty, eventy

**Wydanie:** v2 · **Ekran:** `SCR-G-23`

To rozszerzenie **beachheadu** z koncepcji (§8.1) — pierwszego segmentu, od którego zaczynamy
sprzedaż. Funkcje segmentowe przychodzą w v2, ale sam segment obsługujemy od v0.1.

### Tryb bez kelnera

```
┌───────────────────────────────────────────┐
│  Festiwal Smaków · Stanowisko 12          │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │  Zamówienie #128                     │  │
│  │  🍔 Burger Truck    ✓ gotowe         │  │
│  │  🍺 Bar Zdrój       🔥 w przygot.    │  │
│  │  🌮 Taco Loco       ⏳ w kolejce      │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  Powiadomimy, gdy wszystko będzie gotowe. │
│  Odbierasz sam — nie ma obsługi kelnerskiej│
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  Burger Truck jest gotowy!           │  │
│  │  Odbierz w stanowisku 3.             │  │
│  └─────────────────────────────────────┘  │
└───────────────────────────────────────────┘
```

**Kilka kuchni na jeden rachunek** to najgłębsza zmiana modelu w całym v2. `Bill` przestaje
należeć do jednego `Venue` — wymaga `ENT-Vendor` w obrębie wydarzenia i podziału płatności
między wielu sprzedawców.

⚠️ **Konsekwencja prawna:** każdy sprzedawca fiskalizuje własną sprzedaż osobno. Nie wolno
wystawić jednego paragonu za pozycje trzech różnych podmiotów. Split w PSP musi rozdzielić
środki między sprzedawców **przed** fiskalizacją.

**Odporność na słaby zasięg** jest tu wymaganiem podstawowym, nie dodatkiem — na plenerowym
evencie sieć zawsze jest przeciążona. To argument za dopracowaniem `F-G-006` (kolejka offline)
już w v1.

---

## 7. `F-S-004` · Hotele, room service, SPA

**Wydanie:** v2 · **Ekrany:** `SCR-G-24`, `SCR-P-18`

```
┌───────────────────────────────────────────┐
│  Hotel Zdrój · Pokój 412                  │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │  Dzień dobry, Panie Kowalski.        │  │  ← rozpoznanie z PMS
│  │  Doliczymy do rachunku pokoju.       │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────┐ ┌─────────────┐          │
│  │ Room service│ │  Śniadanie  │          │
│  │  teraz      │ │  na jutro   │          │
│  └─────────────┘ └─────────────┘          │
│  ┌─────────────┐ ┌─────────────┐          │
│  │ Bar przy    │ │   SPA       │          │
│  │ basenie     │ │             │          │
│  └─────────────┘ └─────────────┘          │
│                                           │
│  ZAMÓWIENIE NA GODZINĘ                    │
│  ┌──────┐┌──────┐┌──────┐┌──────┐         │
│  │ teraz││ 7:30 ││ 8:00 ││ 8:30 │         │
│  └──────┘└──────┘└──────┘└──────┘         │
└───────────────────────────────────────────┘
```

**Nowa metoda płatności:** `room_charge` — dopisanie do rachunku pokoju przez PMS
(np. KWHotel). Wymaga rozszerzenia `Payment.method` i adaptera PMS w warstwie antykorupcyjnej,
tak samo jak POS ([`04`](04_Architektura_Moduly.md) §7).

**Zamówienie na konkretną godzinę** (śniadanie zamawiane wieczorem) wymaga
`Order.scheduled_for` — pola, którego nie ma w v0. Kolejkowanie do KDS musi umieć czekać.

QR nie jest przy stoliku, tylko **w pokoju, przy basenie, na leżaku**. `Table` przestaje
oznaczać stolik i staje się „punktem zamawiania" — kolejny argument za tym, żeby nie zaszywać
w modelu założenia „sesja = stolik".

---

## 8. Ograniczenia, które v2/v3 nakładają na v0/v1

> **To jest najważniejsza sekcja dokumentu.** Nie chodzi o to, żeby budować te funkcje teraz.
> Chodzi o to, żeby **nie zamknąć sobie do nich drzwi** decyzjami podejmowanymi w v0.

| # | Funkcja v2/v3 | Wymóg wobec projektu v0/v1 | Koszt ignorowania |
|---|---|---|---|
| **O1** | Rezerwacje (`F-S-005`) | Maszyna stanów `TableSession` musi dopuszczać stan `Zarezerwowana` **przed** `Otwarta` | Przebudowa rdzenia domeny przy działającym ruchu |
| **O2** | Food court, kawiarnie (`F-S-001`, `F-S-003`) | Relacja `TableSession` → `Table` **opcjonalna**. Sesja może być identyfikowana numerem odbioru albo pokojem | Migracja wszystkich sesji + przepisanie logiki wejścia |
| **O3** | Food court — wiele kuchni | `Bill` przygotowany na wielu sprzedawców. Nie zaszywać `venue_id` jako jedynego wymiaru rachunku | Przepisanie modułu rozliczeń i płatności |
| **O4** | Hotele (`F-S-004`) | `Payment.method` jako **rozszerzalny** zbiór wartości. `Order.scheduled_for` przewidziane w schemacie | Migracja tabeli płatności |
| **O5** | Multilokacja (`F-P-008`) | **Multitenancy od pierwszej migracji** (`D2`). Katalog rozdzielony od cennika (`RULE-011`) | Migracja całej bazy — najdroższy scenariusz ze wszystkich |
| **O6** | Lojalność, „smak pamięta" | **Drabina tożsamości** wdrożona od v0 (`D4`, [`03`](03_Model_Domenowy.md) §5) | Brak trwałej tożsamości gościa = funkcje niewykonalne |
| **O7** | Kampanie (`F-P-003`) | Zgody per kanał, wersjonowane, ze znacznikiem czasu (`RULE-023`) — zbierane od v1 | Pusta baza odbiorców. Zgody nie dają się zebrać wstecz (art. 398 PKE) |
| **O8** | Własna kasa (`F-X-001`) | `MOD-fiscal` za interfejsem `PosAdapter`, nie zaszyty w logice płatności | Przepisanie ścieżki fiskalnej — najbardziej wrażliwej prawnie |
| **O9** | Otwarte API (`F-X-002`) | Uprawnienia planu (`MOD-entitlements`) egzekwowane na granicy API od v0 (`P8`) | Brak możliwości bramkowania dostępu zewnętrznego |
| **O10** | Ekspansja CZ/SK/RO (`F-X-003`) | Waluta i stawki VAT jako **dane**, nie stałe w kodzie. `RULE-027` mówi „tylko PLN do v3" — ale schemat musi dopuszczać więcej | Przepisanie całej warstwy pieniędzy |

**Zasada nadrzędna:** różnica między „przewidziane w schemacie" a „zaimplementowane" jest
ogromna kosztowo. Kolumna, której nie używamy, kosztuje nic. Migracja tabeli płatności na
produkcji kosztuje tydzień i ryzyko rozjazdu sald.

---

## 9. Wydanie v3 — fosa

### `F-X-001` · Własna certyfikowana kasa wirtualna (GUM)

Największy krok strategiczny całego produktu.

| Aspekt | Znaczenie |
|---|---|
| **Co daje** | Sprzedaż lokalom **bez POS-a w ogóle**. Przejęcie pełnego stosu. Koniec zależności od 6–8 integracji |
| **Dlaczego to fosa** | Certyfikacja GUM to 6–12 miesięcy pracy, której żaden gracz globalny nie wykona dla ~15 tys. potencjalnych lokali. To dokładnie ta bariera, która trzyma poza Polską Sunday, Qlub, Toast i SkyTab |
| **Wymóg architektoniczny** | `MOD-fiscal` musi być od v0 za interfejsem adaptera (`O8`), żeby własna kasa była **kolejnym adapterem**, nie przepisaniem |
| **Ryzyko** | Wejście w rolę dostawcy urządzenia fiskalnego zmienia profil odpowiedzialności prawnej. Wymaga osobnej analizy przed decyzją (`DEC-013`) |

### `F-X-002` · Otwarte API i marketplace integracji

⚠️ **Ostrzeżenie regulacyjne.** Ewolucja w kierunku marketplace'u uruchamia obowiązki
**DAC7** — raportowanie do Szefa KAS, kara do 1 mln zł. W modelu czysto SaaS ryzyko jest niskie,
ale otwarty marketplace zmienia kwalifikację. Do analizy przed uruchomieniem (`LEG-013`, `DEC-012`).

Dodatkowo **P2B (UE) 2019/1150**: przejrzystość warunków dla użytkowników biznesowych,
15 dni na powiadomienie o zmianach, wewnętrzny system rozpatrywania skarg. Często pomijane
przez startupy (`LEG-015`).

### `F-X-003` · Ekspansja: Czechy, Słowacja, Rumunia

Wybór tych rynków nie jest przypadkowy: **podobna złożoność fiskalna = ta sama fosa**.
Praca włożona w polską fiskalizację jest metodologicznie przenośna, a bariera wejścia dla
graczy globalnych pozostaje identyczna.

**Wymóg wobec v0:** waluta i stawki VAT jako dane w bazie, nie stałe w kodzie (`O10`).

---

## 10. Pozostałe funkcje v2

| ID | Funkcja | Uwagi |
|---|---|---|
| `F-G-014` | Zamawianie głosowe | Podwójne uzasadnienie: dostępność (`LEG-011`) i szybkość w ciemnym, głośnym barze. Warstwa wejścia nad istniejącym filtrem `F-G-009` — nie osobny przepływ |
| `F-G-015` | „Smak pamięta" | `Ostatnio brałeś Żywiec i burgera. To samo?` Wymaga tożsamości T3. Rozszerzenie `F-G-007` o kontekst historii, nie nowy ekran |
| `F-G-022` | Quiz oczekiwania | Brandowany miniquiz stolika, nagroda = zniżka na deser. Zamienia czas oczekiwania w dosprzedaż. Niski priorytet — miła funkcja, nie argument sprzedażowy |
| `F-P-006` | Mapa cieplna stolików i godzin | Które stoliki i w jakich godzinach ile przynoszą. Podstawa planowania zmian i przemeblowania sali |
| `F-P-016` | Prognoza odpisów i zakupów | Przewidywanie zużycia z historii sprzedaży. Wymaga danych z ≥ 6 miesięcy — dlatego dopiero v2 |

---

## Powiązane dokumenty

- Katalog funkcji z przypisaniem wydań → [`01_Produkt_Zakres_Roadmapa.md`](01_Produkt_Zakres_Roadmapa.md) §4
- Model domenowy, który te funkcje rozszerzają → [`03_Model_Domenowy.md`](03_Model_Domenowy.md)
- Adaptery, do których dołączą PMS i własna kasa → [`04_Architektura_Moduly.md`](04_Architektura_Moduly.md) §7
- Otwarte decyzje `DEC-011` … `DEC-013` → [`10_Tuning_Decyzje_Ryzyka.md`](10_Tuning_Decyzje_Ryzyka.md) §3
