# 05 · System projektowy

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md).
> Makiety stosujące ten system: [`06`](06_Ekrany_Gosc.md), [`07`](07_Ekrany_Kelner_KDS.md), [`08`](08_Ekrany_Panel.md), [`09`](09_Ekrany_v2_v3.md).

---

## 1. Cztery konteksty, jeden zestaw tokenów

Jeden responsywny layout nie obsłuży czterech sytuacji. Ale cztery niezależne systemy
projektowe to koszt utrzymania, którego nie udźwigniemy. Rozwiązanie: **wspólne tokeny,
cztery różne sposoby ich użycia**.

| Powierzchnia | Warunki | Konsekwencja projektowa |
|---|---|---|
| **Gość** | Telefon w ręku, słabe 3G, ciemny lokal, hałas, jedna ręka, mało baterii | Ciepła paleta, zdjęcia jedzenia, duże cele dotykowe, tryb ciemny domyślny wieczorem, minimum JavaScriptu |
| **Kelner** | Jedna ręka, w ruchu, ciemno, ekran w kieszeni fartucha, dziesiątki spojrzeń na zmianę | Gęsta informacja, wysoki kontrast, wszystko w zasięgu kciuka, tryb ciemny domyślny **zawsze** |
| **Kuchnia** | Czytanie **z 2 metrów**, mokre ręce, para, brak czasu, ekran włączony 14 h | Ogromna typografia, status kodowany kolorem **i** liczbą, zero dekoracji, bez animacji |
| **Panel** | Biurko, laptop, analiza po zmianie, długie sesje | Gęste tabele, wykresy, standardowa typografia, tryb jasny domyślny |

**Wspólne dla wszystkich:** tokeny kolorów, skala odstępów, promienie, ikonografia, formaty
polskie, reguły dostępności.
**Różne:** skala typograficzna, gęstość, domyślny motyw, wagi kolorów.

---

## 2. Kolor

### 2.1. Zasada porządkująca

Kolory statusu — zielony, żółty, czerwony — są **zarezerwowane**. Nigdy nie służą jako kolor
marki. Dzięki temu zielone pole na KDS zawsze znaczy „w czasie", a nie „przycisk marki".

Marka ma dwa kolory:

| Rola | Kolor | Gdzie |
|---|---|---|
| **Primary — petrol** | Chrom aplikacji, elementy sterujące, powierzchnie personelu | Kelner Pro, KDS, panel, nawigacja |
| **Accent — terakota** | Główne wezwania do działania na powierzchni gościa | „Zamawiam", „Zapłać", wyróżnienia w menu |

Powierzchnia gościa opiera się na **akcencie i fotografii** — bo sprzedaje jedzenie.
Powierzchnie personelu opierają się na **primary i statusach** — bo sprzedają czytelność.
KDS niemal nie używa kolorów marki: pokazuje wyłącznie statusy.

### 2.2. Tokeny — motyw jasny

```css
:root {
  /* Marka */
  --color-primary-50:   #ECFAFB;
  --color-primary-100:  #CFF0F4;
  --color-primary-500:  #1B7A91;
  --color-primary-600:  #175E75;   /* podstawowy */
  --color-primary-700:  #124B5E;
  --color-primary-900:  #0B2E3A;

  --color-accent-50:    #FEF3EC;
  --color-accent-100:   #FCE0CE;
  --color-accent-500:   #E05A22;
  --color-accent-600:   #C2410C;   /* podstawowy CTA gościa */
  --color-accent-700:   #9A3412;

  /* Neutralne — ciepłe, nie zimne */
  --color-surface:      #FFFFFF;
  --color-surface-sunken: #FAF8F6;
  --color-surface-raised: #FFFFFF;
  --color-border:       #E5E0DB;
  --color-border-strong:#C9C2BA;
  --color-text:         #1C1917;
  --color-text-muted:   #57534E;
  --color-text-subtle:  #78716C;
  --color-text-inverse: #FFFFFF;

  /* Status — zarezerwowane */
  --color-success:      #15803D;
  --color-success-bg:   #F0FDF4;
  --color-warning:      #CA8A04;
  --color-warning-bg:   #FEFCE8;
  --color-danger:       #B91C1C;
  --color-danger-bg:    #FEF2F2;
  --color-info:         #1D4ED8;
  --color-info-bg:      #EFF6FF;
}
```

### 2.3. Tokeny — motyw ciemny

