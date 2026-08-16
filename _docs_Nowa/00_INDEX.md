# 00 · INDEX — mapa dokumentacji produktowej

**Produkt:** zamawianie przy stoliku przez QR/NFC dla rynku polskiego
**Stan:** dokumentacja produktowa v1.0 — podstawa do implementacji
**Stos technologiczny:** Next.js (PWA) + NestJS + PostgreSQL (Node/TypeScript)
**Język:** dokumentacja i copy UI — polski. Identyfikatory encji i zdarzeń — angielski.

---

## 1. Czym jest ten katalog

`_docs_Nowa/` to **dokumentacja produktowa i projektowa** — przepływy użytkowników, model
domenowy, granice modułów, system projektowy i makiety ekranów.

Powstała na bazie `../01_Koncepcja_produktu.md`, ale **nie jest jej powtórzeniem**. Tamten
dokument opisuje **rynek, konkurencję, monetyzację i prawo** i pozostaje jedynym źródłem prawdy
w tych obszarach. Ten katalog opisuje **co budujemy i jak to działa**.

> **Zasada niepowielania.** Liczby rynkowe, analiza konkurencji, cennik i uzasadnienie biznesowe
> **nie są tu duplikowane**. Jeśli potrzebujesz danych rynkowych — czytaj `01_Koncepcja_produktu.md`.
> Jeśli potrzebujesz wiedzieć, jak zachowuje się ekran koszyka — czytaj tutaj.

---

## 2. Jak używać jako kontekst w kolejnych sesjach

Nie wczytuj całego katalogu. Wczytaj `00_INDEX.md` + dokumenty właściwe dla zadania:

| Rodzaj zadania | Wczytaj |
|---|---|
| Rozumienie produktu od zera | `00`, `01` |
| Projektowanie / zmiana przepływu użytkownika | `00`, `01`, `02` |
| Praca nad bazą danych, encjami, statusami | `00`, `03` |
| Nowy moduł, API, zdarzenie, integracja | `00`, `03`, `04` |
| Ekran gościa (PWA) | `00`, `05`, `06`, + `03` jeśli dotyka stanów |
| Ekran kelnera lub KDS | `00`, `05`, `07` |
| Panel managera, onboarding lokalu | `00`, `05`, `08` |
| Funkcje v2/v3 (rezerwacje, lojalność, sieć, hotel) | `00`, `01`, `09` |
| Płatności, napiwki, fiskalizacja | `00`, `03`, `04`, `10` + `01_Koncepcja` §6 |
| Decyzja produktowa / zmiana priorytetu | `00`, `01`, `10` |
| Optymalizacja wyglądu lub logiki biznesowej | `00`, `10` |

**Zawsze podawaj `00_INDEX.md`** — zawiera konwencje ID i glosariusz, bez których pozostałe
dokumenty czyta się gorzej.

---

## 3. Spis dokumentów

