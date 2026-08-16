# 07 · Ekrany — Kelner Pro i KDS

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md), tokeny z [`05`](05_System_Projektowy.md).
> Uprawnienia: [`02`](02_Aktorzy_Scenariusze.md) §5.

**Kelner Pro** — motyw **zawsze ciemny**, obsługa jedną ręką, wszystko w zasięgu kciuka.
**KDS** — motyw **zawsze ciemny**, czytelność z 2 metrów, zero dekoracji, zero animacji.

> Ta powierzchnia realizuje zasadę **Z2 — kelner nigdy nie traci**. To ona decyduje o adopcji.
> Kelner, który uzna, że system mu szkodzi, powie gościom „lepiej zamówić u mnie" — i wdrożenie
> umrze niezależnie od jakości pozostałych ekranów.

---

## Spis ekranów

| ID | Ekran | Wydanie |
|---|---|---|
| [`SCR-K-01`](#scr-k-01--start-zmiany) | Start zmiany | v0.1 |
| [`SCR-K-02`](#scr-k-02--tablica-stolików) | Tablica stolików | v0.1 |
| [`SCR-K-03`](#scr-k-03--szczegóły-stolika) | Szczegóły stolika | v0.1 |
| [`SCR-K-04`](#scr-k-04--przyjmowanie-zamówienia) | Przyjmowanie zamówienia | v0.1 |
| [`SCR-K-05`](#scr-k-05--potwierdzenie-wieku) | Potwierdzenie wieku | v0.1 |
| [`SCR-K-06`](#scr-k-06--rozliczenie-i-zamknięcie-sesji) | Rozliczenie i zamknięcie sesji | v0.1 |
| [`SCR-K-07`](#scr-k-07--moje-napiwki) | Moje napiwki | v0.2 |
| [`SCR-K-08`](#scr-k-08--podsumowanie-zmiany) | Podsumowanie zmiany | v0.2 |
| [`SCR-D-01`](#scr-d-01--kolejka-kuchni) | KDS · Kolejka kuchni | v0.1 |
| [`SCR-D-02`](#scr-d-02--lista-86) | KDS · Lista 86 | v0.1 |
| [`SCR-D-03`](#scr-d-03--stan-połączenia) | KDS · Stan połączenia | v0.1 |

---

# Część I · Kelner Pro

## `SCR-K-01` · Start zmiany

**Realizuje:** `F-K-006`, przypisanie sekcji

```
┌───────────────────────────────────────────┐
│                                           │
│              Bar Zdrój                    │
│                                           │
│           ┌─────────────┐                 │
│           │   ▓▓▓▓▓▓▓   │                 │
│           │   ▓▓▓▓▓▓▓   │                 │
│           └─────────────┘                 │
│               Marek                       │
│                                           │
│  TWOJA SEKCJA DZIŚ                        │
│  ┌─────────────────────────────────────┐  │
│  │ (•) Sala główna     stoliki 1–14    │  │
│  │ ( ) Taras           stoliki 15–24   │  │
│  │ ( ) Bar             stoliki 25–30   │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  ⚠ Konto napiwków niezweryfikowane   │  │  ← v0.2
│  │  Goście nie zobaczą opcji napiwku,   │  │
│  │  dopóki nie dokończysz weryfikacji.  │  │
│  │  ▸ Dokończ weryfikację               │  │
│  └─────────────────────────────────────┘  │
│                                           │
├───────────────────────────────────────────┤
│            ZACZYNAM ZMIANĘ                │
└───────────────────────────────────────────┘
```

**Ostrzeżenie o koncie napiwków jest kluczowe.** Bez zweryfikowanego konta ekran napiwku
w ogóle nie jest pokazywany gościowi (`SCR-G-10`) — bo alternatywa, czyli zbieranie napiwku
„na później", to pooling, który przekwalifikowuje napiwek na przychód ze stosunku pracy
z pełnym PIT i ZUS (`LEG-006`). Kelner musi rozumieć, że traci pieniądze, dopóki tego nie zrobi.

---

## `SCR-K-02` · Tablica stolików

**Realizuje:** `F-K-003`, `F-K-004` · **Ekran główny.** Kelner patrzy tu dziesiątki razy na zmianę.
Musi dać się odczytać **jednym spojrzeniem w ciemności**.

```
┌───────────────────────────────────────────┐
│  Sala główna · Marek        🔔 2    ⋮     │
├───────────────────────────────────────────┤
│  ┌────────────┐  ┌────────────┐           │
│  │  🔔 12     │  │     7      │           │  ← 12 woła: danger, pulsuje
│  │  WOŁA      │  │  Czeka na  │           │
│  │  0:42      │  │  rachunek  │           │
│  │            │  │  25:10  ⚠  │           │  ← 7: przekroczony czas
│  │  79,00 zł  │  │  142,00 zł │           │
│  └────────────┘  └────────────┘           │
│  ┌────────────┐  ┌────────────┐           │
│  │     3      │  │     5      │           │
│  │  W kuchni  │  │  Zamawia   │           │
│  │  8:15      │  │  2:30      │           │
│  │  65,00 zł  │  │  —         │           │
│  └────────────┘  └────────────┘           │
│  ┌────────────┐  ┌────────────┐           │
│  │     1      │  │     2      │           │
│  │   Wolny    │  │   Wolny    │           │  ← wyciszone
│  └────────────┘  └────────────┘           │
│                            ⋮              │
├───────────────────────────────────────────┤
│  🍽 Stoliki   🧾 Rachunki   💰 Napiwki    │  ← nawigacja w zasięgu kciuka
└───────────────────────────────────────────┘
```

### Kolejność sortowania — czyli co naprawdę robi ten ekran

Kafelki **nie są ułożone według numerów stolików**. Są ułożone według **pilności**.
To jest różnica między narzędziem a listą.

| Priorytet | Stan | Kolor | Zachowanie |
|---|---|---|---|
| 1 | Wezwanie kelnera | `danger`, pulsujący | Zawsze na górze, z licznikiem od zgłoszenia |
| 2 | Rachunek > 25 min bez płatności | `warning` + ⚠ | Ryzyko wyjścia bez zapłaty (`E3`) |
| 3 | Zamówienie gotowe do podania | `success` | Danie stygnie |
| 4 | Alkohol czeka na potwierdzenie | `info` + 🍺 | Blokuje realizację (`RULE-008`) |
| 5 | W kuchni / zamawia | neutralny | Kontekst |
| 6 | Wolny | wyciszony | Tło |

### Stany

| Stan | Treść |
|---|---|
| Wszystko spokojne | `Wszystko pod kontrolą.` + siatka wyciszonych stolików. Bez pustej ilustracji |
| Offline | Tablica z ostatniego stanu + `Brak połączenia · dane sprzed 2 min`. **Odczyt działa offline** |
| Poza zmianą | `Nie masz aktywnej zmiany.` + `Zacznij zmianę` |
| Stolik przypisany komuś innemu | Widoczny, wyszarzony, z inicjałami kolegi. Bez możliwości działania |

### Kryteria akceptacji

1. Interaktywna ≤ 1,5 s od otwarcia.
2. Czytelna z odległości wyciągniętej ręki w ciemnym lokalu.
3. Wszystkie działania osiągalne kciukiem jednej ręki — nawigacja u dołu.
4. Aktualizacja realtime bez przeskoków układu.
5. Odczyt działa offline z ostatniego stanu.

---

## `SCR-K-03` · Szczegóły stolika

```
┌───────────────────────────────────────────┐
│  ←   Stolik 12 · 3 osoby        79,00 zł  │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ 🔔 WOŁA OD 0:42                      │  │
│  │           [ IDĘ ]                    │  │  ← 56px, pierwsze
│  └─────────────────────────────────────┘  │
├───────────────────────────────────────────┤
│  Kasia · Marek · Ola      od 20:12 (46m)  │
├───────────────────────────────────────────┤
│  KOLEJKA 2 · 20:51                        │
│  ┌─────────────────────────────────────┐  │
│  │ 🍺 2× Żywiec 0,5 l                   │  │
│  │    CZEKA NA POTWIERDZENIE WIEKU      │  │
│  │              [ POTWIERDŹ ]           │  │
│  ├─────────────────────────────────────┤  │
│  │ ✓ 1× Burger Zdrój      → dla Kasi   │  │  ← F-G-020
│  │   Średnio wysmażony, + Jalapeño      │  │
│  │   „bez cebuli"                       │  │
│  │   GOTOWE                             │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  KOLEJKA 1 · 20:34            ✓ podane    │
│  ▸ 3 pozycje                              │
│                                           │
├───────────────────────────────────────────┤
│  [ + ZAMÓWIENIE ]  [ 🧾 ROZLICZ ]         │
└───────────────────────────────────────────┘
```

**Uwagi dla kuchni i przypisanie do osoby są widoczne bez rozwijania.** „bez cebuli" musi być
widoczne przy podaniu, inaczej funkcja jest bezużyteczna. `→ dla Kasi` (`F-G-020`) sprawia, że
kelner podaje bez pytania „czyj to burger?" — i wygląda profesjonalnie.

---

## `SCR-K-04` · Przyjmowanie zamówienia

**Realizuje:** `F-K-005` — oszczędność ~1,5 h na zmianę
Ten sam model domenowy co zamówienie gościa (`S7`), inne wejście.

```
┌───────────────────────────────────────────┐
│  ←   Zamówienie · Stolik 12               │
├───────────────────────────────────────────┤
│  🔍 Szukaj                                │
├───────────────────────────────────────────┤
│ ┌─────┐┌──────┐┌─────┐┌──────┐┌────────   │
│ │ ⚡  ││ Piwo ││Przek││Dania ││ Napoje    │  ← ⚡ = najczęstsze
│ └─────┘└──────┘└─────┘└──────┘└────────   │
├───────────────────────────────────────────┤
│  NAJCZĘŚCIEJ ZAMAWIANE                    │  ← klucz do szybkości
│  ┌───────────────┐ ┌───────────────┐      │
│  │ Żywiec 0,5    │ │ Ciechan       │      │
│  │ 12,00 zł      │ │ 15,00 zł      │      │
│  │      [ + ]    │ │      [ + ]    │      │
│  └───────────────┘ └───────────────┘      │
│  ┌───────────────┐ ┌───────────────┐      │
│  │ Frytki        │ │ Burger Zdrój  │      │
│  │ 14,00 zł      │ │ 38,00 zł      │      │
│  │      [ + ]    │ │      [ + ]    │      │
│  └───────────────┘ └───────────────┘      │
│                            ⋮              │
├───────────────────────────────────────────┤
│  3 pozycje · 65,00 zł                     │
│           WYŚLIJ DO KUCHNI                │
└───────────────────────────────────────────┘
```

### Różnice wobec ekranu gościa

| Aspekt | Gość | Kelner |
|---|---|---|
| Wejście | Kategorie menu | **Siatka najczęściej zamawianych** — kelner zna menu na pamięć |
| Zdjęcia | Duże, sprzedają | **Brak** — zajmują miejsce, kelner ich nie potrzebuje |
| Alergeny | Obowiązkowo, wyeksponowane | Dostępne po dotknięciu — kelner pyta gościa ustnie |
| Modyfikatory | Panel z opisami | Zwarta lista, szybki wybór |
| Gęstość | Komfortowa | **Maksymalna** — dwie kolumny, niskie karty |

---

## `SCR-K-05` · Potwierdzenie wieku

**Realizuje:** `F-K-008` · **Wymóg prawny z wartością dowodową** (`LEG-010`)

```
┌───────────────────────────────────────────┐
│                    ▬▬▬                    │
│                                           │
│              🍺  Stolik 12                │
│                                           │
│           2× Żywiec 0,5 l                 │
│                                           │
│   Potwierdź, że sprawdziłeś wiek osoby,   │
│   której podajesz alkohol.                │
│                                           │
│   ┌─────────────────────────────────────┐ │
│   │      ✓  POTWIERDZAM                 │ │  ← success, 56px
│   └─────────────────────────────────────┘ │
│   ┌─────────────────────────────────────┐ │
│   │      ✕  ODMAWIAM PODANIA            │ │  ← danger
│   └─────────────────────────────────────┘ │
│                                           │
│   Potwierdzenie jest zapisywane           │
│   z Twoim nazwiskiem i godziną.           │  ← świadomość odpowiedzialności
│                                           │
└───────────────────────────────────────────┘

  ── po odmowie ──

   Powód odmowy
   ( ) Brak dokumentu
   ( ) Osoba niepełnoletnia
   ( ) Osoba nietrzeźwa
   ( ) Inny

   Pozycja zostanie zdjęta z rachunku,
   a gość zobaczy powód.
```

### Konstrukcja prawna

| Element | Uzasadnienie |
|---|---|
| Potwierdzenie **przy podaniu**, nie przy zamówieniu | Personel wykonuje art. 15 ust. 2 ustawy o wychowaniu w trzeźwości **osobiście i naocznie** |
| Zapis z nazwiskiem i znacznikiem czasu | Wartość dowodowa przy kontroli (`ENT-StaffConfirmation`) |
| Deklaracja gościa „mam 18+" **nie wystarcza** | Samodeklaracja nie realizuje obowiązku |
| Sprzedawcą alkoholu jest **lokal** | Posiadacz zezwolenia. Przyjęcie pieniędzy we własnym imieniu za pozycję alkoholową to przestępstwo z art. 43 ust. 1 uwt (`LEG-004`) |
| Odmowa wymaga powodu | Ochrona kelnera i lokalu przy sporze |

---

## `SCR-K-06` · Rozliczenie i zamknięcie sesji

**Realizuje:** `F-K-009`, `F-K-010` · Domyka lukę prywatności `P5`

```
┌───────────────────────────────────────────┐
│  ←   Rozliczenie · Stolik 12              │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ Rachunek                     79,00 zł│  │
│  │ Zapłacono online             41,00 zł│  │
│  │ ──────────────────────────────────── │  │
│  │ Do zapłaty                   38,00 zł│  │
│  └─────────────────────────────────────┘  │
│                                           │
│  JAK GOŚĆ PŁACI                           │
│  ┌─────────────────────────────────────┐  │
│  │ (•) Gotówka                          │  │
│  │ ( ) Terminal lokalu                  │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  ⚠ Pamiętaj o paragonie z kasy.      │  │  ← tryb bez POS
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │      ROZLICZ I ZAMKNIJ STOLIK        │  │
│  └─────────────────────────────────────┘  │
│                                           │
│   Zamknięcie zwolni stolik. Następny      │
│   gość zacznie z czystym rachunkiem.      │
└───────────────────────────────────────────┘
```

⚠️ **Dlaczego to jest ważniejsze, niż wygląda.** Bez ręcznego zamknięcia sesji następny gość
przy tym samym stoliku zobaczy koszyk i rachunek poprzednika. To błąd prywatności, nie usterka
UX (`RULE-021`). Automatyczne zamknięcie działa po opłaceniu całości, ale przy gotówce, sporze
albo przesiadce — decyduje kelner.

### Stany

| Stan | Treść |
|---|---|
| Rachunek opłacony w całości online | Sesja zamyka się sama. Ekran dostępny tylko do wglądu |
| Gość wyszedł bez zapłaty (`E3`) | `Zamknij jako nieopłacone` + obowiązkowy powód. Trafia do raportu strat |
| Zaległość fiskalna (`E4`) | Czerwony baner: `Płatność 41,00 zł nie została zafiskalizowana. Wystaw paragon ręcznie.` Zamknięcie możliwe, ale odnotowane |
| Przesiadka gościa (`E11`) | `Przenieś na inny stolik` zamiast zamknięcia |

---

## `SCR-K-07` · Moje napiwki

**Realizuje:** `F-K-001`, `F-K-002`, `F-K-007` · **Wydanie:** v0.2
**To jest ekran, który sprzedaje system kelnerowi.**

```
┌───────────────────────────────────────────┐
│  Moje napiwki                      ⋮      │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │  DZIŚ                                │  │
│  │            127,40 zł                 │  │  ← 36px, akcent
│  │            z 14 rachunków            │  │
│  │  ▲ 22% więcej niż średnia z wtorków  │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌──────────────┐  ┌──────────────┐       │
│  │ Ten tydzień  │  │ Ten miesiąc  │       │
│  │   612,00 zł  │  │  2 340,00 zł │       │
│  └──────────────┘  └──────────────┘       │
│                                           │
│  OSTATNIE                                 │
│  ┌─────────────────────────────────────┐  │
│  │ 21:04  Stolik 12          7,90 zł ✓ │  │
│  │ 20:41  Stolik 5          12,00 zł ✓ │  │
│  │ 20:15  Stolik 3           4,50 zł ⏳│  │  ← w drodze
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  RANKING · SIERPIEŃ                  │  │  ← v1
│  │  1. Ania          3 120 zł           │  │
│  │  2. Ty            2 340 zł           │  │
│  │  3. Piotr         1 890 zł           │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  Napiwki trafiają wprost na Twoje konto.  │
│  Lokal ich nie dotyka.                    │  ← powtarzane świadomie
├───────────────────────────────────────────┤
│  🍽 Stoliki   🧾 Rachunki   💰 Napiwki    │
└───────────────────────────────────────────┘
```

### Reguły

| Reguła | Uzasadnienie |
|---|---|
| Kelner widzi **wyłącznie własne** napiwki | Kwoty innych — nigdy. Ranking pokazuje sumy, ale bez rozbicia na rachunki (`02` §5) |
| Kelner **nigdy** nie widzi marż ani kosztów | Egzekwowane polityką **i** usuwaniem pól w API ([`04`](04_Architektura_Moduly.md) §8.3) |
| Zdanie o koncie powtarzane na ekranie | Zaufanie kelnera to warunek adopcji. Persona P2 pamięta poprzedni system, który zabrał mu napiwki |
| Ranking opcjonalny per lokal | W części zespołów grywalizacja szkodzi atmosferze. Manager może wyłączyć |

---

## `SCR-K-08` · Podsumowanie zmiany

```
┌───────────────────────────────────────────┐
│  ←   Zmiana 16.08 · 16:00–00:00           │
├───────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐       │
│  │ Obsłużone    │  │ Sprzedaż     │       │
│  │      23      │  │  4 120,00 zł │       │
│  │   stoliki    │  │              │       │
│  └──────────────┘  └──────────────┘       │
│  ┌──────────────┐  ┌──────────────┐       │
│  │ Napiwki      │  │ Mój upsell   │       │
│  │   287,40 zł  │  │    340,00 zł │       │  ← F-K-007
│  └──────────────┘  └──────────────┘       │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ Średni czas do pierwszego zamówienia │  │
│  │              2 min 40 s              │  │
│  │ ▼ 4 min szybciej niż przed QR        │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ⚠ DO ZAŁATWIENIA                         │
│  ┌─────────────────────────────────────┐  │
│  │ Stolik 7 — rachunek nierozliczony    │  │
│  │ 142,00 zł          [ ROZLICZ ]       │  │
│  └─────────────────────────────────────┘  │
│                                           │
├───────────────────────────────────────────┤
│            KOŃCZĘ ZMIANĘ                  │
└───────────────────────────────────────────┘
```

**Nie da się zakończyć zmiany z otwartymi rachunkami** bez jawnego rozliczenia albo oznaczenia
jako nieopłacone z powodem. Ta bramka pilnuje `RULE-021`.

---

# Część II · KDS

**Zasady bezwzględne dla KDS:**

1. Czytelność **z 2 metrów** — osobna skala typograficzna ([`05`](05_System_Projektowy.md) §3.2)
2. **Zero animacji** poza pulsowaniem przekroczonego licznika — ekran świeci 14 h
3. Status kodowany **kolorem, liczbą i grubością obramowania** — nigdy samym kolorem (WCAG 1.4.1)
4. Obsługa **mokrą ręką albo klawiaturą** — cele minimum 64 px
5. Bez przewijania poziomego, bez menu ukrytych pod ikonami

---

## `SCR-D-01` · Kolejka kuchni

**Realizuje:** `F-D-001`, `F-D-003`, `F-D-004` · Ekran poziomy, zwykle 24–32 cale

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  KUCHNIA · Bar Zdrój          ● połączono          21:04         [86] [⚙]   │
├──────────────────────────────────────────────────────────────────────────────┤
│  WSZYSTKIE (7)   │   GRILL (3)   │   ZIMNA (2)   │   BAR (2)                 │
├────────────────────┬────────────────────┬────────────────────┬───────────────┤
│ ┏━━━━━━━━━━━━━━━━┓ │ ┏━━━━━━━━━━━━━━━━┓ │ ┌────────────────┐ │ ┌───────────  │
│ ┃ STOLIK 7       ┃ │ ┃ STOLIK 12      ┃ │ │ STOLIK 3       │ │ │ STOLIK 5    │
│ ┃                ┃ │ ┃                ┃ │ │                │ │ │             │
│ ┃    11:03       ┃ │ ┃    07:48       ┃ │ │    04:12       │ │ │   01:20     │
│ ┃  PRZEKROCZONY  ┃ │ ┃    UWAGA       ┃ │ │   W NORMIE     │ │ │  W NORMIE   │
│ ┃                ┃ │ ┃                ┃ │ │                │ │ │             │
│ ┃ 2× Burger      ┃ │ ┃ 1× Burger      ┃ │ │ 1× Sałatka     │ │ │ 2× Żywiec   │
│ ┃   średnio      ┃ │ ┃   średnio      ┃ │ │   grecka       │ │ │             │
│ ┃   BEZ CEBULI   ┃ │ ┃   + jalapeño   ┃ │ │ 1× Talerz      │ │ │ 1× Cola     │
│ ┃ 1× Frytki      ┃ │ ┃   BEZ CEBULI   ┃ │ │   serów        │ │ │             │
│ ┃                ┃ │ ┃                ┃ │ │                │ │ │             │
│ ┃ ── razem ──    ┃ │ ┃ etap 2/2       ┃ │ │                │ │ │             │
│ ┃                ┃ │ ┃ za 20 min      ┃ │ │                │ │ │             │
│ ┠────────────────┨ │ ┠────────────────┨ │ ├────────────────┤ │ ├───────────  │
│ ┃    GOTOWE      ┃ │ ┃    GOTOWE      ┃ │ │    GOTOWE      │ │ │   GOTOWE    │
│ ┗━━━━━━━━━━━━━━━━┛ │ ┗━━━━━━━━━━━━━━━━┛ │ └────────────────┘ │ └───────────  │
│   obramowanie 6px  │   obramowanie 4px  │  obramowanie 2px   │               │
│   czerwony, puls   │   żółty            │  zielony           │               │
└────────────────────┴────────────────────┴────────────────────┴───────────────┘
```

### Elementy karty

| Element | Rozmiar | Uzasadnienie |
|---|---|---|
| Numer stolika | 34 px, pogrubiony | Pierwsza informacja, której szuka kucharz |
| Licznik | **48 px** | Największy element. Czytelny z 2 m bez wysiłku |
| Etykieta stanu | 20 px, wersaliki | Duplikuje kolor tekstem — WCAG 1.4.1 |
| Nazwy pozycji | 26 px | |
| Modyfikatory | 20 px, wcięte | |
| **Uwagi dla kuchni** | 20 px, **wersaliki, wyróżnione** | `BEZ CEBULI` — najczęstsze źródło reklamacji. Nie może zginąć |
| Przycisk gotowe | Wysokość 64 px, cała szerokość | Obsługa mokrą ręką |

### Kodowanie czasu

| Stan | Próg | Kolor | Obramowanie | Ruch |
|---|---|---|---|---|
| W normie | < 80% czasu normatywnego | `success` | 2 px | brak |
| Uwaga | 80–100% | `warning` | 4 px | brak |
| Przekroczony | > 100% | `danger` | 6 px | licznik pulsuje 1 Hz |

Czas normatywny pochodzi z `MenuItem.prep_time_minutes`, a rzeczywisty jest zapisywany
w `ENT-PrepTimeLog` — te dane zasilają uczciwe ETA dla gościa (`F-G-030`) i analitykę kuchni.

### Stany

| Stan | Treść |
|---|---|
| Brak zamówień | `Brak zamówień.` Godzina wielką czcionką. Zero dekoracji |
| Nowe zamówienie | Karta pojawia się **bez animacji**, z jednorazowym sygnałem dźwiękowym |
| Zamówienie anulowane | Karta czerwona z etykietą `ANULOWANE`, znika po potwierdzeniu przez kucharza |
| Coursing (`F-D-004`) | Etap 2 wyszarzony do czasu `serve_after_minutes`. Nagłówek `ETAP 2/2 · ZA 20 MIN` |
| Więcej kart niż mieści ekran | Przewijanie **pionowe** w kolumnie. Nigdy poziome |
| Pozycja alkoholowa niepotwierdzona | **Nie pojawia się na KDS w ogóle**, dopóki kelner nie potwierdzi (`RULE-008`) |

---

## `SCR-D-02` · Lista 86

**Realizuje:** `F-D-002` · Wywoływana przyciskiem `[86]` z paska górnego. Jedno tapnięcie.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  CO SIĘ SKOŃCZYŁO                                                   [ ✕ ]    │
├──────────────────────────────────────────────────────────────────────────────┤
│  🔍 Szukaj pozycji                                                           │
├──────────────────────────────────────────────────────────────────────────────┤
│  NIEDOSTĘPNE TERAZ (2)                                                       │
│  ┌────────────────────────────────┐  ┌────────────────────────────────┐      │
│  │  Tatar wołowy                  │  │  Łosoś z grilla                │      │
│  │  od 19:40                      │  │  od 20:15                      │      │
│  │  ┌──────────────────────────┐  │  │  ┌──────────────────────────┐  │      │
│  │  │       PRZYWRÓĆ           │  │  │  │       PRZYWRÓĆ           │  │      │
│  │  └──────────────────────────┘  │  │  └──────────────────────────┘  │      │
│  └────────────────────────────────┘  └────────────────────────────────┘      │
├──────────────────────────────────────────────────────────────────────────────┤
│  DOSTĘPNE                                                                    │
│  ┌──────────────────┐ ┌──────────────────┐ ┌──────────────────┐              │
│  │  Burger Zdrój    │ │  Sałatka grecka  │ │  Frytki          │              │
│  │  [ SKOŃCZYŁO SIĘ]│ │  [ SKOŃCZYŁO SIĘ]│ │  [ SKOŃCZYŁO SIĘ]│              │
│  └──────────────────┘ └──────────────────┘ └──────────────────┘              │
│                                    ⋮                                         │
└──────────────────────────────────────────────────────────────────────────────┘
```

### Skutki oznaczenia — zdarzenie o najszerszym zasięgu w systemie

Jedno tapnięcie uruchamia `EVT-menu.item_unavailable`, które dociera do **każdej otwartej sesji
w lokalu**:

| Odbiorca | Skutek |
|---|---|
| Menu wszystkich gości | Pozycja wyszarzona z etykietą `SKOŃCZYŁO SIĘ` (`SCR-G-02`) |
| Otwarte koszyki | Pozycja usunięta z **widocznym banerem** i propozycją zamiennika (`RULE-014`) |
| Złożone zamówienia | **Bez zmian.** Decyduje kuchnia (`RULE-015`) |
| Cache brzegowy | Unieważnienie menu lokalu |
| POS (v0.2+) | Synchronizacja dostępności |

**Automatyczne przywrócenie** o starcie kolejnego dnia serwisowego (`Availability.auto_restore_at`) —
inaczej lista 86 rośnie z dnia na dzień i przestaje odpowiadać rzeczywistości. To był powód
numer trzy niepowodzenia QR po pandemii: nieaktualizowane menu.

---

## `SCR-D-03` · Stan połączenia

KDS pracuje bez przerwy przez całą zmianę. Utrata połączenia musi być **natychmiast widoczna**,
bo cicho niedziałający KDS oznacza niewykonane zamówienia.

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  KUCHNIA · Bar Zdrój      ● BRAK POŁĄCZENIA          21:04        [86] [⚙]  │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   ⚠  BRAK POŁĄCZENIA OD 1 min 20 s                                           │
│                                                                              │
│   Nowe zamówienia mogą nie docierać.                                         │
│   Powiadom kelnera, żeby przyjmował ustnie.                                  │
│                                                                              │
│   Łączymy ponownie…                                                          │
│                                                                              │
├──────────────────────────────────────────────────────────────────────────────┤
│   Poniżej ostatni znany stan (21:02):                                        │
│   ┌────────────────┐ ┌────────────────┐                                      │
│   │ STOLIK 7       │ │ STOLIK 12      │        wyszarzone                    │
│   └────────────────┘ └────────────────┘                                      │
└──────────────────────────────────────────────────────────────────────────────┘
```

| Zagadnienie | Rozwiązanie |
|---|---|
| Wskaźnik połączenia | Zawsze widoczny w pasku. Zielony `●` / czerwony `●` z licznikiem |
| Heartbeat | Co 30 s. Brak dwóch pod rząd = stan braku połączenia |
| Ponowne łączenie | Wykładniczy backoff, po odzyskaniu **pełne odświeżenie stanu**, nie odtwarzanie zdarzeń |
| Ostatni stan | Zostaje widoczny, wyszarzony, z godziną. Nigdy pusty ekran |
| Zmiana wersji aplikacji | Automatyczne przeładowanie **tylko** przy pustej kolejce |
| Alert do managera | Po 2 min bez połączenia — `EVT` do panelu i push |

---

## Lista kontrolna dla ekranów personelu

**Kelner Pro:**

- [ ] Wszystkie działania osiągalne kciukiem jednej ręki
- [ ] Motyw ciemny, kontrast ≥ 4,5:1 w ciemnym lokalu
- [ ] Cele dotykowe ≥ 48 px, podstawowe działania 56 px
- [ ] Odczyt tablicy stolików działa offline
- [ ] Żaden ekran nie pokazuje kosztów, marż ani cudzych napiwków
- [ ] Copy krótkie i operacyjne, bez uprzejmości
- [ ] Sortowanie według pilności, nie według numerów

**KDS:**

- [ ] Czytelny z 2 metrów — sprawdzone fizycznie, nie na oko w przeglądarce
- [ ] Zero animacji poza pulsem przekroczonego licznika
- [ ] Status kodowany kolorem **i** liczbą **i** obramowaniem
- [ ] Cele ≥ 64 px, obsługa mokrą ręką i klawiaturą
- [ ] Wskaźnik połączenia zawsze widoczny
- [ ] Uwagi dla kuchni wyróżnione wersalikami
- [ ] Bez przewijania poziomego
- [ ] Stabilny przez 14 h bez przeładowania

---

## Powiązane dokumenty

- Ekrany gościa → [`06_Ekrany_Gosc.md`](06_Ekrany_Gosc.md)
- Panel i onboarding → [`08_Ekrany_Panel.md`](08_Ekrany_Panel.md)
- Skala typograficzna KDS → [`05_System_Projektowy.md`](05_System_Projektowy.md) §3.2