Tryb ciemny **nie jest opcją**. Jest domyślny w Kelner Pro i domyślny wieczorem w PWA gościa
(bar o 21:00, jasny ekran w twarz — to realny problem, nie preferencja).

```css
[data-theme="dark"] {
  --color-primary-500:  #3BA9C4;
  --color-primary-600:  #4FBBD4;   /* jaśniejszy — ciemne tło wymaga odwrócenia */
  --color-accent-600:   #F97316;   /* rozjaśniona terakota, kontrast na ciemnym */

  --color-surface:      #161514;
  --color-surface-sunken: #0D0C0C;
  --color-surface-raised: #232120;
  --color-border:       #35322F;
  --color-border-strong:#4A4642;
  --color-text:         #F5F3F1;
  --color-text-muted:   #B3ADA7;
  --color-text-subtle:  #8A847E;
  --color-text-inverse: #161514;

  --color-success:      #4ADE80;
  --color-success-bg:   #16251A;
  --color-warning:      #FACC15;
  --color-warning-bg:   #262009;
  --color-danger:       #F87171;
  --color-danger-bg:    #2A1414;
  --color-info:         #60A5FA;
  --color-info-bg:      #12203A;
}
```

**Reguły trybu:**

| Powierzchnia | Domyślnie | Przełącznik |
|---|---|---|
| Gość | Systemowy, z wymuszeniem ciemnego po zmroku wg godzin serwisowych lokalu | Tak, w stopce |
| Kelner Pro | **Zawsze ciemny** | Nie |
| KDS | **Zawsze ciemny**, maksymalny kontrast | Nie |
| Panel | Systemowy, domyślnie jasny | Tak |

### 2.4. Statusy KDS — kolor to za mało

Czas oczekiwania na KDS kodowany jest **trzema jednoczesnymi środkami**: kolorem, licznikiem
liczbowym i grubością obramowania.

| Stan | Kolor | Licznik | Obramowanie |
|---|---|---|---|
| W normie (< 80% czasu normatywnego) | `--color-success` | `04:12` | 2 px |
| Uwaga (80–100%) | `--color-warning` | `07:48` | 4 px |
| Przekroczony (> 100%) | `--color-danger` | `11:03` **pulsujący** | 6 px |

⚠️ **Wymóg WCAG 1.4.1:** informacja nie może być przekazywana samym kolorem. Tu nie chodzi
o formalność — w kuchni pracują ludzie z zaburzeniami rozróżniania barw, a licznik jest
i tak czytelniejszy z 2 metrów.

---

## 3. Typografia

### 3.1. Krój — decyzja wydajnościowa

**Systemowy stos czcionek na wszystkich powierzchniach.** Zero pobierania fontów.

```css
--font-sans: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
             "Helvetica Neue", Arial, "Noto Sans", sans-serif;
--font-mono: ui-monospace, "SF Mono", "Cascadia Mono", Menlo, monospace;
```

**Uzasadnienie:** pobranie kroju to 30–120 kB i opóźnienie pierwszego renderu tekstu. Budżet
gościa to **< 1 s first paint na 3G** (§7). Font marki kosztowałby jedną trzecią tego budżetu
w zamian za wrażenie estetyczne, którego gość i tak nie zauważy przez 4 sekundy, na jakie
patrzy na ekran. Krój firmowy zostaje na stronie marketingowej — poza zakresem tego produktu.

Systemowe kroje mają też pełne wsparcie polskich znaków diakrytycznych i cyrylicy dla wersji
ukraińskiej — bez dodatkowych podzbiorów.

`--font-mono` wyłącznie dla kwot w tabelach panelu i numerów zamówień na KDS — wyrównanie cyfr.

### 3.2. Skale — różne per powierzchnia

**Gość i panel** (baza 16 px):

| Token | Rozmiar | Interlinia | Zastosowanie |
|---|---|---|---|
| `--text-xs` | 12 px | 16 px | Metadane, legenda alergenów |
| `--text-sm` | 14 px | 20 px | Opisy, tekst pomocniczy |
| `--text-base` | 16 px | 24 px | Tekst podstawowy. **Minimum dla treści** |
| `--text-lg` | 18 px | 26 px | Nazwy pozycji menu |
| `--text-xl` | 22 px | 28 px | Nagłówki sekcji |
| `--text-2xl` | 28 px | 34 px | Kwota rachunku |
| `--text-3xl` | 36 px | 42 px | Nagłówek ekranu |

