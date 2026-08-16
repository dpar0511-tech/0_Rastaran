# 08 · Ekrany — Panel managera i onboarding

> Wymagany kontekst: [`00_INDEX.md`](00_INDEX.md), tokeny z [`05`](05_System_Projektowy.md).
> Uprawnienia ról: [`02`](02_Aktorzy_Scenariusze.md) §5.

**Kontekst:** biurko, laptop, analiza po zmianie, długie sesje. Motyw jasny domyślnie,
gęste tabele, bez ograniczeń wagi. Widok odniesienia: **1440 px**.

---

## Spis ekranów

| ID | Ekran | Wydanie |
|---|---|---|
| [`SCR-P-01`](#scr-p-01--kreator-uruchomienia) | Kreator uruchomienia lokalu | v0.1 |
| [`SCR-P-02`](#scr-p-02--edytor-menu) | Edytor menu | v0.1 |
| [`SCR-P-03`](#scr-p-03--pozycja-menu) | Pozycja menu | v0.1 |
| [`SCR-P-04`](#scr-p-04--plan-sali-i-kody-qr) | Plan sali i kody QR | v0.1 |
| [`SCR-P-05`](#scr-p-05--personel-i-sekcje) | Personel i sekcje | v0.1 |
| [`SCR-P-06`](#scr-p-06--pulpit) | Pulpit | v0.1 |
| [`SCR-P-07`](#scr-p-07--rotacja-stolika) | Rotacja stolika | v0.1 |
| [`SCR-P-08`](#scr-p-08--rachunki-i-zaległości-fiskalne) | Rachunki i zaległości fiskalne | v0.2 |
| [`SCR-P-09`](#scr-p-09--parowanie-pos) | Parowanie POS | v0.2 |
| [`SCR-P-10`](#scr-p-10--menu-engineering) | Menu engineering | v1 |
| [`SCR-P-11`](#scr-p-11--analityka-kelnerów) | Analityka kelnerów | v1 |
| [`SCR-P-12`](#scr-p-12--goście-i-opinie) | Goście i opinie | v1 |
| [`SCR-P-13`](#scr-p-13--plan-i-uprawnienia) | Plan i uprawnienia | v0.1 |

---

## `SCR-P-01` · Kreator uruchomienia lokalu

**Realizuje:** `F-P-010` · **Brak w koncepcji — dodany (`P7`)**

Od tego ekranu zależy obietnica „szkolenie personelu w 40 minut". Lokal, który utknie na
onboardingu, nigdy nie dojdzie do pilotu.

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Bar Zdrój · Uruchomienie                                    3 z 7 gotowe  │
├────────────────────────────────────────────────────────────────────────────┤
│  ████████████░░░░░░░░░░░░░░░░░░░  43%                                      │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                            │
│  ✓  1. Dane lokalu                                          gotowe         │
│     Nazwa, adres, godziny otwarcia, NIP                     [ edytuj ]     │
│                                                                            │
│  ✓  2. Karta menu                                     124 pozycje          │
│     Zaimportowana z pliku · 12.08                           [ edytuj ]     │
│                                                                            │
│  ⚠  3. Alergeny                              108 ze 124 uzupełnione        │
│     ┌──────────────────────────────────────────────────────────────────┐   │
│     │  16 pozycji bez informacji o alergenach.                         │   │
│     │  Bez tego nie można uruchomić lokalu — to obowiązek prawny.       │   │
│     │                                        [ UZUPEŁNIJ 16 POZYCJI ]  │   │
│     └──────────────────────────────────────────────────────────────────┘   │
│                                                                            │
│  ○  4. Tłumaczenia                             PL ✓  UA ○  EN ○  DE ○      │
│     Przetłumaczymy automatycznie, Ty sprawdzasz     [ PRZETŁUMACZ ]        │
│                                                                            │
│  ○  5. Stoliki i kody QR                              0 stolików           │
│     Ustaw plan sali i wygeneruj kody              [ USTAW STOLIKI ]        │
│                                                                            │
│  ○  6. Personel                                       0 osób               │
│     Dodaj kelnerów, kucharzy, managerów             [ DODAJ LUDZI ]        │
│                                                                            │
│  ○  7. Integracja POS                          tryb bez POS                │
│     Możesz zacząć bez POS-a. Fiskalizację prowadzi                         │
│     wtedy lokal na własnej kasie.                     [ SKONFIGURUJ ]      │
│                                                                            │
├────────────────────────────────────────────────────────────────────────────┤
│  Do uruchomienia brakuje: alergeny, stoliki, personel                      │
│                                              [ URUCHOM LOKAL ]  nieaktywne │
└────────────────────────────────────────────────────────────────────────────┘
```

### Bramki uruchomienia

| Krok | Wymagany? | Uzasadnienie |
|---|:---:|---|
| Dane lokalu | ✅ | Bez adresu nie wskażemy administratora danych w zgodach (`LEG-007`) |
| Karta menu | ✅ | Oczywiste |
| **Alergeny — komplet** | ✅ | Obowiązek prawny **bez progu wielkości lokalu**. Pozycja bez alergenów nie przechodzi publikacji (`RULE-010`, `I8`, `LEG-009`) |
| Tłumaczenia | ❌ | PL wystarczy do startu. UA/EN/DE zwiększają zasięg, nie warunkują |
| Stoliki i kody | ✅ | Bez kodów nie ma produktu |
| Personel | ✅ | Minimum jeden kelner i jeden manager |
| Integracja POS | ❌ | **Tryb bez POS jest pełnoprawny** (`P2`). Kluczowe dla beachheadu — ogródki piwne często nie mają POS-a |

### Import menu — trzy drogi

```
┌──────────────────────────────────────────────────────────────────────┐
│  Skąd weźmiemy menu?                                                 │
│                                                                      │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐          │
│  │   Z POS-a      │  │   Z pliku      │  │  Ze zdjęcia    │          │
│  │                │  │                │  │  lub PDF       │          │
│  │ Najszybciej.   │  │ CSV lub XLSX.  │  │ Odczytamy AI,  │          │
│  │ Ceny same się  │  │ Damy szablon.  │  │ Ty poprawiasz. │          │
│  │ synchronizują. │  │                │  │                │          │
│  │  [ WYBIERZ ]   │  │  [ WYBIERZ ]   │  │  [ WYBIERZ ]   │          │
│  └────────────────┘  └────────────────┘  └────────────────┘          │
│                                                                      │
│              albo wpisz ręcznie w edytorze →                         │
└──────────────────────────────────────────────────────────────────────┘
```

⚠️ Import ze zdjęcia daje `MenuItemTranslation.source = 'ai'` oraz alergeny do weryfikacji.
**Żadne z nich nie przejdzie publikacji bez ludzkiej korekty** (`I12`, `RULE-010`).
Za poprawność danych o alergenach odpowiada lokal (art. 8 FIC) — kreator musi to komunikować wprost.

---

## `SCR-P-02` · Edytor menu

**Realizuje:** `F-P-009`

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Menu · Bar Zdrój            wersja 12 · opublikowano 14.08 18:22          │
├──────────────────┬─────────────────────────────────────────────────────────┤
│  KATEGORIE       │  🔍 Szukaj      ⚠ 16 bez alergenów   [ + POZYCJA ]      │
│                  ├─────────────────────────────────────────────────────────┤
│  ⣿ Piwo      18  │  PRZEKĄSKI                                              │
│  ⣿ Przekąski 12  │  ┌───────────────────────────────────────────────────┐  │
│  ⣿ Dania     24  │  │ ⣿ ▓▓ Talerz serów      32,00 zł  8%  ⓖⓜⓝ  ●   ⋮ │  │
│  ⣿ Desery     8  │  │ ⣿ ▓▓ Krążki cebulowe   18,00 zł  8%  ⓖⓙ   ●   ⋮ │  │
│  ⣿ Napoje    16  │  │ ⣿ ▓▓ Tatar wołowy      45,00 zł  8%  ⓙ    ○   ⋮ │  │
│  ⣿ Alkohole  46  │  │ ⣿ ▓▓ Nachos            26,00 zł  8%  ⚠     ●   ⋮ │  │
│                  │  └───────────────────────────────────────────────────┘  │
│  [ + KATEGORIA ] │            ⣿ przeciągnij     ● dostępne  ○ 86           │
│                  │            ⚠ brak alergenów                             │
│                  │                                                         │
│                  │  DANIA                                                  │
│                  │  ┌───────────────────────────────────────────────────┐  │
│                  │  │ ⣿ ▓▓ Burger Zdrój      38,00 zł  8%  ⓖⓜⓙⓢ ●   ⋮ │  │
│                  │  └───────────────────────────────────────────────────┘  │
├──────────────────┴─────────────────────────────────────────────────────────┤
│  ⚠ 3 niezapisane zmiany         [ ODRZUĆ ]        [ OPUBLIKUJ ZMIANY ]    │
└────────────────────────────────────────────────────────────────────────────┘
```

### Publikacja jest jawną czynnością

Zmiany w edytorze **nie trafiają od razu do gości**. `OPUBLIKUJ ZMIANY` wywołuje
`EVT-menu.published`, które unieważnia cache brzegowy i podnosi wersję menu.

**Uzasadnienie:** manager poprawiający ceny w środku serwisu nie może zmieniać menu gościom
w trakcie kompletowania zamówienia. Publikacja to jedna, kontrolowana chwila.

Wyjątek: **lista 86 działa natychmiast** i nie wymaga publikacji (`F-D-002`) — to zmiana
operacyjna, nie edycja katalogu.

### Blokada publikacji

```
┌──────────────────────────────────────────────────────────────────────┐
│  ⚠  Nie można opublikować                                            │
│                                                                      │
│  4 pozycje nie mają informacji o alergenach:                         │
│    · Nachos            · Zupa dnia                                   │
│    · Deser lodowy      · Lemoniada domowa                            │
│                                                                      │
│  Informacja o 14 alergenach jest obowiązkowa dla żywności            │
│  nieopakowanej i musi być dostępna przed złożeniem zamówienia.       │
│                                                                      │
│                          [ UZUPEŁNIJ ]      [ Anuluj ]               │
└──────────────────────────────────────────────────────────────────────┘
```

To jest **twarda bramka**, nie ostrzeżenie do zignorowania (`I8`).

---

## `SCR-P-03` · Pozycja menu

```
┌────────────────────────────────────────────────────────────────────────────┐
│  ←  Burger Zdrój                                       [ ZAPISZ ]          │
├────────────────────────────────────────────────────────────────────────────┤
│  PODSTAWY          │  ALERGENY  ⚠      │  TŁUMACZENIA     │  MODYFIKATORY  │
├────────────────────┴───────────────────┴──────────────────┴────────────────┤
│                                                                            │
│  Nazwa            ┌──────────────────────────────────────┐                 │
│                   │ Burger Zdrój                         │                 │
│                   └──────────────────────────────────────┘                 │
│  Opis             ┌──────────────────────────────────────┐                 │
│                   │ Wołowina 200 g, ser cheddar, boczek, │                 │
│                   │ karmelizowana cebula, bułka maślana  │                 │
│                   └──────────────────────────────────────┘                 │
│                                                                            │
│  Cena brutto      ┌────────────┐        VAT   ┌──────────┐                 │
│                   │  38,00 zł  │              │ 8%    ▾  │                 │
│                   └────────────┘              └──────────┘                 │
│                                                                            │
│  Koszt surowca    ┌────────────┐        Marża     14,20 zł · 37%           │
│                   │  23,80 zł  │        🔒 niewidoczne dla kelnerów        │
│                   └────────────┘                                           │
│                                                                            │
│  Czas przygot.    ┌────────────┐  min   Stacja  ┌──────────┐               │
│                   │     12     │                │ Grill ▾  │               │
│                   └────────────┘                └──────────┘               │
│                                                                            │
│  [ ] Zawiera alkohol — wymaga potwierdzenia wieku przy podaniu             │
│                                                                            │
│  Zdjęcie          ┌──────────────┐                                         │
│                   │  ▓▓▓▓▓▓▓▓▓▓  │  [ Zmień ]  [ Usuń ]                   │
│                   │  ▓▓▓▓▓▓▓▓▓▓  │  Bez zdjęcia też wygląda dobrze         │
│                   └──────────────┘                                         │
└────────────────────────────────────────────────────────────────────────────┘
```

### Zakładka alergenów

```
│  Zawiera                                                                   │
│  [✓] ⓖ Gluten      [✓] ⓜ Mleko      [✓] ⓙ Jaja       [✓] ⓢ Sezam          │
│  [ ] ⓝ Orzechy     [ ] ⓟ Orzeszki   [ ] ⓡ Ryby       [ ] ⓚ Skorupiaki     │
│  [ ] ⓞ Soja        [ ] ⓔ Seler      [ ] ⓤ Gorczyca   [ ] ⓛ Łubin          │
│  [ ] ⓑ Mięczaki    [ ] ⓓ Dwutlenek siarki                                  │
│                                                                            │
│  Może zawierać (zanieczyszczenie krzyżowe)                                 │
│  [✓] ⓝ Orzechy                                                             │
│                                                                            │
│  ( ) Ta pozycja nie zawiera żadnego z 14 alergenów                         │
│                                                                            │
│  ⓘ Za poprawność tych danych odpowiada lokal (art. 8 rozporządzenia FIC).   │
│    My odpowiadamy za ich prawidłowe wyświetlenie.                          │
```

**Opcja „nie zawiera żadnego" jest konieczna** — inaczej pozycje bez alergenów nie przejdą
bramki publikacji, a manager zacznie zaznaczać cokolwiek, żeby ją ominąć. Wtedy dane o alergenach
przestają być wiarygodne, a to gorsze niż ich brak.

### Zakładka tłumaczeń

```
│  Język     Nazwa                          Status                           │
│  🇵🇱 PL     Burger Zdrój                    źródło                          │
│  🇺🇦 UA     Бургер Zdrój                    ⚠ AI — wymaga sprawdzenia       │
│  🇬🇧 EN     Zdrój Burger                    ✓ sprawdzone                    │
│  🇩🇪 DE     Zdrój Burger                    ⚠ AI — wymaga sprawdzenia       │
│                                                                            │
│  [ PRZETŁUMACZ BRAKUJĄCE ]        [ OZNACZ WSZYSTKIE JAKO SPRAWDZONE ]     │
```

Tłumaczenie oznaczone `AI` nie trafia do opublikowanego menu (`I12`). Gość zobaczy wersję
polską zamiast niesprawdzonego tłumaczenia — lepiej zrozumiały obcy język niż błędna nazwa dania
przy alergii.

---

## `SCR-P-04` · Plan sali i kody QR

**Realizuje:** `F-P-011`, `F-G-008`, `F-G-005`

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Stoliki · Bar Zdrój                              [ + STREFA ] [ DRUKUJ ]  │
├──────────────────┬─────────────────────────────────────────────────────────┤
│  STREFY          │  SALA GŁÓWNA · 14 stolików                              │
│                  │  ┌───────────────────────────────────────────────────┐  │
│  ● Sala główna   │  │                                                   │  │
│    stoliki 1–14  │  │   ┌──┐  ┌──┐  ┌──┐        ┌────┐   ┌────┐         │  │
│    kelner: Marek │  │   │ 1│  │ 2│  │ 3│        │  7 │   │  8 │         │  │
│                  │  │   └──┘  └──┘  └──┘        └────┘   └────┘         │  │
│  ○ Taras         │  │    2os   2os   2os          4os      4os          │  │
│    stoliki 15–24 │  │                                                   │  │
│    kelner: Ania  │  │   ┌──┐  ┌──┐              ┌──────────┐            │  │
│                  │  │   │ 4│  │ 5│              │    12    │            │  │
│  ○ Bar           │  │   └──┘  └──┘              └──────────┘            │  │
│    stoliki 25–30 │  │    2os   2os                  8os                 │  │
│    kelner: Piotr │  │                                                   │  │
│                  │  │           [ + DODAJ STOLIK ]                      │  │
│  [ + STREFA ]    │  └───────────────────────────────────────────────────┘  │
├──────────────────┴─────────────────────────────────────────────────────────┤
│  KODY DO DRUKU                                                             │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐                              │
│  │  ▪▪ ▪ ▪▪   │ │  ▪▪ ▪ ▪▪   │ │  ▪▪ ▪ ▪▪   │   Format: ○ Naklejka        │
│  │ ▪ ▪▪▪▪ ▪   │ │ ▪ ▪▪▪▪ ▪   │ │ ▪ ▪▪▪▪ ▪   │           ● Stojak trójkąt. │
│  │  ▪▪ ▪ ▪▪   │ │  ▪▪ ▪ ▪▪   │ │  ▪▪ ▪ ▪▪   │           ○ Podkładka       │
│  │  STOLIK 1  │ │  STOLIK 2  │ │  STOLIK 3  │                              │
│  │Zeskanuj    │ │Zeskanuj    │ │Zeskanuj    │   [✓] Dodaj tag NFC          │
│  │i zamów     │ │i zamów     │ │i zamów     │       ok. 1,50 zł/szt.       │
│  └────────────┘ └────────────┘ └────────────┘                              │
│                                                                            │
│  [ POBIERZ PDF DO DRUKU ]     [ ZAMÓW STOJAKI U NAS ]                      │
└────────────────────────────────────────────────────────────────────────────┘
```

### Kody są statyczne — na zawsze

`TableToken.token` nie zmienia się nigdy (`RULE-013`, `F-G-008`). Cała dynamika jest po stronie
serwera. Restauracje nie znoszą przedrukowywania kodów, a wyblakła naklejka z martwym kodem to
gwarantowana rezygnacja z systemu.

Unieważnienie kodu (kradzież stojaka, zmiana układu sali) tworzy **nowy rekord** z `revoked_at`
na starym — nigdy nie edytuje istniejącego.

### Jakość fizyczna ma znaczenie

Jedną z przyczyn niepowodzenia QR po pandemii był tani wygląd — zalaminowana kartka A4.
Dlatego oferujemy stojaki w stylu lokalu i sami je montujemy przy wdrożeniu. To pozycja
w koszcie pozyskania klienta, nie dodatek.

---

## `SCR-P-05` · Personel i sekcje

**Realizuje:** `F-P-012`, `F-K-006`

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Personel · Bar Zdrój                                    [ + DODAJ OSOBĘ ] │
├────────────────────────────────────────────────────────────────────────────┤
│  Osoba          Rola       Sekcja        Konto napiwków    Status          │
│  ─────────────────────────────────────────────────────────────────────────  │
│  ▓ Marek K.     Kelner     Sala główna   ✓ zweryfikowane   ● na zmianie    │
│  ▓ Ania W.      Kelner     Taras         ✓ zweryfikowane   ● na zmianie    │
│  ▓ Piotr S.     Kelner     Bar           ⚠ brak            ○ poza zmianą   │
│  ▓ Ewa N.       Kucharz    —             —                 ● na zmianie    │
│  ▓ Tomek L.     Manager    —             —                 ● na zmianie    │
│                                                                            │
│  ⚠ Piotr nie ma konta napiwków. Goście przy jego stolikach                 │
│    nie zobaczą opcji napiwku.                    [ WYŚLIJ PRZYPOMNIENIE ]  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Manager nie może dodać konta wypłat za kelnera.** Konto powstaje w PSP, weryfikuje je sam
kelner. Gdyby manager miał do niego dostęp, powstałby argument, że lokal ma władztwo nad
napiwkami — a to dokładnie ta przesłanka, która przekwalifikowuje napiwek na przychód
ze stosunku pracy (`LEG-006`).

---

## `SCR-P-06` · Pulpit

**Realizuje:** `F-P-014`

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Bar Zdrój · dziś, 16 sierpnia                       ⚙  Tomek L. (manager) │
├────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐       │
│  │ Sprzedaż     │ │ Rachunki     │ │ Rotacja      │ │ Przez QR     │       │
│  │  8 240 zł    │ │     94       │ │  1 h 12 min  │ │    47%       │       │
│  │  ▲ 12%       │ │   ▲ 8        │ │  ▼ 9 min     │ │   ▲ 6 p.p.   │       │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘       │
│                                                                            │
│  ⚠ WYMAGA UWAGI                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ 🔴 Stolik 7 — rachunek 142 zł, 28 min bez płatności                  │  │
│  │ 🟠 Płatność 86,90 zł niezafiskalizowana od 21:04     [ SZCZEGÓŁY ]   │  │
│  │ 🟠 Piotr S. — brak konta napiwków                                    │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  ┌───────────────────────────────────┐ ┌──────────────────────────────┐   │
│  │  SPRZEDAŻ W CIĄGU DNIA            │ │  NAJCZĘŚCIEJ ZAMAWIANE       │   │
│  │        ▁▂▃▅█▇▅▃▂▁▁▂▃▅█▇          │ │  1. Żywiec 0,5 l      142×   │   │
│  │  12   15   18   21   00           │ │  2. Burger Zdrój       58×   │   │
│  │                                   │ │  3. Frytki             51×   │   │
│  └───────────────────────────────────┘ └──────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────────┘
```

**Blok „wymaga uwagi" jest ważniejszy od wykresów** i dlatego stoi nad nimi. Manager otwiera
panel w trakcie zmiany, żeby dowiedzieć się, co jest nie tak — nie żeby oglądać trendy.

---

## `SCR-P-07` · Rotacja stolika

**Realizuje:** `F-P-007` · **Główna metryka dowodowa ROI.** Ten ekran sprzedaje pilot na 30. dzień.

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Rotacja stolika                        okres: [ ostatnie 30 dni  ▾ ]      │
├────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  PRZED WDROŻENIEM          PO WDROŻENIU              RÓŻNICA         │  │
│  │  1 h 21 min                1 h 12 min                −9 min (−11%)   │  │
│  │  (tydzień 1: 1–7.07)       (tydzień 4: 22–28.07)                     │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  CO TO ZNACZY W ZŁOTÓWKACH                                           │  │
│  │                                                                      │  │
│  │  30 stolików × 9 min oszczędności × 5 h szczytu                      │  │
│  │  = ok. 1,4 dodatkowego obrotu stolika dziennie                       │  │
│  │  × średni rachunek 87 zł × 26 dni                                    │  │
│  │                                                                      │  │
│  │  ≈  +3 170 zł miesięcznie                                            │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  ROZBICIE NA ETAPY                                                         │
│  ─────────────────────────────────────────────────────────────────────────  │
│  Zajęcie → pierwsze zamówienie     2 min 40 s     ▼ 4 min 10 s             │
│  Pierwsze zamówienie → podanie    14 min 20 s     ▼ 1 min                  │
│  Podanie → prośba o rachunek      48 min 00 s     ▬ bez zmian              │
│  Rachunek → wyjście                7 min 12 s     ▼ 3 min 50 s             │
│                                                                            │
│  ⓘ Największe oszczędności: początek i koniec wizyty. Tam działa QR.       │
│                                                    [ POBIERZ RAPORT PDF ]  │
└────────────────────────────────────────────────────────────────────────────┘
```

### Dlaczego rozbicie na etapy jest kluczowe

Sama liczba „−9 min" jest łatwa do podważenia („to przypadek", „mieliśmy słabszy miesiąc").
Rozbicie pokazuje **gdzie dokładnie** system działa: przy zajęciu stolika (gość zamawia od razu,
nie czeka na kelnera) i przy wyjściu (płaci w telefonie, nie czeka na terminal). Środek wizyty
się nie zmienia — i uczciwe przyznanie tego buduje wiarygodność liczby.

⚠️ **Pomiar bazowy musi powstać w tygodniu 1 pilotu**, zanim system realnie zadziała.
Bez punktu odniesienia cały argument ROI nie istnieje. To krok w kreatorze uruchomienia.

---

## `SCR-P-08` · Rachunki i zaległości fiskalne

**Realizuje:** obsługa `E4` · **Wydanie:** v0.2

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Rachunki                    [ dziś ▾ ]  [ wszystkie ▾ ]  [ EKSPORT CSV ]  │
├────────────────────────────────────────────────────────────────────────────┤
│  ⚠  ZALEGŁOŚCI FISKALNE (2)                                                │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ 21:04  Stolik 12   86,90 zł  BLIK   ⚠ POS nie odpowiedział           │  │
│  │        Zafiskalizuj ręcznie na kasie.       [ OZNACZ JAKO ZROBIONE ] │  │
│  ├──────────────────────────────────────────────────────────────────────┤  │
│  │ 20:41  Stolik 5    54,00 zł  Karta  ⚠ POS nie odpowiedział           │  │
│  │                                             [ OZNACZ JAKO ZROBIONE ] │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
├────────────────────────────────────────────────────────────────────────────┤
│  Godz.  Stolik  Kwota      Metoda    Kelner   Napiwek   Paragon   Status   │
│  ─────────────────────────────────────────────────────────────────────────  │
│  21:04   12     86,90 zł   BLIK      Marek     7,90 zł   ⚠        opłacony │
│  20:58    3     65,00 zł   Gotówka   Marek        —      ✓        opłacony │
│  20:41    5     54,00 zł   Karta     Ania      5,40 zł   ⚠        opłacony │
│  20:15    8    142,00 zł   —         Piotr        —      —        ⚠ 28 min │
└────────────────────────────────────────────────────────────────────────────┘
```

### Dlaczego to nie jest zwykła lista

Zaległość fiskalna to **ryzyko podatkowe lokalu**, nie usterka techniczna. Paragon musi być
wystawiony nie później niż w chwili przyjęcia zapłaty, a przy przedpłacie obowiązek podatkowy
powstaje w momencie zapłaty (`LEG-003`).

Dlatego blok stoi **nad** tabelą, jest czerwony i wymaga jawnego potwierdzenia. Płatność nigdy
nie jest cofana — pieniądze gościa już poszły (`RULE-022`, `E4`).

---

## `SCR-P-09` · Parowanie POS

**Realizuje:** `F-P-013` · **Wydanie:** v0.2

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Integracja POS                                                            │
├────────────────────────────────────────────────────────────────────────────┤
│  Twój POS   ┌────────────────────┐        ● Połączono · sprawdzono 21:04   │
│             │  Dotykačka      ▾  │                                         │
│             └────────────────────┘        [ TESTUJ POŁĄCZENIE ]            │
│                                                                            │
│  MAPOWANIE POZYCJI                              118 ze 124 dopasowanych    │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Nasza pozycja          →   Pozycja w POS                            │  │
│  │  ────────────────────────────────────────────────────────────────    │  │
│  │  Burger Zdrój           →   BURGER ZDROJ (SKU 1042)         ✓        │  │
│  │  Żywiec 0,5 l           →   ZYWIEC 500ML (SKU 2201)         ✓        │  │
│  │  Talerz serów           →   ┌──────────────────────┐        ⚠        │  │
│  │                             │  wybierz…         ▾  │                 │  │
│  │                             └──────────────────────┘                 │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  CZAS FISKALIZACJI                                    średnio 1,2 s        │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  ▁▁▂▁▁▁▃▁▁▂▁▁▁▁▂▁▁█▁▁▂▁▁          ── limit umowny 5 s               │  │
│  │  Przekroczenia w ostatnich 30 dniach: 2                              │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Chcesz działać bez POS-a?                                           │  │
│  │  Zamówienia pójdą na drukarkę bonową, a paragony wystawisz           │  │
│  │  na własnej kasie.                          [ PRZEJDŹ NA TRYB BEZ POS ] │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Wykres czasu fiskalizacji nie jest ciekawostką techniczną.** SLA na przekazanie zdarzenia
płatności jest obowiązkowym punktem umowy z lokalem, bo od niego zależy zgodność podatkowa
(`LEG-003`, `DEC-003`). Manager musi widzieć, czy jego POS się w nim mieści.

---

## `SCR-P-10` · Menu engineering

**Realizuje:** `F-P-004` · **Wydanie:** v1 — konsulting za 5 tys. zł wbudowany w abonament

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Menu engineering                       okres: [ ostatnie 30 dni ▾ ]       │
├────────────────────────────────────────────────────────────────────────────┤
│   marża ▲                                                                  │
│         │                                                                  │
│    wys. │   PUZZLES                    │            STARS                  │
│         │   wysoka marża,              │            wysoka marża,          │
│         │   niska sprzedaż             │            wysoka sprzedaż        │
│         │                              │                                   │
│         │   ● Krewetki                 │   ● Burger Zdrój                  │
│         │   ● Deska mięs               │   ● Żywiec 0,5 l                  │
│         │                              │   ● Frytki                        │
│         ├──────────────────────────────┼───────────────────────────────    │
│         │   DOGS                       │            PLOWHORSES             │
│         │   niska marża,               │            niska marża,           │
│    nis. │   niska sprzedaż             │            wysoka sprzedaż        │
│         │                              │                                   │
│         │   ● Zupa dnia                │   ● Sałatka grecka                │
│         │   ● Lemoniada                │   ● Cola 0,33                     │
│         └──────────────────────────────┴───────────────────────────────    │
│              niska                sprzedaż                wysoka  ▶        │
├────────────────────────────────────────────────────────────────────────────┤
│  CO ZROBIĆ                                                                 │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ 🟢 Burger Zdrój — Star. Zostaw jak jest, promuj w upsellu.           │  │
│  │ 🟠 Sałatka grecka — 84 sprzedaże, marża tylko 22%.                   │  │
│  │    Podnieś o 4 zł → +336 zł miesięcznie przy tej samej sprzedaży.    │  │
│  │ 🟠 Krewetki — marża 61%, ale tylko 9 sprzedaży.                      │  │
│  │    Przenieś wyżej w karcie albo dodaj do rekomendacji.               │  │
│  │ 🔴 Zupa dnia — 6 sprzedaży, marża 18%. Rozważ usunięcie.             │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Rekomendacja musi być w złotych, nie w kategoriach.** „Sałatka to Plowhorse" nic nie znaczy
dla właściciela. „Podnieś o 4 zł → +336 zł miesięcznie" to decyzja, którą podejmie w 5 sekund.
To realizacja zasady Z5.

---

## `SCR-P-11` · Analityka kelnerów

**Realizuje:** `F-P-005` · **Wydanie:** v1

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Kelnerzy                               okres: [ sierpień 2026 ▾ ]         │
├────────────────────────────────────────────────────────────────────────────┤
│  Osoba    Rachunki  Sprzedaż   Śr.rach.  Upsell   Napiwki   Rotacja  Ocena │
│  ────────────────────────────────────────────────────────────────────────  │
│  Ania W.     284    24 120 zł    85 zł   1 840 zł  3 120 zł  1h 04m   4,8  │
│  Marek K.    261    22 700 zł    87 zł   1 420 zł  2 340 zł  1h 12m   4,7  │
│  Piotr S.    198    14 850 zł    75 zł     620 zł    890 zł  1h 31m   4,3  │
│                                                                            │
│  ⓘ Piotr: najniższy upsell i najdłuższa rotacja. Brak konta napiwków       │
│    — jego goście nie widzą opcji napiwku, co zaniża też jego ocenę.        │
└────────────────────────────────────────────────────────────────────────────┘
```

⚠️ **Ten ekran jest niewidoczny dla kelnerów.** Kelner widzi wyłącznie własne dane
(`SCR-K-07`). Dwie warstwy: polityka dostępu i usuwanie pól w API ([`04`](04_Architektura_Moduly.md) §8.3).

---

## `SCR-P-12` · Goście i opinie

**Realizuje:** `F-P-001`, `F-P-002` · **Wydanie:** v1
**Główny argument przeciw Wolt: ta baza należy do lokalu.**

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Goście                    2 184 osób   ·   1 402 ze zgodą marketingową    │
├────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐       │
│  │ Nowi (30 dni)│ │ Powracający  │ │ Śr. rachunek │ │ Ocena Google │       │
│  │     412      │ │     38%      │ │    87 zł     │ │  4,6  ▲ 0,3  │       │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘       │
│                                                                            │
│  SEGMENTY RFM                          OPINIE — OSTATNIE                   │
│  ┌────────────────────────────┐  ┌──────────────────────────────────────┐  │
│  │ Czempioni          184     │  │ ★★★★★  → wysłane do Google           │  │
│  │ Lojalni            412     │  │ ★★★★★  → wysłane do Google           │  │
│  │ Zagrożeni          298     │  │ ★★☆☆☆  „Długo czekaliśmy na danie"   │  │
│  │ Utraceni           521     │  │         ⚠ prywatnie · nieodczytane    │  │
│  │ Nowi               769     │  │ ★★★☆☆  „Głośna muzyka"                │  │
│  └────────────────────────────┘  └──────────────────────────────────────┘  │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Ta baza należy do Twojego lokalu.                                   │  │
│  │  Możesz ją wyeksportować w każdej chwili.        [ EKSPORT CSV ]     │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

**Blok o własności bazy jest elementem produktu, nie ozdobnikiem.** To jednocześnie:
zgodność z art. 28 RODO (jesteśmy procesorem, `LEG-008`), realizacja zasady Z4 i najmocniejszy
argument sprzedażowy przeciw marketplace'om, które zabierają gościa sobie.

**Przechwytywanie opinii:** ocena 4–5 → link do Google Maps. Ocena 1–3 → prywatnie do managera
z alertem. Ocena Google przekłada się na ruch, a ruch na przychód — dlatego ta funkcja sprzedaje
się na pierwszym demo.

---

## `SCR-P-13` · Plan i uprawnienia

**Realizuje:** `F-P-015` · **Wydanie:** v0.1 (`P8`)

```
┌────────────────────────────────────────────────────────────────────────────┐
│  Plan i płatności                                                          │
├────────────────────────────────────────────────────────────────────────────┤
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Twój plan: PAY · 249 zł netto/miesiąc                               │  │
│  │  Okres próbny do 15.09.2026 (30 dni)                                 │  │
│  │  Prowizja od płatności: 1,9% + 0,30 zł — bez dopłat                  │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
│                                                                            │
│  Funkcja                             Menu   Order   Pay   Growth           │
│  ────────────────────────────────────────────────────────────────────────  │
│  Menu QR, alergeny, 4 języki           ✓      ✓      ✓      ✓             │
│  Wezwanie kelnera                      ✓      ✓      ✓      ✓             │
│  Zamawianie przy stoliku               —      ✓      ✓      ✓             │
│  Aplikacja kelnera, KDS                —      ✓      ✓      ✓             │
│  Płatności, napiwki, podział rach.     —      —      ✓      ✓             │
│  CRM, opinie, menu engineering         —      —      —      ✓             │
│                                                          [ ZMIEŃ PLAN ]    │
│                                                                            │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │  Bez limitów zamówień. Nie dopłacasz za wzrost.                      │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘
```

⚠️ **Uprawnienia planu są egzekwowane na granicy API, nie w tym interfejsie** (`RULE-025`,
Z-A8). Ukrycie funkcji w panelu nie jest zabezpieczeniem.

---

## Lista kontrolna dla ekranów panelu

- [ ] Blok „wymaga uwagi" nad wykresami — manager przychodzi po problemy, nie po trendy
- [ ] Każda liczba ma porównanie: okres wcześniejszy albo cel
- [ ] Rekomendacje wyrażone **w złotych**, nie w kategoriach (zasada Z5)
- [ ] Tabele: sortowanie, filtry, eksport CSV
- [ ] Dane finansowe niedostępne dla ról bez uprawnień — dwie warstwy
- [ ] Twarde bramki tam, gdzie chodzi o zgodność prawną (alergeny, fiskalizacja)
- [ ] Pełna obsługa klawiaturą, WCAG 2.1 AA
- [ ] Stany: ładowanie, brak danych, błąd, brak uprawnień

---

## Powiązane dokumenty

- Ekrany gościa → [`06_Ekrany_Gosc.md`](06_Ekrany_Gosc.md)
- Ekrany personelu → [`07_Ekrany_Kelner_KDS.md`](07_Ekrany_Kelner_KDS.md)
- Funkcje v2/v3 w panelu → [`09_Ekrany_v2_v3.md`](09_Ekrany_v2_v3.md)
