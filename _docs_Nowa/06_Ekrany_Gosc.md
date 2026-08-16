# 06 · Ekrany — powierzchnia gościa

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md), tokeny i ton z [`05`](05_System_Projektowy.md).
> Przepływy: [`02`](02_Aktorzy_Scenariusze.md). Stany encji: [`03`](03_Model_Domenowy.md).

**Zakres:** v0.1 i v0.2 wyczerpująco, v1 pełne ekrany krytyczne.
**Widok odniesienia:** 375 × 812 px. Makiety pokazują układ i treść, nie proporcje.

---

## Spis ekranów

| ID | Ekran | Wydanie |
|---|---|---|
| [`SCR-G-01`](#scr-g-01--wejście-po-zeskanowaniu) | Wejście po zeskanowaniu | v0.1 |
| [`SCR-G-02`](#scr-g-02--menu) | Menu | v0.1 |
| [`SCR-G-03`](#scr-g-03--ekran-pozycji) | Ekran pozycji | v0.1 |
| [`SCR-G-04`](#scr-g-04--koszyk) | Koszyk | v0.1 |
| [`SCR-G-05`](#scr-g-05--status-zamówienia) | Status zamówienia | v0.1 |
| [`SCR-G-06`](#scr-g-06--wezwanie-kelnera) | Wezwanie kelnera | v0.1 |
| [`SCR-G-07`](#scr-g-07--rachunek) | Rachunek | v0.1 |
| [`SCR-G-08`](#scr-g-08--stany-graniczne-sesji) | Stany graniczne sesji | v0.1 |
| [`SCR-G-09`](#scr-g-09--wybór-metody-płatności) | Wybór metody płatności | v0.2 |
| [`SCR-G-10`](#scr-g-10--napiwek) | Napiwek | v0.2 |
| [`SCR-G-11`](#scr-g-11--potwierdzenie-płatności) | Potwierdzenie płatności | v0.2 |
| [`SCR-G-12`](#scr-g-12--wspólny-koszyk-stolika) | Wspólny koszyk stolika | v1 |
| [`SCR-G-13`](#scr-g-13--podział-rachunku--wybór-trybu) | Podział rachunku — wybór trybu | v1 |
| [`SCR-G-14`](#scr-g-14--podział-po-pozycjach) | Podział po pozycjach | v1 |
| [`SCR-G-15`](#scr-g-15--filtr-konwersacyjny) | Filtr konwersacyjny | v1 |
| [`SCR-G-16`](#scr-g-16--zgody-i-ocena-wizyty) | Zgody i ocena wizyty | v1 |

**Obowiązuje na każdym ekranie:** przycisk `Poproszę kelnera` (`F-G-028`) jest zawsze osiągalny.
To realizacja zasady Z2 — technologia woła człowieka, nie zastępuje go.

---

## `SCR-G-01` · Wejście po zeskanowaniu

**Realizuje:** `F-G-001`, `F-G-002`, `F-G-008`, `F-G-005`, `F-G-012`
**Budżet:** first paint ≤ 1,0 s. To jest ekran, który decyduje o całym celu 20 s.

### Stan podstawowy — nowy gość

Nie ma osobnego ekranu powitalnego. Splash to strata 1–2 s z budżetu.
**Skan prowadzi prosto do menu**, a kontekst stolika jest paskiem u góry.

```
┌───────────────────────────────────────────┐
│  Bar Zdrój · Stolik 12          PL ▾  ☾   │  ← pasek kontekstu, 44px
├───────────────────────────────────────────┤
│                                           │
│   Cześć! Zamów bez czekania.              │  ← jedno zdanie, nie ekran
│   Kelner przyjdzie, gdy go poprosisz.     │
│                                           │
├───────────────────────────────────────────┤
│  🔍 Szukaj w menu                         │
├───────────────────────────────────────────┤
│ ┌──────┐┌───────┐┌──────┐┌───────┐┌─────  │  ← kategorie, przewijane
│ │ Piwo ││Przekąs││ Dania││Desery ││ Nap…  │
│ └──────┘└───────┘└──────┘└───────┘└─────  │
├───────────────────────────────────────────┤
│  PIWO                                     │
│  ┌─────────────────────────────────────┐  │
│  │ ▓▓▓▓▓  Żywiec 0,5 l           12,00 zł│  │
│  │ ▓▓▓▓▓  Lager, 5,6%                 [+]│  │
│  └─────────────────────────────────────┘  │
│  ┌─────────────────────────────────────┐  │
│  │ ▓▓▓▓▓  Ciechan Pszeniczne     15,00 zł│  │
│  │ ▓▓▓▓▓  Pszeniczne, 4,9%            [+]│  │
│  └─────────────────────────────────────┘  │
│                            ⋮              │
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │  ← przyklejone, zawsze
└───────────────────────────────────────────┘
```

### Stan — gość powracający (cel 8 s)

⚠️ **Kluczowa różnica:** `Zamów to samo` jest **nad zgięciem**, przed kategoriami. Przy budżecie
8 s nie ma miejsca na przewijanie do przycisku. Wariant do testu: `TUN-001`.

```
┌───────────────────────────────────────────┐
│  Bar Zdrój · Stolik 12          PL ▾  ☾   │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │  Ostatnio u nas brałeś:             │  │
│  │  2× Żywiec 0,5 l · Talerz serów     │  │
│  │                            36,00 zł │  │
│  │  ┌───────────────────────────────┐  │  │
│  │  │      ZAMÓW TO SAMO            │  │  │  ← accent, 56px
│  │  └───────────────────────────────┘  │  │
│  │           albo przeglądaj menu ↓    │  │
│  └─────────────────────────────────────┘  │
├───────────────────────────────────────────┤
│  🔍 Szukaj w menu                         │
│                            ⋮              │
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │
└───────────────────────────────────────────┘
```

### Pozostałe stany

| Stan | Zachowanie | Treść |
|---|---|---|
| **Ładowanie** | Szkielet z realnym układem: pasek kontekstu, 3 znaczniki kategorii, 4 karty. **Nigdy spinner na pustym ekranie** | — |
| **Lokal zamknięty** (`E14`) | Menu w trybie tylko do przeglądania, bez `[+]`, bez paska koszyka | `Zamknięte. Otwieramy jutro o 12:00.` + godziny otwarcia |
| **Nieznany token stolika** | Ekran bez menu | `Nie rozpoznajemy tego kodu. Poproś obsługę o pomoc.` |
| **Brak połączenia** | Menu z cache Service Workera + baner | `Brak połączenia. Menu może być nieaktualne.` |
| **System niedostępny** (`E2`) | Bez możliwości zamówienia, wyeksponowane wezwanie kelnera | `System chwilowo niedostępny. Zamów proszę u kelnera.` |
| **Trwa cudza sesja** (`E8`) | Ekran rozstrzygający — patrz `SCR-G-08` | — |

### Kryteria akceptacji

1. First Contentful Paint ≤ 1,0 s przy 3G z ograniczeniem przepustowości.
2. Pierwszy widok ≤ 200 kB, zero pobranych krojów pisma.
3. Język ustawiany automatycznie z `Accept-Language`, z możliwością zmiany bez przeładowania.
4. `Poproszę kelnera` działa nawet przy niedostępnym backendzie — kolejkuje żądanie.
5. Brak ekranu powitalnego, brak modala zgody na pliki cookie przed menu.

---

## `SCR-G-02` · Menu

**Realizuje:** `F-G-013`, `F-G-029`, `F-G-012`

```
┌───────────────────────────────────────────┐
│  Bar Zdrój · Stolik 12          PL ▾  ☾   │
├───────────────────────────────────────────┤
│  🔍 Szukaj w menu            ⚙ Filtry (2) │
├───────────────────────────────────────────┤
│ ┌──────┐┌───────┐┌──────┐┌───────┐┌─────  │  ← przyklejone przy przewijaniu
│ │ Piwo ││Przekąs││ Dania││Desery ││ Nap…  │
│ └──────┘└───────┘└──────┘└───────┘└─────  │
├───────────────────────────────────────────┤
│  PRZEKĄSKI                                │  ← nagłówek sekcji, przyklejony
│  ┌─────────────────────────────────────┐  │
│  │ ▓▓▓▓▓  Talerz serów           32,00 zł│  │
│  │ ▓▓▓▓▓  Trzy sery, orzechy, miód      │  │
│  │ ▓▓▓▓▓  ⓖ mleko  ⓖ orzechy       [+] │  │  ← alergeny widoczne w liście
│  └─────────────────────────────────────┘  │
│  ┌─────────────────────────────────────┐  │
│  │ ▓▓▓▓▓  Krążki cebulowe        18,00 zł│  │
│  │ ▓▓▓▓▓  Z sosem czosnkowym            │  │
│  │ ▓▓▓▓▓  ⓖ gluten  ⓖ jaja         [+] │  │
│  └─────────────────────────────────────┘  │
│  ┌─────────────────────────────────────┐  │
│  │ ░░░░░  Tatar wołowy           45,00 zł│  │  ← wyszarzone
│  │ ░░░░░  SKOŃCZYŁO SIĘ                 │  │
│  └─────────────────────────────────────┘  │
│                            ⋮              │
├───────────────────────────────────────────┤
│  🛒 3 pozycje · 62,00 zł     ZOBACZ  →   │  ← pasek koszyka
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │
└───────────────────────────────────────────┘
```

### Decyzje projektowe

| Decyzja | Uzasadnienie |
|---|---|
| **Ciągłe przewijanie z przyklejonymi nagłówkami**, a nie zakładki przełączające widok | Gość w barze przegląda, nie nawiguje. Znaczniki kategorii przewijają do sekcji, nie zmieniają ekranu. Alternatywa do testu: `TUN-002` |
| Alergeny **widoczne już w liście**, nie tylko na ekranie pozycji | Obowiązek to „dostępne przed zamówieniem" (`LEG-009`). Ekran pozycji wystarcza formalnie, ale filtr `F-G-009` zyskuje na wcześniejszej widoczności |
| Pozycja niedostępna **zostaje widoczna**, wyszarzona | Znikająca pozycja wygląda jak błąd. Widoczna z etykietą `SKOŃCZYŁO SIĘ` jest uczciwa i zapobiega pytaniom do kelnera |
| `[+]` dodaje z domyślnymi opcjami. Ekran pozycji tylko gdy są wymagane modyfikatory | Skraca ścieżkę o 2 tapnięcia dla piwa. Dla dania z wymaganym wyborem otwiera `SCR-G-03` |

### Stany

| Stan | Treść |
|---|---|
| Ładowanie | Szkielet 4 kart z odpowiednimi wysokościami |
| Brak wyników wyszukiwania | `Nic nie znaleźliśmy dla „xyz". Spróbuj innej nazwy albo poproś kelnera.` |
| Filtry nie dają wyników | `Żadna pozycja nie spełnia tych kryteriów.` + `Wyczyść filtry` |
| Pozycja przeszła w 86 podczas przeglądania | Karta przechodzi w stan wyszarzony z animacją 200 ms. Bez przeskoku układu |
| Brak zdjęć w lokalu | Blok w kolorze kategorii z nazwą. Układ się nie psuje (`05` §9) |

---

## `SCR-G-03` · Ekran pozycji

**Realizuje:** `F-G-029` (obowiązek prawny), modyfikatory
**Otwierany:** dotknięciem karty albo `[+]` przy wymaganych modyfikatorach.
Panel dolny, nie osobna strona — zachowuje pozycję przewijania menu.

```
┌───────────────────────────────────────────┐
│                    ▬▬▬                    │  ← uchwyt panelu
│  ┌─────────────────────────────────────┐  │
│  │                                     │  │
│  │       ▓▓▓ zdjęcie 16:9 ▓▓▓          │  │
│  │                                     │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  Burger Zdrój                    38,00 zł │
│  Wołowina 200 g, ser cheddar, boczek,     │
│  karmelizowana cebula, bułka maślana      │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  ALERGENY                           │  │  ← OBOWIĄZKOWE, przed koszykiem
│  │  Zawiera:                           │  │
│  │   ⓖ gluten   ⓜ mleko   ⓙ jaja      │  │
│  │   ⓢ sezam                           │  │
│  │  Może zawierać:                     │  │
│  │   ⓝ orzechy                         │  │
│  │                                     │  │
│  │  Masz alergię? Powiedz kelnerowi.   │  │
│  │  ▸ Pełna legenda alergenów          │  │  ← rozwijane, TEN SAM ekran
│  └─────────────────────────────────────┘  │
│                                           │
│  STOPIEŃ WYSMAŻENIA          wymagane     │
│  ( ) Krwisty                              │
│  (•) Średnio wysmażony                    │
│  ( ) Dobrze wysmażony                     │
│                                           │
│  DODATKI                    wybierz do 3  │
│  [ ] Dodatkowy boczek           +6,00 zł  │
│  [✓] Jalapeño                   +3,00 zł  │
│  [ ] Podwójny ser               +7,00 zł  │
│                                           │
│  UWAGI DLA KUCHNI                         │
│  ┌─────────────────────────────────────┐  │
│  │ np. bez cebuli                      │  │
│  └─────────────────────────────────────┘  │
│                                           │
│      [ − ]        1        [ + ]          │
├───────────────────────────────────────────┤
│      DODAJ DO KOSZYKA · 41,00 zł          │  ← accent, 56px, kwota żywa
└───────────────────────────────────────────┘
```

### Wymagania prawne tego ekranu

To jedyny ekran w produkcie, którego układ jest podyktowany przepisem.

| Wymóg | Realizacja | Podstawa |
|---|---|---|
| Informacja o 14 alergenach dla żywności nieopakowanej | Blok alergenów, zawsze widoczny | `LEG-009` |
| Dostępna **przed** zamówieniem | Blok nad przyciskiem koszyka, bez rozwijania | jw. |
| Sama informacja ustna nie wystarcza | Forma elektroniczna spełnia obowiązek | jw. |
| Legenda na tym samym ekranie | Rozwijane `▸ Pełna legenda`, bez opuszczania panelu | jw. |
| Jednoznaczne oznaczenia | Piktogram **plus** etykieta tekstowa | `LEG-009` + WCAG 1.4.1 |

⚠️ **Alergeny nie mogą trafić do PDF w stopce ani na osobny ekran.** To najczęstszy błąd
implementacji cyfrowego menu i realne ryzyko przy kontroli sanepidu.

### Stany

| Stan | Treść |
|---|---|
| Ładowanie | Panel otwiera się natychmiast z nazwą i ceną z listy, szczegóły dopełniają się |
| Brak danych o alergenach | **Nie da się osiągnąć** — pozycja bez alergenów nie przechodzi publikacji (`RULE-010`, `I8`) |
| Brak alergenów w składzie | `Ta pozycja nie zawiera alergenów z listy 14.` Jawny komunikat, nie pusty blok |
| Pozycja przeszła w 86 w otwartym panelu | Panel przechodzi w tryb informacyjny. `Ta pozycja właśnie się skończyła.` + `Zobacz podobne` |
| Modyfikator wymagany, niewybrany | Przycisk nieaktywny + `Wybierz stopień wysmażenia` przy grupie |

---

## `SCR-G-04` · Koszyk

**Realizuje:** `F-G-031`, `F-G-021`

```
┌───────────────────────────────────────────┐
│  ←   Twoje zamówienie                     │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ 2× Żywiec 0,5 l              24,00 zł│  │
│  │    🍺 wymaga potwierdzenia obsługi   │  │  ← znacznik alkoholu
│  │                        [ − ] [ + ] 🗑│  │
│  ├─────────────────────────────────────┤  │
│  │ 1× Burger Zdrój              41,00 zł│  │
│  │    Średnio wysmażony, + Jalapeño     │  │
│  │    „bez cebuli"                      │  │
│  │                        [ − ] [ + ] 🗑│  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  Do burgera goście najczęściej biorą:│  │  ← F-G-010, v1
│  │  ▓▓ Frytki belgijskie    14,00 zł [+]│  │
│  └─────────────────────────────────────┘  │
│                                           │
│  KIEDY PODAĆ                              │
│  (•) Wszystko razem                       │
│  ( ) Przekąski teraz, dania za 20 min     │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  Razem                      65,00 zł │  │
│  └─────────────────────────────────────┘  │
├───────────────────────────────────────────┤
│              ZAMAWIAM · 65,00 zł          │  ← accent, 56px
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │
└───────────────────────────────────────────┘
```

### Stany

| Stan | Treść |
|---|---|
| Pusty | `Koszyk jest pusty.` + `Wróć do menu`. Bez ilustracji — strata wagi |
| Pozycja przeszła w 86 (`RULE-014`) | Baner nad listą: `Tatar wołowy właśnie się skończył i zniknął z koszyka.` + `Zobacz podobne`. Pozycja usunięta z animacją. **Nigdy po cichu** |
| Wysyłanie | Przycisk w stanie ładowania, lista zablokowana. Bez pełnoekranowej nakładki |
| Błąd wysyłania | `Nie udało się wysłać zamówienia. Spróbuj ponownie albo poproś kelnera.` Koszyk **zachowany** |
| Offline (`F-G-006`, v1) | `Brak połączenia. Zamówienie wyśle się automatycznie.` Przycisk → `W KOLEJCE` |
| Lokal ma tylko alkohol w koszyku | Bez zmian. Potwierdzenie następuje przy podaniu, nie przy zamawianiu |

---

## `SCR-G-05` · Status zamówienia

**Realizuje:** `F-G-030`, `F-G-032`, `F-G-031`

```
┌───────────────────────────────────────────┐
│  Bar Zdrój · Stolik 12          PL ▾  ☾   │
├───────────────────────────────────────────┤
│                                           │
│         Zamówienie przyjęte                │
│         Gotowe za ok. 12 minut             │  ← konkret, nie „przetwarzamy"
│                                           │
│   ●━━━━━━━━●━━━━━━━━○━━━━━━━━○            │
│  Przyjęte  W kuchni  Gotowe   Podane      │
│                                           │
├───────────────────────────────────────────┤
│  KOLEJKA 1                       20:34    │
│  ┌─────────────────────────────────────┐  │
│  │ 2× Żywiec 0,5 l                     │  │
│  │    ⏳ Czeka na potwierdzenie obsługi │  │  ← F-G-032, uczciwy status
│  ├─────────────────────────────────────┤  │
│  │ 1× Burger Zdrój                     │  │
│  │    🔥 W przygotowaniu                │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │   + DOZAMÓW COŚ JESZCZE             │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │   POPROSZĘ RACHUNEK · 65,00 zł      │  │
│  └─────────────────────────────────────┘  │
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │
└───────────────────────────────────────────┘
```

### Status alkoholu — decyzja projektowa (`P6`)

Koncepcja opisuje obowiązek potwierdzenia wieku przez personel, ale nie mówi, co gość widzi
w międzyczasie. Rozstrzygnięcie:

| Podejście | Ocena |
|---|---|
| Ukryć pozycję do potwierdzenia | ❌ Gość myśli, że zamówienie przepadło. Wezwie kelnera |
| Pokazać jako błąd | ❌ Nieprawda i psuje zaufanie |
| **Pokazać uczciwy status oczekiwania** | ✅ `⏳ Czeka na potwierdzenie obsługi`. Gość rozumie, że ktoś przyjdzie |

Reszta zamówienia idzie do kuchni normalnie — alkohol nie blokuje dania.

### Stany

| Stan | Treść |
|---|---|
| Wiele kolejek | Grupowane po `sequence_no`, najnowsza u góry. Nagłówek `KOLEJKA 2 · 21:05` |
| Pozycja odrzucona przez obsługę | `Nie podaliśmy: 2× Żywiec — obsługa nie potwierdziła wieku. Pozycja zdjęta z rachunku.` |
| Pozycja anulowana przez kuchnię | `Anulowane: Tatar wołowy. Kelner podejdzie wyjaśnić.` |
| Utrata połączenia realtime | Dyskretny pasek `Odświeżam status…`. Bez blokowania ekranu |
| Wszystko podane | Stan przechodzi w `SCR-G-07` z akcentem na rachunek |

---

## `SCR-G-06` · Wezwanie kelnera

**Realizuje:** `F-G-028` · **Obecny na każdym ekranie.** Panel dolny, nie osobna strona.

```
┌───────────────────────────────────────────┐
│                    ▬▬▬                    │
│                                           │
│         Poprosić kelnera do stolika 12?    │
│                                           │
│   ┌─────────────────────────────────────┐ │
│   │            TAK, POPROŚ              │ │
│   └─────────────────────────────────────┘ │
│                  Anuluj                   │
│                                           │
└───────────────────────────────────────────┘

  ── po potwierdzeniu ──

┌───────────────────────────────────────────┐
│                    ▬▬▬                    │
│                    ✓                      │
│         Kelner został powiadomiony.        │
│         Marek podejdzie za chwilę.         │  ← imię, gdy znane
│                                           │
│                  Zamknij                  │
└───────────────────────────────────────────┘
```

### Stany

| Stan | Treść |
|---|---|
| Kelner potwierdził | `Marek już idzie.` Aktualizacja przez realtime, bez odświeżania |
| Brak reakcji > 90 s | `Kelner jeszcze nie odpowiedział. Powiadomiliśmy managera.` Eskalacja automatyczna |
| Ponowne wezwanie < 60 s | Blokada: `Już poprosiliśmy o kelnera. Zaraz podejdzie.` Ochrona przed spamem |
| Offline | Kolejkowane lokalnie. `Wyślemy prośbę, gdy wróci połączenie.` |

---

## `SCR-G-07` · Rachunek

**Realizuje:** `F-G-027` (v0.1), wejście do płatności (v0.2)

### Wariant v0.1 — bez płatności w aplikacji

```
┌───────────────────────────────────────────┐
│  ←   Rachunek · Stolik 12                 │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ 2× Żywiec 0,5 l              24,00 zł│  │
│  │ 1× Burger Zdrój              38,00 zł│  │
│  │    + Jalapeño                 3,00 zł│  │
│  │ 1× Frytki belgijskie         14,00 zł│  │
│  ├─────────────────────────────────────┤  │
│  │ Razem                        79,00 zł│  │
│  │ w tym VAT 8%                  4,15 zł│  │
│  │ w tym VAT 23%                 4,49 zł│  │  ← alkohol osobno
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │         ZAPŁACĘ U KELNERA           │  │
│  └─────────────────────────────────────┘  │
│                                           │
│   Kelner podejdzie z terminalem.          │
│   Możesz zapłacić kartą lub gotówką.      │
│                                           │
└───────────────────────────────────────────┘
```

### Wariant v0.2 — z płatnością

```
│  ┌─────────────────────────────────────┐  │
│  │ Razem                        79,00 zł│  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │      ZAPŁAĆ I WYJDŹ · 79,00 zł      │  │  ← accent, główne
│  └─────────────────────────────────────┘  │
│  ┌─────────────────────────────────────┐  │
│  │         PODZIEL RACHUNEK            │  │  ← drugorzędne, v1
│  └─────────────────────────────────────┘  │
│                                           │
│         Wolisz zapłacić u kelnera?        │  ← nigdy ukryte, LEG-012
│              Poproś o terminal            │
└───────────────────────────────────────────┘
```

⚠️ **`Zapłacę u kelnera` musi być zawsze widoczne i osiągalne.** Art. 59ea UUP zabrania
uzależniania umowy od płatności bezgotówkowej. Ukrycie tej opcji pod „więcej opcji" jest
ryzykiem prawnym, nie decyzją projektową (`LEG-012`).

⚠️ **Kolejność `Zapłać i wyjdź` przed `Podziel rachunek` jest świadoma** i ma wpływ na marżę —
patrz `TUN-007`. To nie jest przypadkowy układ.

### Stany

| Stan | Treść |
|---|---|
| Dozamówienie w trakcie | `Rachunek zaktualizowany: +14,00 zł` Baner, przeliczenie bez utraty kontekstu |
| Rachunek już opłacony | `Rachunek opłacony 20:58. Dziękujemy!` + dostęp do paragonu |
| Częściowo opłacony (podział) | `Zapłacono 59,25 zł z 79,00 zł. Brakuje 19,75 zł.` + kto nie zapłacił |
| Brak płatności 25 min (`E3`) | Bez zmian dla gościa. Alert idzie do kelnera, nie straszymy gościa |

---

## `SCR-G-08` · Stany graniczne sesji

Ekrany, które w większości wdrożeń są pomijane — i tam właśnie wdrożenia się psują.

### Trwa cudza sesja przy tym stoliku (`E8`)

```
┌───────────────────────────────────────────┐
│  Bar Zdrój · Stolik 12                    │
├───────────────────────────────────────────┤
│                                           │
│   Przy tym stoliku jest otwarty rachunek  │
│   na 79,00 zł.                            │
│                                           │
│   ┌─────────────────────────────────────┐ │
│   │   DOŁĄCZAM DO TEGO STOLIKA          │ │  ← domyślne
│   └─────────────────────────────────────┘ │
│   ┌─────────────────────────────────────┐ │
│   │   ZACZYNAM NOWY RACHUNEK            │ │
│   └─────────────────────────────────────┘ │
│                                           │
│   Nowy rachunek wymaga potwierdzenia      │
│   przez obsługę.                          │
│                                           │
├───────────────────────────────────────────┤
│           🔔  Poproszę kelnera            │
└───────────────────────────────────────────┘
```

⚠️ Wybór `ZACZYNAM NOWY RACHUNEK` **wymaga potwierdzenia kelnera** (`F-K-009`). Bez tej bramki
przypadkowa osoba mogłaby zamknąć cudzą sesję albo obejrzeć cudzy rachunek.

### System niedostępny (`E2`)

```
┌───────────────────────────────────────────┐
│                                           │
│   System chwilowo niedostępny.            │
│                                           │
│   Zamów proszę u kelnera —                │
│   karta papierowa jest na stoliku.        │
│                                           │
│   ┌─────────────────────────────────────┐ │
│   │      🔔  POPROSZĘ KELNERA           │ │  ← jedyna akcja
│   └─────────────────────────────────────┘ │
│                                           │
│         Spróbuj ponownie za chwilę        │
└───────────────────────────────────────────┘
```

**Uczciwość ponad wrażenie.** Kolejka offline na telefonie gościa nic nie da, jeśli KDS też jest
bez sieci (`P9`). Lepiej powiedzieć prawdę i wskazać działającą drogę.

### Lokal zamknięty (`E14`)

```
│   Zamknięte.                              │
│   Otwieramy jutro o 12:00.                │
│                                           │
│   pon–czw   12:00 – 23:00                 │
│   pt–sob    12:00 – 01:00                 │
│   niedz     12:00 – 22:00                 │
│                                           │
│   Menu możesz przejrzeć poniżej ↓         │
```

---

## `SCR-G-09` · Wybór metody płatności

**Realizuje:** `F-G-004`, `F-G-023`, `F-G-026` · **Wydanie:** v0.2

```
┌───────────────────────────────────────────┐
│  ←   Płatność · 79,00 zł                  │
├───────────────────────────────────────────┤
│  JAK CHCESZ ZAPŁACIĆ                      │
│  ┌─────────────────────────────────────┐  │
│  │ (•) BLIK                            │  │  ← pierwszy, standard PL
│  │     Kod z aplikacji banku            │  │
│  ├─────────────────────────────────────┤  │
│  │ ( )  Apple Pay                       │  │  ← tylko gdy dostępny
│  ├─────────────────────────────────────┤  │
│  │ ( ) 💳 Karta                         │  │
│  ├─────────────────────────────────────┤  │
│  │ ( ) 🧾 Zapłacę u kelnera             │  │  ← zawsze, LEG-012
│  └─────────────────────────────────────┘  │
│                                           │
│  [ ] Chcę fakturę na firmę                │
│      ┌───────────────────────────────┐    │
│      │ NIP                           │    │
│      └───────────────────────────────┘    │
│      Dane pobierzemy automatycznie.       │
│                                           │
├───────────────────────────────────────────┤
│              DALEJ · NAPIWEK              │
└───────────────────────────────────────────┘
```

### Stany

| Stan | Treść |
|---|---|
| BLIK odrzucony | `Kod nie został zaakceptowany. Spróbuj ponownie lub wybierz inną metodę.` Rachunek nietknięty |
| Przekroczony czas BLIK | `Kod wygasł. Wygeneruj nowy w aplikacji banku.` |
| NIP nieznaleziony w GUS | `Nie znaleźliśmy firmy o tym numerze NIP. Sprawdź numer albo wpisz dane ręcznie.` |
| PSP niedostępny | `Płatność online chwilowo niedostępna. Zapłać proszę u kelnera.` — automatyczne przejście na `F-G-027` |
| Apple/Google Pay niedostępne | Opcja **nie jest pokazywana**, nie wyszarzana |

---

## `SCR-G-10` · Napiwek

**Realizuje:** `F-G-024`, `F-K-001` · **Wydanie:** v0.2
**Najbardziej wrażliwy prawnie ekran w produkcie** (`LEG-006`).

```
┌───────────────────────────────────────────┐
│  ←   Napiwek                              │
├───────────────────────────────────────────┤
│                                           │
│              ┌─────────┐                  │
│              │  ▓▓▓▓▓  │                  │  ← zdjęcie kelnera
│              │  ▓▓▓▓▓  │                  │
│              └─────────┘                  │
│                 Marek                     │
│           obsługiwał Twój stolik          │
│                                           │
│  ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐ │
│  │  Bez  │ │  5%   │ │  10%  │ │ Inna  │ │
│  │napiwku│ │3,95 zł│ │7,90 zł│ │ kwota │ │
│  └───────┘ └───────┘ └───────┘ └───────┘ │
│                                           │
│   Napiwek trafia bezpośrednio do Marka.   │  ← wymóg prawny
│   Jest w pełni dobrowolny.                │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ Rachunek                     79,00 zł│  │
│  │ Napiwek                       7,90 zł│  │
│  │ ──────────────────────────────────── │  │
│  │ Razem                        86,90 zł│  │
│  └─────────────────────────────────────┘  │
│                                           │
├───────────────────────────────────────────┤
│           ZAPŁAĆ · 86,90 zł               │
└───────────────────────────────────────────┘
```

### Wymagania prawne tego ekranu

| Wymóg | Realizacja | Podstawa |
|---|---|---|
| Napiwek zawsze opcjonalny | `Bez napiwku` jest **pełnoprawnym pierwszym kafelkiem**, nie ukrytym „pomiń" | `LEG-005` |
| Nigdy zaznaczony domyślnie | Żaden kafelek nie jest wstępnie wybrany. Przycisk aktywny dopiero po wyborze | jw. |
| Środki nie przechodzą przez lokal ani przez nas | Split w PSP prosto na konto kelnera | `LEG-006`, `I10` |
| Jasny komunikat dla gościa | `Napiwek trafia bezpośrednio do Marka.` — zdanie ma znaczenie prawne, nie marketingowe | `LEG-006` |
| Brak puli wspólnej | Odbiorcą jest konkretny kelner (`RULE-020`) | `LEG-006` |

### Presety 5% i 10% — decyzja przeciw domyślnym wzorcom

Badanie MFR 2025: 89% polskich gości zostawia napiwki, typowy rozmiar **5–10%**.
Amerykańskie presety 15/20/25 są w Polsce odbierane jako nachalne i **obniżają** odsetek
napiwków. Presety pozostają parametrem do przetestowania: `TUN-005`.

### Stany

| Stan | Treść |
|---|---|
| Kelner bez zweryfikowanego konta wypłat | **Ekran napiwku nie jest pokazywany w ogóle.** Przejście prosto do płatności. Nigdy „zbierzemy na później" — to byłby pooling (`LEG-006`) |
| Kelner nieprzypisany do sekcji | Ekran pominięty. Napiwek bez adresata nie istnieje |
| Własna kwota | Pole numeryczne, maksimum 100% rachunku, walidacja groszy |
| Podział rachunku | Każdy uczestnik decyduje o napiwku osobno, od **swojej** części |

---

## `SCR-G-11` · Potwierdzenie płatności

**Realizuje:** `F-G-023`, `F-G-025` · **Wydanie:** v0.2 (e-Paragon: v1)

```
┌───────────────────────────────────────────┐
│                                           │
│                    ✓                      │
│            Zapłacone                      │
│                                           │
│         86,90 zł · BLIK · 20:58           │
│                                           │
│      Możesz wychodzić. Dziękujemy!        │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  🧾 Paragon                          │  │
│  │  Kelner przyniesie wydruk            │  │  ← v0.2
│  │  ─── albo w v1: ───                  │  │
│  │  Paragon jest w aplikacji e-Paragony  │  │
│  │  Kod: 7K4M-2XQ9                      │  │
│  │  ▸ Jak odebrać paragon                │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  Jak było u nas?                     │  │  ← v1
│  │   ☆   ☆   ☆   ☆   ☆                 │  │
│  └─────────────────────────────────────┘  │
│                                           │
└───────────────────────────────────────────┘
```

**e-Paragon bez adresu e-mail** (`F-G-025`) — gość dostaje anonimowy identyfikator KID
z HUB Paragonowy. Zgodne z zasadą Z4: nie zbieramy danych, których nie potrzebujemy.

### Stany

| Stan | Treść |
|---|---|
| Fiskalizacja opóźniona (`E4`) | **Gość nie widzi problemu.** `Paragon za chwilę.` Alert idzie do kelnera i managera |
| Płatność częściowa przy podziale | `Twoja część opłacona. Czekamy jeszcze na 2 osoby.` + kwota pozostała |
| Zwrot | `Zwrócono 86,90 zł. Środki wrócą na konto w ciągu 1–3 dni roboczych.` |

---

## `SCR-G-12` · Wspólny koszyk stolika

**Realizuje:** `F-G-016`, `F-G-018`, `F-G-020` · **Wydanie:** v1

```
┌───────────────────────────────────────────┐
│  ←   Stolik 12 · 3 osoby                  │
├───────────────────────────────────────────┤
│  ┌───┐ ┌───┐ ┌───┐  ┌─────────────────┐  │
│  │ K │ │ M │ │ O │  │ + zaproś         │  │  ← uczestnicy
│  └───┘ └───┘ └───┘  └─────────────────┘  │
│  Kasia Marek  Ola                         │
├───────────────────────────────────────────┤
│  WSPÓLNY KOSZYK                           │
│  ┌─────────────────────────────────────┐  │
│  │ 2× Żywiec 0,5 l              24,00 zł│  │
│  │    dodał Marek                       │  │
│  ├─────────────────────────────────────┤  │
│  │ 1× Sałatka grecka            28,00 zł│  │
│  │    dodała Ola                        │  │
│  ├─────────────────────────────────────┤  │
│  │ 1× Burger Zdrój              41,00 zł│  │
│  │    dodałaś Ty          [ − ] [ + ] 🗑│  │  ← edycja tylko swoich
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │  🍻 TA KOLEJKA NA MNIE               │  │
│  │  Pozycje innych dopiszą się do       │  │
│  │  Twojego rachunku                    │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ Razem                        93,00 zł│  │
│  └─────────────────────────────────────┘  │
├───────────────────────────────────────────┤
│         ZAMAWIAM ZA WSZYSTKICH            │
└───────────────────────────────────────────┘
```

### Reguły

| Reguła | Zachowanie |
|---|---|
| Widoczność | Uczestnik widzi wszystkie pozycje i całość rachunku, ale **nie** metody płatności innych (`RULE-016`) |
| Edycja | Uczestnik edytuje wyłącznie własne pozycje. Cudze są tylko do odczytu |
| Zamawianie | Zamówić może każdy uczestnik. Zamawia **całość koszyka**, nie tylko swoje |
| Aktualizacja | Realtime na kanale `session.{id}`. Dodanie pozycji przez innego = dyskretny toast, nie modal |
| Dołączenie | Skan tego samego kodu QR. Bez zaproszeń i linków |

### Stany

| Stan | Treść |
|---|---|
| Sam przy stoliku | Pasek uczestników ukryty. Ekran wygląda jak `SCR-G-04` |
| Ktoś dołączył | Toast `Marek dołączył do stolika` · 3 s |
| Ktoś dodał pozycję | Toast `Marek dodał 2× Żywiec` · lista aktualizuje się bez przeskoku |
| Ktoś zamówił pierwszy | `Ola zamówiła za wszystkich.` Koszyk pusty, przejście do `SCR-G-05` |
| Konflikt edycji | Wygrywa ostatni zapis. Bez blokad — koszyk to nie dokument |

---

## `SCR-G-13` · Podział rachunku — wybór trybu

**Realizuje:** `F-G-017` · **Wydanie:** v1

```
┌───────────────────────────────────────────┐
│  ←   Podziel rachunek · 93,00 zł          │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ (•) Po równo                         │  │
│  │     3 × 31,00 zł                     │  │
│  ├─────────────────────────────────────┤  │
│  │ ( ) Po pozycjach                     │  │
│  │     Każdy płaci za to, co jadł       │  │
│  ├─────────────────────────────────────┤  │
│  │ ( ) Wpisuję kwoty ręcznie            │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  KTO PŁACI                                │
│  [✓] Kasia (Ty)              31,00 zł     │
│  [✓] Marek                   31,00 zł     │
│  [✓] Ola                     31,00 zł     │
│                                           │
│   Każdy dostanie własny link.             │
│   Kto wyjdzie wcześniej — płaci i idzie.  │
│                                           │
├───────────────────────────────────────────┤
│            PODZIEL I ZAPŁAĆ               │
└───────────────────────────────────────────┘
```

### Reszta z dzielenia

93,00 zł na 3 osoby dzieli się równo. Ale 100,00 zł na 3 daje 33,33 + 33,33 + **33,34**.
**Reszta trafia deterministycznie do inicjatora podziału** (`RULE-002`). Interfejs pokazuje
to jawnie: `Twoja część: 33,34 zł (o grosz więcej z zaokrąglenia)`.

Suma udziałów musi równać się sumie rachunku co do grosza (`RULE-017`, `I2`).

### Stany

| Stan | Treść |
|---|---|
| Jedna osoba przy stoliku | Ekran niedostępny. `Podziel rachunek` się nie pokazuje |
| Ktoś już zapłacił swoją część | Jego wiersz oznaczony `✓ Zapłacone`, kwota niemodyfikowalna |
| Zmiana trybu po utworzeniu podziału | Możliwa **tylko** dopóki nikt nie zapłacił. Potem zablokowana (`RULE-018`) |
| Dozamówienie w trakcie podziału | Nowe pozycje → **nowy** rachunek (`E15`). Trwający podział nietknięty |
| Brakuje jednej wpłaty po 15 min (`E6`) | `Czekamy na Marka — 31,00 zł.` + `Powiadom Marka` + `Poproszę kelnera` |

---

## `SCR-G-14` · Podział po pozycjach

**Realizuje:** `F-G-017` tryb `by_items` · **Wydanie:** v1

```
┌───────────────────────────────────────────┐
│  ←   Kto co brał?                         │
├───────────────────────────────────────────┤
│  Dotknij pozycji, które są Twoje.         │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ [✓] 1× Burger Zdrój          41,00 zł│  │  ← moje, akcent
│  │     Ty                               │  │
│  ├─────────────────────────────────────┤  │
│  │ [ ] 2× Żywiec 0,5 l          24,00 zł│  │
│  │     Marek ✓                          │  │  ← już przypisane
│  ├─────────────────────────────────────┤  │
│  │ [ ] 1× Sałatka grecka        28,00 zł│  │
│  │     nikt jeszcze                     │  │  ← wymaga uwagi
│  └─────────────────────────────────────┘  │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ Twoja część                  41,00 zł│  │
│  │ Nieprzypisane                28,00 zł│  │  ← ostrzeżenie
│  └─────────────────────────────────────┘  │
│                                           │
│   ⚠ Sałatka grecka nie ma właściciela.    │
│      Podzielimy ją po równo?              │
│      [ Tak, po równo ]  [ To moje ]       │
│                                           │
├───────────────────────────────────────────┤
│           ZAPŁAĆ SWOJĄ CZĘŚĆ              │
└───────────────────────────────────────────┘
```

**Pozycja bez właściciela** to najczęstszy problem tego trybu. Nie wolno go zignorować —
suma udziałów musi się zgadzać co do grosza (`I2`). Rozwiązanie: jawne pytanie, a nie ciche
doliczenie komukolwiek.

---

## `SCR-G-15` · Filtr konwersacyjny

**Realizuje:** `F-G-009`, `F-G-011` · **Wydanie:** v1

```
┌───────────────────────────────────────────┐
│  ←   Czego szukasz?                       │
├───────────────────────────────────────────┤
│  ┌─────────────────────────────────────┐  │
│  │ bez glutenu, nie za ostre,          │  │
│  │ coś lekkiego do 60 zł           🎤  │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  Szybkie filtry                           │
│  ┌────────────┐ ┌───────────┐ ┌────────┐ │
│  │ bez glutenu│ │wegetariańs│ │ ostre  │ │
│  └────────────┘ └───────────┘ └────────┘ │
│  ┌────────────┐ ┌───────────┐            │
│  │ bez laktozy│ │  do 40 zł │            │
│  └────────────┘ └───────────┘            │
├───────────────────────────────────────────┤
│  ZNALEŹLIŚMY 6 POZYCJI                    │
│  ┌─────────────────────────────────────┐  │
│  │ ▓▓▓ Sałatka grecka           28,00 zł│  │
│  │     Bez glutenu · łagodna        [+] │  │
│  ├─────────────────────────────────────┤  │
│  │ ▓▓▓ Krewetki na maśle        42,00 zł│  │
│  │     Bez glutenu · łagodna        [+] │  │
│  └─────────────────────────────────────┘  │
│                                           │
│   ⓘ Informacje o alergenach podaje lokal. │
│     Przy alergii potwierdź u kelnera.     │  ← zastrzeżenie odpowiedzialności
└───────────────────────────────────────────┘
```

⚠️ **Zastrzeżenie jest obowiązkowe.** Za poprawność danych o alergenach odpowiada lokal
(art. 8 FIC), za ich wyświetlenie — my. Filtr AI nie może sprawiać wrażenia medycznej gwarancji
(`LEG-009`).

**Zamiana obowiązku w funkcję:** ten ekran istnieje, bo alergeny i tak musimy mieć w strukturze
danych. Konkurencja traktuje je jako obowiązek do odhaczenia. My budujemy na nich najbardziej
odróżniającą funkcję menu.

---

## `SCR-G-16` · Zgody i ocena wizyty

**Realizuje:** `F-G-033`, `F-P-001`, `F-P-002` · **Wydanie:** v1

### Zgody — jedyne legalne okno

```
┌───────────────────────────────────────────┐
│  Chcesz dostawać oferty od Bar Zdrój?     │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │ Telefon                             │  │
│  │ +48 ___ ___ ___                     │  │
│  └─────────────────────────────────────┘  │
│                                           │
│  [ ] Zgadzam się na SMS-y od Bar Zdrój    │  ← osobno per kanał
│  [ ] Zgadzam się na e-maile od Bar Zdrój  │  ← nigdy zaznaczone
│                                           │
│  Administratorem danych jest              │
│  Bar Zdrój Sp. z o.o., ul. Rynek 1,       │  ← administrator wskazany
│  31-042 Kraków. Zgodę możesz wycofać      │
│  w każdej chwili.                         │
│  ▸ Pełna informacja o przetwarzaniu       │
│                                           │
│  ┌─────────────────────────────────────┐  │
│  │            ZAPISZ                    │  │
│  └─────────────────────────────────────┘  │
│              Nie, dziękuję                │  ← równorzędne wyjście
└───────────────────────────────────────────┘
```

**Dlaczego dokładnie tutaj:** art. 398 PKE (obowiązuje od 10.11.2024) wymaga uprzedniej zgody
na komunikację marketingową i **zabrania nawiązywania kontaktu po to, żeby dopiero o zgodę
poprosić**. Moment składania zamówienia jest więc **jedynym legalnym oknem** (`LEG-007`).

| Wymóg | Realizacja |
|---|---|
| Osobne zgody per kanał | Dwa niezależne pola wyboru: SMS, e-mail |
| Bez zaznaczenia wstępnego | Oba puste. `ZAPISZ` nieaktywne bez zaznaczenia choć jednej |
| Jednoznaczny administrator | Pełna nazwa i adres **lokalu** — nie nasza. Jesteśmy procesorem (`LEG-008`) |
| Wersjonowanie treści | `Consent.text_version` zapisywany razem ze zgodą (`RULE-023`) |
| Łatwe wycofanie | Link w każdej wiadomości i w profilu gościa |

### Ocena wizyty i przechwytywanie opinii

```
   Jak było u nas?                            ocena 4–5:
   ☆ ☆ ☆ ☆ ☆                         ┌─────────────────────────┐
                                     │ Super! Podzielisz się    │
   ── po dotknięciu ──               │ opinią w Google?         │
                                     │  [ OCEŃ W GOOGLE ]       │
   ocena 1–3:                        └─────────────────────────┘
   ┌─────────────────────────┐
   │ Przykro nam. Co poszło  │       Zadowoleni → Google Maps.
   │ nie tak?                │       Niezadowoleni → prywatnie
   │ ┌─────────────────────┐ │       do managera + alert.
   │ │                     │ │
   │ └─────────────────────┘ │       Ocena Google = ruch = przychód.
   │  [ WYŚLIJ DO MANAGERA ] │       Ta funkcja sprzedaje się
   └─────────────────────────┘       na pierwszym demo.
```

---

## Lista kontrolna dla każdego ekranu gościa

Przed uznaniem ekranu za gotowy:

- [ ] Stany: ładowanie (szkielet, nie spinner), pusty, błąd z **działaniem**, offline
- [ ] `Poproszę kelnera` osiągalne
- [ ] Cele dotykowe ≥ 48 px
- [ ] Kontrast ≥ 4,5:1 w motywie jasnym **i** ciemnym
- [ ] Pełna obsługa klawiaturą, widoczny fokus
- [ ] Copy po polsku, na „ty", bez „Ups!" i bez wykrzykników
- [ ] Kwoty jako `123,45 zł` z cyframi tabelarycznymi
- [ ] Bez przeskoków układu przy doładowywaniu treści
- [ ] Działa w motywie ciemnym bez poprawek
- [ ] Tłumaczenia PL / UK / EN / DE kompletne
- [ ] Mieści się w budżecie wagi ([`05`](05_System_Projektowy.md) §7.3)

---

## Powiązane dokumenty

- Ekrany personelu → [`07_Ekrany_Kelner_KDS.md`](07_Ekrany_Kelner_KDS.md)
- Tokeny, ton, budżet → [`05_System_Projektowy.md`](05_System_Projektowy.md)
- Kandydaci do tuningu tych ekranów → [`10_Tuning_Decyzje_Ryzyka.md`](10_Tuning_Decyzje_Ryzyka.md) §2