**Kelner Pro** — ta sama skala, ale gęściej: `--text-sm` jako podstawa dla list stolików,
`--text-base` dla wszystkiego, co wymaga decyzji.

**KDS** — osobna skala, czytelność z 2 metrów:

| Token | Rozmiar | Zastosowanie |
|---|---|---|
| `--kds-text-sm` | 20 px | Modyfikatory, notatki |
| `--kds-text-base` | 26 px | Nazwy pozycji |
| `--kds-text-lg` | 34 px | Numer stolika |
| `--kds-text-xl` | 48 px | Licznik czasu |

⚠️ KDS to **nie jest responsywna wersja panelu**. To osobna skala typograficzna wynikająca
z odległości obserwacji.

### 3.3. Reguły tekstu

- Maksymalna szerokość bloku tekstu: **68 znaków**.
- Bez tekstu wyjustowanego — polskie łamanie z justowaniem daje rzeki.
- Nazwy pozycji menu: maksimum 2 wiersze, potem wielokropek. Pełna nazwa na ekranie pozycji.
- Kwoty **zawsze** tabelaryczne: `font-variant-numeric: tabular-nums`.
- Bez wersalików w dłuższych ciągach — polskie diakrytyki w wersalikach czyta się gorzej.

---

## 4. Odstępy, promienie, cienie

```css
/* Skala 4 px */
--space-1: 4px;    --space-2: 8px;    --space-3: 12px;
--space-4: 16px;   --space-5: 20px;   --space-6: 24px;
--space-8: 32px;   --space-10: 40px;  --space-12: 48px;
--space-16: 64px;

--radius-sm: 6px;    /* pola formularzy, znaczniki */
--radius-md: 10px;   /* przyciski, karty */
--radius-lg: 16px;   /* karty pozycji menu, panele dolne */
--radius-full: 9999px;

--shadow-sm: 0 1px 2px rgb(0 0 0 / 0.06);
--shadow-md: 0 4px 12px rgb(0 0 0 / 0.08);
--shadow-lg: 0 12px 32px rgb(0 0 0 / 0.12);

--touch-min: 48px;   /* minimalny cel dotykowy — nienegocjowalny */
```

**Cele dotykowe:** minimum **48 × 48 px** ze wszystkimi odstępami. Kontekst to hałas, ruch
i jedna ręka. WCAG 2.1 AA wymaga 44 px — bierzemy zapas.

Cienie tylko na powierzchniach uniesionych (panele dolne, modale). Karty używają obramowania,
nie cienia — na ciemnym motywie cień jest niewidoczny, a obramowanie działa w obu motywach.

---

## 5. Ruch

```css
--motion-fast:   120ms;   /* reakcja na dotknięcie */
--motion-base:   200ms;   /* przejścia stanów */
--motion-slow:   320ms;   /* panele dolne, modale */
--ease-out:      cubic-bezier(0.16, 1, 0.3, 1);
```

**Reguły:**

1. Ruch **wyłącznie funkcjonalny**: informuje o zmianie stanu albo o kierunku nawigacji.
2. `prefers-reduced-motion: reduce` → wszystkie przejścia do 0 ms. Nie negocjujemy (WCAG 2.3.3).
3. **KDS nie animuje niczego**, poza pulsowaniem licznika po przekroczeniu czasu. Animacja
   w polu widzenia przez 14 godzin jest męcząca.
4. Żaden ruch nie opóźnia interakcji. Reakcja na dotknięcie jest natychmiastowa, animacja
   dogania.

---

## 6. Dostępność — WCAG 2.1 AA

Ustawa z 26.04.2024 (Dz.U. 2024 poz. 731) obowiązuje od **28.06.2025**. Jesteśmy niemal na pewno
w zakresie jako „usługi handlu elektronicznego". Wyłączenie dla mikroprzedsiębiorcy przerośniemy,
a klienci niebędący mikro będą wymagać zgodności umownie. Standard: **EN 301 549 / WCAG 2.1 AA**,
z wyraźnie wskazanym wymogiem dostępności **funkcji dokonywania płatności** (`LEG-011`).

**Wpisujemy to w tokeny od pierwszego dnia.** Retrofit jest droższy i blokuje przetargi sieciowe.