| Dokument | Zawartość | Zakres wydań |
|---|---|---|
| [`00_INDEX.md`](00_INDEX.md) | Ten plik. Nawigacja, konwencje ID, glosariusz, delta wobec koncepcji | — |
| [`01_Produkt_Zakres_Roadmapa.md`](01_Produkt_Zakres_Roadmapa.md) | Pozycjonowanie, zasady produktowe, katalog funkcji `F-*`, rozbicie na wydania, metryki | v0–v3 |
| [`02_Aktorzy_Scenariusze.md`](02_Aktorzy_Scenariusze.md) | Aktorzy, persony, przepływy end-to-end, przypadki brzegowe | v0–v3 |
| [`03_Model_Domenowy.md`](03_Model_Domenowy.md) | Encje `ENT-*`, ERD, maszyny stanów, reguły `RULE-*`, zasady operacji na pieniądzach | v0–v3 |
| [`04_Architektura_Moduly.md`](04_Architektura_Moduly.md) | Moduły, **interakcja międzymodułowa**, katalog zdarzeń `EVT-*`, realtime, adaptery POS/PSP | v0–v3 |
| [`05_System_Projektowy.md`](05_System_Projektowy.md) | Tokeny, typografia, kolor, komponenty, WCAG 2.1 AA, budżet wydajności, tone of voice | — |
| [`06_Ekrany_Gosc.md`](06_Ekrany_Gosc.md) | Makiety PWA gościa | v0–v1 |
| [`07_Ekrany_Kelner_KDS.md`](07_Ekrany_Kelner_KDS.md) | Makiety Kelner Pro i KDS | v0–v1 |
| [`08_Ekrany_Panel.md`](08_Ekrany_Panel.md) | Makiety panelu managera i onboardingu lokalu | v0–v1 |
| [`09_Ekrany_v2_v3.md`](09_Ekrany_v2_v3.md) | Rezerwacje, lojalność, sieć, food court, hotel, kasa własna | v2–v3 |
| [`10_Tuning_Decyzje_Ryzyka.md`](10_Tuning_Decyzje_Ryzyka.md) | Kandydaci do tuningu `TUN-*`, otwarte decyzje `DEC-*`, ograniczenia prawne `LEG-*` | v0–v3 |

---

## 4. Konwencje identyfikatorów