| Kryterium | Wymóg | Jak egzekwowane |
|---|---|---|
| 1.4.3 Kontrast tekstu | ≥ 4,5:1 dla tekstu, ≥ 3:1 dla dużego | Wszystkie pary tokenów przetestowane w obu motywach. Test w CI |
| 1.4.11 Kontrast elementów | ≥ 3:1 dla obramowań i ikon niosących znaczenie | jw. |
| 1.4.1 Bez samego koloru | Status zawsze kolor **+** tekst lub ikona | §2.4, znaczniki alergenów, stany zamówień |
| 2.5.5 Rozmiar celu | ≥ 44 px, u nas 48 px | Token `--touch-min` |
| 2.4.7 Widoczny fokus | Obrys 2 px o kontraście ≥ 3:1, nigdy `outline: none` | Reguła globalna, blokada w lintingu |
| 2.1.1 Obsługa klawiaturą | Pełna, także panel płatności | Panel i KDS krytycznie — KDS bywa obsługiwany klawiaturą |
| 2.3.3 Ruch | Poszanowanie `prefers-reduced-motion` | §5 |
| 3.1.1 Język strony | `lang` zgodny z wybranym językiem gościa | Ustawiane przy przełączeniu języka |
| 4.1.2 Nazwa, rola, wartość | Semantyczny HTML, ARIA tylko gdy semantyka nie wystarcza | Przegląd komponentów |
| 1.3.1 Informacja i relacje | Alergeny powiązane z pozycją programowo, nie tylko wizualnie | `LEG-009` |

**Trzy obowiązki formalne** wynikające z ustawy — do realizacji przed komercyjnym startem:
sekcja „Dostępność" w regulaminie, zgłoszenie do Ministra Cyfryzacji w razie niezgodności,
udokumentowana ocena adekwatności przy powoływaniu się na wyłączenie z art. 21.

---

## 7. Budżet wydajności

Koncepcja stawia cele „< 20 s" i „< 8 s", ale ich nie rozbija. Cel bez dekompozycji jest
życzeniem (`P11`). Poniżej rozbicie, które da się zmierzyć w CI.

### 7.1. Nowy gość — budżet 20 s

| Etap | Budżet | Kto odpowiada |
|---|---|---|
| Skan kodu → otwarcie przeglądarki | ~2 s | Urządzenie. **NFC skraca do ~0,5 s** (`F-G-005`) |
| DNS + TLS + pierwszy bajt | 400 ms | Brzeg, cache CDN |
| First Contentful Paint | **≤ 1,0 s od żądania** | Krytyczny CSS inline, SSR |
| Largest Contentful Paint | ≤ 2,0 s | Pierwsze zdjęcia, priorytet ładowania |
| Interaktywność | ≤ 2,5 s | Minimalny JavaScript |
| Przeglądanie i decyzja | 10–13 s | **Czas ludzki.** Optymalizujemy architekturą informacji, nie kodem |
| Dodanie do koszyka | ≤ 300 ms na reakcję | Aktualizacja optymistyczna |
| Potwierdzenie zamówienia | ≤ 500 ms w obie strony | |
| **Razem** | **< 20 s** | |

### 7.2. Gość powracający — budżet 8 s

| Etap | Budżet |
|---|---|
| Skan → otwarcie | ~2 s (NFC: 0,5 s) |
| Wznowienie z Service Workera | ≤ 400 ms |
| Ekran z widocznym „Zamów to samo" | ≤ 800 ms |
| Decyzja gościa | 3–4 s |
| Dwa tapnięcia + potwierdzenie | ≤ 800 ms |
| **Razem** | **< 8 s** |

⚠️ To działa **wyłącznie wtedy**, gdy „Zamów to samo" jest nad zgięciem przy powrocie
(`TUN-001`). Przy tym budżecie nie ma miejsca na przewijanie do przycisku.

### 7.3. Budżet wagi — PWA gościa

| Zasób | Limit | Uwagi |
|---|---|---|
| HTML pierwszej odpowiedzi | **≤ 14 kB** po kompresji | Mieści się w pierwszym oknie TCP |
| Krytyczny CSS wbudowany | ≤ 8 kB | Tylko pierwszy ekran |
| JavaScript początkowy | **≤ 60 kB** po gzip | Reszta doładowywana |
| Obrazy pierwszego ekranu | ≤ 120 kB łącznie | WebP/AVIF, `loading=lazy` poniżej zgięcia |
| **Całość pierwszego widoku** | **≤ 200 kB** | Twardy limit. Test w CI |
| Pobrane kroje pisma | **0 kB** | §3.1 |