Identyfikatory są **stabilne i przekrojowe** — służą do odwoływania się między dokumentami
i do zadawania precyzyjnych zadań („zaimplementuj `F-G-014` zgodnie z `SCR-G-07`").

| Prefiks | Znaczenie | Przykład | Dokument źródłowy |
|---|---|---|---|
| `F-G-nnn` | Funkcja — powierzchnia **gościa** | `F-G-014` Podział rachunku | `01` |
| `F-K-nnn` | Funkcja — powierzchnia **kelnera** | `F-K-003` Tablica stolików | `01` |
| `F-D-nnn` | Funkcja — powierzchnia **kuchni** (display) | `F-D-002` Lista 86 | `01` |
| `F-P-nnn` | Funkcja — **panel** managera / właściciela | `F-P-011` Menu engineering | `01` |
| `SCR-G-nn` | Ekran gościa | `SCR-G-07` Podział rachunku | `06` |
| `SCR-K-nn` | Ekran kelnera | `SCR-K-02` Tablica stolików | `07` |
| `SCR-D-nn` | Ekran kuchni | `SCR-D-01` Kolejka zamówień | `07` |
| `SCR-P-nn` | Ekran panelu | `SCR-P-05` Kreator stolików | `08` |
| `ENT-Nazwa` | Encja domenowa | `ENT-TableSession` | `03` |
| `RULE-nnn` | Reguła biznesowa | `RULE-021` Zamknięcie sesji stolika | `03` |
| `EVT-modul.zdarzenie` | Zdarzenie domenowe | `EVT-order.placed` | `04` |
| `MOD-nazwa` | Moduł aplikacji | `MOD-payments` | `04` |
| `LEG-nnn` | Ograniczenie prawne przełożone na wymaganie | `LEG-006` Napiwki poza rachunkiem lokalu | `10` |
| `TUN-nnn` | Kandydat do tuningu wyglądu lub logiki | `TUN-004` Moment upsellu | `10` |
| `DEC-nnn` | Otwarta decyzja do podjęcia | `DEC-009` Wybór PSP | `10` |

**Zasada:** identyfikator raz nadany **nigdy nie jest zmieniany ani nie jest ponownie użyty**.
Funkcja usunięta z zakresu zostaje z adnotacją `[WYCOFANE]`, a jej numer nie wraca do puli.

---

## 5. Glosariusz

Terminy domenowe używane konsekwentnie w całej dokumentacji. Kolumna „W kodzie" to nazwa,
która ma trafić do encji, zdarzeń i API.

| Polski | W kodzie | Znaczenie |
|---|---|---|
| Lokal | `Venue` | Pojedynczy punkt gastronomiczny. Jednostka rozliczeniowa abonamentu |
| Sieć / najemca | `Tenant` | Właściciel jednego lub wielu lokali. Granica izolacji danych |
| Stolik | `Table` | Fizyczny stolik z przypisanym kodem QR/NFC |
| Sesja stolika | `TableSession` | Otwarty „pobyt" przy stoliku. Może mieć wielu uczestników. Rdzeń domeny |
| Uczestnik | `Participant` | Jedno urządzenie gościa w sesji stolika |
| Zamówienie | `Order` | Jedna partia pozycji wysłana do kuchni. Sesja ma wiele zamówień |
| Pozycja zamówienia | `OrderItem` | Konkretna pozycja z modyfikatorami |
| Rachunek | `Bill` | Zobowiązanie finansowe sesji. Może być dzielony |
| Podział rachunku | `BillSplit` | Sposób rozdzielenia rachunku między uczestników |
| Płatność | `Payment` | Pojedyncza transakcja w PSP |
| Napiwek | `Tip` | Kwota dla konkretnego kelnera, poza rachunkiem lokalu (`LEG-006`) |
| Wypłata napiwku | `TipPayout` | Przekazanie napiwku na konto kelnera przez split w PSP |
| Karta menu | `Menu` | Zestaw kategorii i pozycji, z wersjami językowymi |
| Pozycja menu | `MenuItem` | Danie lub napój |
| Modyfikator | `Modifier` | Opcja pozycji (rozmiar, dodatek, stopień wysmażenia) |
| Lista 86 | `Availability` | Pozycje chwilowo niedostępne — „skończyło się" |
| Serwowanie etapami | `Coursing` | Sterowanie kolejnością podania (przystawki teraz, dania później) |
| Wezwanie kelnera | `WaiterCall` | Zgłoszenie z ekranu gościa |
| Paragon | `Receipt` | Dokument fiskalny. Wystawia go **lokal**, nie my (`LEG-002`) |
| e-Paragon | `EReceipt` | Paragon elektroniczny przez HUB Paragonowy KAS |
| Uprawnienia planu | `Entitlement` | Mapowanie plan taryfowy → dostępne funkcje |
| Powierzchnia | — | Jedna z czterech aplikacji: gość, kelner, kuchnia, panel |

**Świadomie unikamy** słów: „klient" (mylące — klientem jest lokal, gościem jest konsument),
„user" bez kwalifikatora, „check" (kalka z angielskiego — mówimy „rachunek").

---

## 6. Delta wobec `01_Koncepcja_produktu.md`

Dokumentacja **nie jest przepisaniem koncepcji**. Poniżej dwanaście miejsc, w których świadomie
odchodzimy od niej lub ją uzupełniamy. Każde ma rozwinięcie we wskazanym dokumencie.

| # | Co zmieniamy | Dlaczego | Gdzie |
|---|---|---|---|
| **P1** | **v0 dzielimy na v0.1 (Order) i v0.2 (Pay)** | Koncepcja wkłada do v0 napiwki przez split, ale sama przyznaje (§10 p.9), że BLIK-split na wielu odbiorców **nie jest potwierdzony przez PSP**, a kwalifikacja podatkowa czeka na ORD-IN (§6.3). Upadek któregokolwiek zabija całe v0. v0.1 bez pieniędzy waliduje główne ryzyko — adopcję 40% | `01` |
| **P2** | **Tryb bez POS** jako pełnoprawny scenariusz | v0 w koncepcji wymaga integracji POS, a beachhead (ogródki piwne, tarasy) często POS-a nie ma. Wymóg tnie segment docelowy. Null-adapter jest architektonicznie darmowy | `01`, `04` |
| **P3** | **Domena „session-first" od v0** | Wspólny koszyk i podział rachunku są w koncepcji w v1, ale to najgłębsza decyzja architektoniczna. `TableSession` z N uczestnikami istnieje od pierwszego dnia, nawet gdy UI v0 pokazuje jednego gościa. Tak samo multitenancy (w koncepcji v2) | `03`, `04` |
| **P4** | **Drabina tożsamości gościa** | „Token urządzenia, bez rejestracji" nie udźwignie własnych funkcji koncepcji: tokenizacja karty, „smak pamięta", CRM i napiwek dla konkretnego kelnera cicho zakładają silniejszą tożsamość. Token ginie w trybie prywatnym | `03` |
| **P5** | **Formalny cykl życia sesji stolika** | Koncepcja nie mówi, kiedy sesja się kończy. Bez reguły następny gość przy tym stoliku zobaczy cudzy koszyk — realny błąd prywatności | `03` (`RULE-021`) |
| **P6** | **Stan `awaiting_staff_confirmation` dla alkoholu** | §6.4 opisuje potwierdzenie wieku przez personel, ale nie mówi, co gość widzi między zamówieniem a potwierdzeniem | `03`, `06` |
| **P7** | **Onboarding lokalu jako pełna powierzchnia** | Koncepcja ma 4 powierzchnie, ale nie opisuje, jak lokal w ogóle trafia do systemu: import menu, generowanie QR, konta personelu, parowanie POS, druk stojaków. Od tego zależy obietnica „szkolenie w 40 minut" | `08` |
| **P8** | **Moduł uprawnień planu od v0** | Plan Menu = 0 zł musi być realnie ograniczony, inaczej monetyzacja nie działa. Koncepcja traktuje to jak szczegół billingu | `04` |
| **P9** | **Uczciwe granice trybu offline** | Kolejka na urządzeniu gościa jest bezużyteczna, jeśli KDS też nie ma sieci. Koncepcja obiecuje za dużo | `02`, `06` |
| **P10** | **Drzewo decyzyjne HUB Paragonowy vs POS** | Jeśli fiskalizuje POS, to POS ma dane. Kto wysyła do HUB-a — my czy POS? Koncepcja stawia oba naraz bez rozstrzygnięcia | `10` (`DEC-005`) |
| **P11** | **Budżet wydajności rozbity na ekrany** | Cele „<20 s / <8 s" bez dekompozycji są życzeniem, nie wymaganiem | `05` |
| **P12** | **Podział rachunku jako parametr sterowany** | §7.3 p.2 sam wykazuje, że podział mnoży składową stałą i psuje marżę. Kolejność opcji i copy to dźwignia — opisujemy ją jawnie, a nie po cichu | `10` (`TUN-007`) |

---

## 7. Co pozostaje w `01_Koncepcja_produktu.md`

Nie przenosimy i nie duplikujemy:

- **§1–2** — dane rynkowe GUS, raport MADE FOR RESTAURANT, analiza konkurencji, zagrożenie Wolt
- **§6** — pełna analiza prawna. Tutaj występuje wyłącznie jako ograniczenia `LEG-*` w `10`
- **§7** — cennik, jednostkowa ekonomia, wrażliwość na miks BLIK
- **§8** — go-to-market, beachhead, kanały sprzedaży
- **§10** — lista rzeczy do zweryfikowania. Tutaj przechodzi w `DEC-*` w `10`

**Zastrzeżenie prawne przenosi się w całości:** część prawna koncepcji jest raportem
badawczym, nie poradą prawną. Punkty `[PRAWNIK]` i interpretacje ORD-IN wymagają potwierdzenia
przez polskiego prawnika lub doradcę podatkowego.

---

## 8. Otwarte sprawy organizacyjne

| Sprawa | Status |
|---|---|
| `0_Rastaran` leży **wewnątrz repozytorium git projektu Drukarnia ERP** (`git log` pokazuje `feat(commhub)`, `feat(pricing)` — inny produkt). Wymaga własnego repozytorium przed pierwszym commitem kodu | `DEC-014` w `10` |