**Egzekwowanie:** budżet sprawdzany w CI. Przekroczenie to niezaliczona kompilacja, nie ostrzeżenie.

### 7.4. Pozostałe powierzchnie

| Powierzchnia | Wymaganie |
|---|---|
| Kelner Pro | Tablica stolików interaktywna ≤ 1,5 s. Działa offline dla odczytu |
| KDS | Nowe zamówienie widoczne ≤ 2 s od złożenia (p95). Sesja stabilna przez 14 h |
| Panel | Bez twardego limitu. Pierwszy widok ≤ 3 s |

---

## 8. Inwentarz komponentów

Komponenty wspólne budowane raz, stylowane tokenami per powierzchnia.

### 8.1. Wspólne

`Button` (primary / accent / secondary / ghost / danger) · `IconButton` · `Input` · `Textarea` ·
`Select` · `Checkbox` · `Radio` · `Switch` · `Badge` · `Chip` · `Card` · `Sheet` (panel dolny) ·
`Dialog` · `Toast` · `Tabs` · `Skeleton` · `EmptyState` · `ErrorState` · `Spinner` ·
`Avatar` · `Money` (formatowanie kwot) · `Timer` · `LanguageSwitcher` · `ThemeToggle`

### 8.2. Gość

`MenuCategoryNav` · `MenuItemCard` · `MenuItemSheet` · `AllergenChips` · `AllergenLegend` ·
`ModifierGroup` · `QtyStepper` · `CartBar` (przyklejony dół) · `CartSheet` ·
`OrderStatusTracker` · `CallWaiterButton` (obecny na **każdym** ekranie) · `ReorderCard` ·
`BillSummary` · `SplitModeSelector` · `SplitShareCard` · `TipSelector` · `PaymentMethodPicker` ·
`ConsentCheckboxGroup` · `OfflineBanner` · `EightySixBanner`

### 8.3. Kelner

`TableGrid` · `TableCard` (stan + licznik + alerty) · `CallQueue` · `OrderTakeSheet` ·
`AlcoholConfirmDialog` · `TipFeed` · `MyStatsCard` · `SessionCloseDialog` ·
`CashSettlementSheet` · `ShiftSummary`

### 8.4. Kuchnia

`TicketCard` (duża skala) · `TicketColumn` · `StationTabs` · `BumpButton` ·
`EightySixToggle` · `CourseTimer` · `ConnectionIndicator`

### 8.5. Panel

`DataTable` (sortowanie, filtry, eksport) · `MetricTile` · `TrendChart` · `HeatmapChart` ·
`MenuEditor` · `MenuItemForm` · `AllergenPicker` · `TranslationPanel` · `FloorPlanEditor` ·
`QrGenerator` · `StaffTable` · `OnboardingChecklist` · `PosPairingWizard` ·
`MenuEngineeringMatrix` · `FiscalDiscrepancyList`

---

## 9. Ikonografia i obrazy

**Ikony:** jeden zestaw konturowy, grubość 1,5 px, siatka 24 px. Ikona nigdy nie występuje sama
jako jedyny nośnik znaczenia — zawsze ma etykietę tekstową albo `aria-label` (WCAG 1.4.1).

**Alergeny:** 14 piktogramów z rozporządzenia (UE) 1169/2011 **plus etykieta tekstowa**.
Piktogram sam w sobie nie realizuje obowiązku informacyjnego — legenda musi być na **tym samym
ekranie** co pozycja (`LEG-009`).

**Zdjęcia potraw:**

| Wymaganie | Wartość |
|---|---|
| Format | AVIF z zapasowym WebP |
| Proporcje | 4:3 na liście, 16:9 na ekranie pozycji |
| Szerokości | 320 / 640 / 960 px, `srcset` |
| Waga po kompresji | ≤ 40 kB dla 640 px |
| Zastępczo | Blok w kolorze kategorii z nazwą. **Nigdy pusty prostokąt ani ikona łamanego obrazka** |
| Tekst alternatywny | Nazwa pozycji. Zdjęcie dekoracyjne → `alt=""` |

⚠️ **Brak zdjęcia jest normą, nie wyjątkiem.** Większość lokali nie ma sesji zdjęciowej całego
menu. Wariant bez zdjęcia musi wyglądać dobrze — nie „zepsuto".

---

## 10. Formaty polskie

| Rodzaj | Format | Przykład |
|---|---|---|
| Kwota | Przecinek dziesiętny, spacja nierozdzielająca przed `zł`, `zł` małą literą | `123,45 zł` |
| Kwota z tysiącami | Spacja nierozdzielająca jako separator | `1 234,56 zł` |
| Data | `dd.MM.yyyy` | `16.08.2026` |
| Data opisowa | dzień + miesiąc w dopełniaczu | `16 sierpnia` |
| Godzina | 24-godzinna, dwucyfrowa | `20:30` |
| Data i godzina | Kropka rozdzielająca | `16.08 · 20:30` |
| Czas trwania | Minuty do 90, dalej godziny | `45 min`, `2 h 15 min` |
| Numer stolika | Bez znaku `#` | `Stolik 12` |
| Numer telefonu | Grupy po trzy | `+48 573 568 812` |
| NIP | Grupowany | `123-456-78-90` |
| Procent | Bez spacji przed znakiem | `10%` |

Języki: `pl` (podstawowy), `uk`, `en`, `de`. Wszystkie formaty liczbowe i datowe **z lokalizacją**.
Interfejs w języku ukraińskim używa polskiego formatu waluty — gość płaci w złotych.

---

## 11. Ton wypowiedzi

### 11.1. Gość — po imieniu, uprzejmie, konkretnie

Polski rynek gastronomiczny w kanale cyfrowym mówi na „ty". Wolt na własnych materiałach:
„Zeskanuj, zamów, zapłać i pomiń kolejkę". Trzymamy się tego rejestru.

| Zasada | Dobrze | Źle |
|---|---|---|
| Tryb rozkazujący w wezwaniach do działania | `Zamawiam` · `Zapłać` · `Poproszę kelnera` | `Kliknij tutaj, aby złożyć zamówienie` |
| Konkret zamiast uprzejmości pustej | `Gotowe za ok. 12 minut` | `Twoje zamówienie jest przetwarzane` |
| Błąd mówi, co zrobić | `Płatność nie przeszła. Spróbuj ponownie lub zapłać u kelnera.` | `Wystąpił błąd. Coś poszło nie tak.` |
| Bez żargonu technicznego | `Brak połączenia` | `Błąd 503 · Service Unavailable` |
| Bez wykrzykników i emoji w komunikatach systemowych | `Zamówienie przyjęte` | `Super! Zamówienie przyjęte! 🎉` |

**Zakazane sformułowania:** „Ups!", „Coś poszło nie tak" bez wskazania działania, „Proszę czekać"
bez informacji jak długo, „Twoje zamówienie" (wystarczy „Zamówienie").

### 11.2. Personel — krótko, operacyjnie

`Stolik 12 woła` · `Potwierdź wiek` · `Rachunek 25 min bez płatności` · `Zamknij sesję`

Bez uprzejmości. Kelner czyta to w biegu, w ciemności, jednym spojrzeniem.

### 11.3. Panel — rzeczowo, profesjonalnie

Pełne zdania, forma bezosobowa. `Rotacja stolika wzrosła o 11% wobec poprzedniego miesiąca.`

### 11.4. Miejsca o wadze prawnej

Treści, których **nie wolno** przeredagowywać bez konsultacji prawnej — sformułowanie ma
skutki prawne, nie tylko komunikacyjne:

- Zgody marketingowe (art. 398 PKE) — `LEG-007`
- Informacja o alergenach i legenda — `LEG-009`
- Wskazanie sprzedawcy alkoholu — sprzedawcą **zawsze jest lokal**, nie my (`LEG-004`)
- Ekran napiwku — musi jasno komunikować, że napiwek trafia do kelnera i jest dobrowolny (`LEG-006`)
- Informacja o dostępności płatności gotówką (`LEG-012`)

---

## 12. Powiązane dokumenty

- Makiety stosujące ten system → [`06`](06_Ekrany_Gosc.md), [`07`](07_Ekrany_Kelner_KDS.md), [`08`](08_Ekrany_Panel.md), [`09`](09_Ekrany_v2_v3.md)
- Wymóg podziału frontendów wynikający z budżetu → [`04`](04_Architektura_Moduly.md) §1.2
- Kandydaci do tuningu wyglądu → [`10`](10_Tuning_Decyzje_Ryzyka.md) §2
