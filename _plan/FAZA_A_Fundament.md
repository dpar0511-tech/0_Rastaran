# FAZA A · FUNDAMENT — plan wykonawczy

> **Zakres:** kroki `K-00` … `K-04` z [`ROADMAP.md`](../_docs_Nowa/ROADMAP.md)
> **Wydanie:** — (fundament; `K-04` liczy się już do v0.1)
> **Stan wyjściowy:** greenfield. Dokumentacja gotowa, kodu nie ma.
> **Data sporządzenia:** 2026-08-16

---

## 0. Jak czytać ten dokument

### 0.1. Czym ten dokument jest, a czym nie jest

`ROADMAP.md` opisuje kroki na poziomie **„co ma powstać i kiedy krok jest skończony"**.
Ten dokument opisuje je na poziomie **„jakie pliki, w jakiej kolejności, z jaką zawartością,
i jaką komendą to sprawdzić"**.

| To NIE jest | To jest |
|---|---|
| Nowe źródło prawdy o zakresie | Rozwinięcie `ROADMAP.md` §8 dla Fazy A |
| Powtórzenie `_docs_Nowa/` | Warstwa wykonawcza między dokumentacją a kodem |
| Zbiór gotowej implementacji | Szkielety sygnatur, DDL i konfiguracji — reszta powstaje w kodzie |
| Zmiana jakiejkolwiek decyzji produktowej | Zapis decyzji **implementacyjnych**, które dokumentacja świadomie zostawiła otwarte |

### 0.2. Hierarchia źródeł

```
_docs_Nowa/              ← ŹRÓDŁO PRAWDY. Rozbieżność rozstrzyga się na jego korzyść (ROADMAP §11.3)
    ↓
ROADMAP.md               ← plan realizacji dokumentacji. Sekcje „Zakres", „Czego NIE robić" są WIĄŻĄCE
    ↓
_plan/FAZA_A_Fundament.md ← ten dokument. Rozbicie na zadania, ścieżki, komendy
    ↓
docs/adr/                ← utrwalone decyzje implementacyjne
```

**Rozbieżności wykryte podczas planowania są zebrane w §10.** Dokument ich **nie rozstrzyga
jednostronnie** i **nie poprawia dokumentacji w miejscu** — zgłasza je do decyzji właściciela
dokumentacji, zgodnie z `ROADMAP.md` §11 zasada 3.

### 0.3. Struktura opisu kroku

Każdy krok (`K-00` … `K-04`) ma tę samą budowę:

1. **Karta kroku** — wydanie, zależności, budżet lektury (przepisane z `ROADMAP.md`)
2. **Cel**
3. **Wczytaj przed startem** — lista wiążąca, przepisana bez zmian
4. **Zadania `K-0n.m`** — ścieżki plików, zawartość, komenda weryfikująca
5. **Testy**
6. **Bramki CI dokładane w tym kroku**
7. **Definicja ukończenia**
8. **Czego NIE robić**
9. **Ryzyka kroku**

### 0.4. Zasada nadrzędna Fazy A

> Faza A nie produkuje ani jednej funkcji widocznej dla użytkownika. Produkuje **rzeczy,
> których retrofit oznacza przepisanie**: rdzeń pieniężny (`D3`), wielodostępność (`D2`),
> granice modułów (`Z-A1`–`Z-A9`), tokeny dostępności (`LEG-011`).
> **Skrócenie tej fazy jest najdroższą oszczędnością w całym projekcie.**

---

## 1. `BRAMKA-0` i krok `K-00` — brama zerowa

### 1.1. Stan zastany (zweryfikowany 2026-08-16, nie założony)

| Fakt | Dowód |
|---|---|
| `0_Rastaran` **nie jest śledzony przez git** — nie ma historii do wydzielenia | `git ls-files .` → 0 plików |
| Repozytorium nadrzędne to `C:/Users/Igor` (cały profil użytkownika) | `git rev-parse --show-toplevel` |
| Repozytorium nadrzędne **nie ma zdalnego** i śledzi projekt Drukarnia ERP | `git remote -v` pusty, `git ls-files` → `Desktop/…` |
| Brak `package.json`, `tsconfig*.json`, `*.ts` w drzewie | `find . -maxdepth 3` |

> **Konsekwencja:** `DEC-014` jest **tańsze**, niż zakłada [`00_INDEX.md`](../_docs_Nowa/00_INDEX.md) §8.
> Dokumentacja pisze „leży wewnątrz repozytorium projektu Drukarnia ERP" i sugeruje wydzielenie
> historii. Historii nie ma — katalog jest po prostu nieśledzony. Wystarczy `git init` na miejscu
> plus zabezpieczenie przed przypadkowym wciągnięciem do repozytorium nadrzędnego.

### 1.2. `K-00` — karta kroku

| | |
|---|---|
| **Wydanie** | — (brama, nie krok programistyczny w rozumieniu ROADMAP) |
| **Zależy od** | — |
| **Odblokowuje** | `K-01` |
| **Budżet lektury** | ~3 k tokenów |

**Cel.** Zamknąć `BRAMKA-0`, żeby pierwszy commit kodu trafił do właściwego repozytorium
i żeby wybór hostingu nie zablokował `K-01` w połowie.

**Wczytaj przed startem**
- [`ROADMAP.md`](../_docs_Nowa/ROADMAP.md) §2 (`BRAMKA-0`), §3.3 (`DEC-020`)
- [`10_Tuning_Decyzje_Ryzyka.md`](../_docs_Nowa/10_Tuning_Decyzje_Ryzyka.md) §3.2 (`DEC-010`, `DEC-014`), `LEG-008`

---

### `K-00.1` · Własne repozytorium git (`DEC-014`)

**Powstaje**

| Plik | Zawartość |
|---|---|
| `.gitignore` | `node_modules/`, `.next/`, `dist/`, `.turbo/`, `coverage/`, `.env`, `.env.*` (bez `.env.example`), `storage/`, `*.log` |
| `.gitattributes` | `* text=auto eol=lf` — **konieczne**, praca na Windows, a CI na Linuksie |
| `README.md` | Nazwa, jednozdaniowy opis, `pnpm install && pnpm dev`, odesłanie do `_docs_Nowa/00_INDEX.md` |

**Procedura**

```bash
cd "C:/Users/Igor/Desktop/Projekt/0_Rastaran"
git init -b main
git add .gitattributes .gitignore README.md _docs_Nowa _plan CLAUDE.md
git commit -m "chore: initial repository with product documentation"
```

**Zabezpieczenie przed repozytorium nadrzędnym** — jedna z dwóch dróg:

| Wariant | Czynność | Ocena |
|---|---|---|
| **A — wpis w ignore** | Dopisać `Desktop/Projekt/0_Rastaran/` do `C:/Users/Igor/.gitignore` | Szybkie. Zagnieżdżone repozytorium zostaje, ale nadrzędne go nie widzi |
| **B — przeniesienie** (zalecany docelowo) | Przenieść katalog poza drzewo robocze repozytorium domowego, np. `C:/dev/rastaran` | Czystsze. Usuwa całą klasę pomyłek. Wymaga przestawienia ścieżek w narzędziach |

> Wariant A wystarcza do zamknięcia `BRAMKA-0`. Wariant B warto wykonać, zanim repozytorium
> urośnie — koszt rośnie z każdym skonfigurowanym narzędziem.

**Weryfikacja**

```bash
git -C "C:/Users/Igor/Desktop/Projekt/0_Rastaran" rev-parse --show-toplevel
```
Oczekiwane: ścieżka do `0_Rastaran`, **nie** `C:/Users/Igor`.

```bash
git -C "C:/Users/Igor" status --porcelain | grep -c "0_Rastaran"
```
Oczekiwane: `0`.

---

### `K-00.2` · Zdalne repozytorium i ochrona gałęzi

**Powstaje:** repozytorium zdalne (prywatne), gałąź domyślna `main`, reguła ochrony.

Reguła ochrony `main` — wymagane, bo `K-01` ma w Definicji Ukończenia „CI blokuje merge":

- wymagany przegląd: ≥ 1 zatwierdzenie
- wymagane zielone kontrole statusu: `quality`, `test`, `build`, `budget`, `security` (nazwy zadań z `K-01.9`)
- zakaz wypychania z pominięciem (`force push`) do `main`
- gałęzie robocze: `krok/K-01-repo`, `krok/K-02-rdzen`, … (konwencja `ROADMAP.md` §4.5)

**Weryfikacja:** próba wypchnięcia bezpośrednio do `main` odrzucona; próba scalenia przy czerwonym CI zablokowana.

---

### `K-00.3` · `DEC-020` — hosting, region, dostawca infrastruktury

**Powstaje:** `docs/adr/ADR-000-hosting-i-region.md`

**Kryteria oceny** (wynikają z dokumentacji, nie z preferencji):

| Kryterium | Źródło wymagania | Waga |
|---|---|---|
| Przetwarzanie danych osobowych **w UE** | `LEG-008`, art. 28 RODO | Blokujące |
| Umowa powierzenia dostępna od dostawcy | `DEC-006` | Blokujące |
| PostgreSQL 16 z częściowymi indeksami UNIQUE (`I6`) | `ROADMAP.md` §3.1 | Blokujące |
| Redis 7 (kolejki `BullMQ` + pub/sub + cache uprawnień) | `04` §9, granica `S3` | Blokujące |
| Opóźnienie do polskich lokali < 30 ms | Budżet `05` §7.1 („DNS + TLS + pierwszy bajt: 400 ms") | Wysoka |
| Koszt na 10 lokali (pilot) i na 100 (v1) | §7.3 koncepcji | Wysoka |
| Możliwość wystawienia CDN dla `apps/guest` i zdjęć menu | `04` §1, `05` §9 | Wysoka |
| Backup PITR bazy z dowiedzionym odtworzeniem | `L8` | Wysoka |

**Wyjście:** wybrany dostawca + region + zapis, gdzie leżą kopie zapasowe. Blokuje treść `DEC-006`.

> Ta decyzja należy do właściciela produktu. Plan jej nie przesądza — podaje wyłącznie kryteria.

---

### `K-00.4` · `DEC-010` — 10 wywiadów pogłębionych

**Powstaje:** `docs/research/DEC-010-wywiady.md`

- 5 właścicieli / managerów lokali, 5 kelnerów.
- Hipoteza do weryfikacji (zasada **Z2**): *„kelner jest beneficjentem systemu, nie jego ofiarą"*.
- **Kryterium falsyfikacji, ustalone przed pierwszym wywiadem:** jeśli ≥ 3 z 5 kelnerów wskaże,
  że system odbiera im napiwek, kontakt z gościem albo kontrolę — hipoteza jest obalona,
  a priorytet `K-11` (Kelner Pro) wymaga przeprojektowania, **nie przesunięcia terminu**.

> ⚠️ `DEC-010` **nie blokuje technicznie ani jednej linijki kodu**. Blokuje **sens** budowania
> powierzchni Kelner Pro w obecnym kształcie. Ryzyko „sabotaż personelu" jest w [`10`](../_docs_Nowa/10_Tuning_Decyzje_Ryzyka.md) §5
> oznaczone jako *wysokie prawdopodobieństwo / krytyczny skutek* — to jedyne ryzyko z tą kombinacją.

---

### `K-00` — Definicja ukończenia

- [ ] `git rev-parse --show-toplevel` w `0_Rastaran` zwraca ten katalog, nie `C:/Users/Igor`.
- [ ] Repozytorium nadrzędne nie widzi `0_Rastaran` (wariant A) albo katalog leży poza nim (wariant B).
- [ ] Zdalne repozytorium istnieje, `main` chroniona, lista wymaganych kontroli statusu ustawiona.
- [ ] `DEC-020` rozstrzygnięte i zapisane jako `ADR-000`.
- [ ] `DEC-010` — 10 wywiadów przeprowadzonych, wnioski zapisane, kryterium falsyfikacji rozstrzygnięte.
- [ ] `docs/decisions/BRAMKA-0.md` zawiera trzy pozycje z datami zamknięcia.

### `K-00` — Czego NIE robić

- Nie tworzyć jeszcze żadnej konfiguracji monorepo — to `K-01`.
- Nie kopiować `_docs_Nowa/` do innego miejsca. Jest źródłem prawdy tam, gdzie jest.
- Nie zamawiać infrastruktury produkcyjnej. `DEC-020` to **decyzja**, nie wdrożenie.

---

## 2. Ustalenia wiążące dla całej Fazy A

### 2.1. Stos i wersje

Wersje zweryfikowane 2026-08-16. Pin trafia do `package.json` / `.nvmrc` w `K-01.1`.

| Element | Pin | Uzasadnienie |
|---|---|---|
| Node.js | **24 LTS** (`.nvmrc` → `24`) | Wsparcie do 04.2028. Node 26 jest bieżący (do 04.2029) — przejście dopiero, gdy NestJS i Next zadeklarują wsparcie |
| pnpm | **11.21.x** (`packageManager` w root `package.json`) | 12.0 jest w RC. Start projektu nie jest miejscem na kandydata do wydania |
| Turborepo | **2.x** | Lżejsze od nx przy 5 aplikacjach i 6 pakietach (`ROADMAP.md` §3.2) |
| TypeScript | **5.x**, `strict: true` | `any` jest długiem `L2` — kompilator musi go wyłapać |
| NestJS | **11.1.x** | |
| Next.js | **16.3 LTS** · React **19** | |
| PostgreSQL | **16** | `ROADMAP.md` §3.1 — nie podlega renegocjacji |
| Redis | **7** | jw. |
| Tailwind CSS | **4.3** | Wyłącznie **konsumuje** zmienne CSS z [`05`](../_docs_Nowa/05_System_Projektowy.md) §2–4. Nie definiuje kolorów |
| Walidacja API | **zod** + `nestjs-zod` | Jedno źródło typu i schematu; kontrakt generowany, nie pisany ręcznie |
| Testy | **Vitest 4.1.x**, **Playwright 1.62**, **fast-check**, **Testcontainers** | `ROADMAP.md` §3.2 |
| Logi / telemetria | **pino** (JSON) + **OpenTelemetry** + **Sentry** | |
| Hasła | **argon2id** (`argon2` / `@node-rs/argon2`) | `ROADMAP.md` §3.2 |
| ORM | **`ADR-002`** — patrz §2.2 | ⚠️ patrz ostrzeżenie poniżej |

> ⚠️ **Ryzyko, którego `ROADMAP.md` nie zna.** Drizzle ORM v1 jest na dzień 2026-08-16
> w wersji **`1.0.0-beta.22`** (kwiecień 2026) — nadal beta. `DEC-016` rekomenduje Drizzle
> bez wiedzy o tym stanie. `ADR-002` musi rozstrzygnąć świadomie: linia stabilna `0.4x`,
> przyjęcie bety z zapisanym ryzykiem, albo Prisma. Patrz §2.2.

### 2.2. Zafiksowane decyzje techniczne

Zgodnie z `ROADMAP.md` §3.2: *„jeśli zespół wybierze inaczej, struktura kroków się nie zmienia"*.
Poniższe wybory przyjmujemy jako obowiązujące dla Fazy A; zmiana wymaga aktualizacji ADR,
nie przebudowy planu.

#### `DEC-016` · ORM i narzędzie migracji → **Drizzle ORM** (`K-01`, `ADR-002`)

| Kryterium z `ROADMAP.md` §3.3 | Drizzle | Prisma |
|---|---|---|
| Częściowe indeksy `UNIQUE` (wymóg `I6` w `K-06`) | Natywne w API schematu | Tylko przez `migration.sql` pisany ręcznie |
| `BIGINT` bez magii (wymóg `D3`) | `bigint({ mode: 'bigint' })` → `bigint` w TS | Domyślnie `BigInt`, ale warstwa klienta bywa nieprzezroczysta |
| Przygotowanie pod RLS (v2) | Pełna kontrola nad SQL sesji | Wymaga obejść |
| Kontrola nad filtrem `venue_id` w klasie bazowej (`I9`, `Z-A9`) | Budowanie zapytań jest kompozycyjne — `BaseRepository` da się domknąć | Klient generowany, trudniej wymusić filtr strukturalnie |
| Dojrzałość na 08.2026 | ⚠️ **v1 w becie** | Stabilna |

**Rozstrzygnięcie:** Drizzle, **z pinem na linię stabilną `0.4x`**, nie na `1.0.0-beta`.
Migracja na `1.x` po wydaniu stabilnym, jako osobne zadanie z własnym ADR.
Powód: kryterium „kontrola nad filtrem `venue_id`" jest w tym projekcie rozstrzygające
(`I9` jest niezmiennikiem, nie preferencją), a beta w fundamencie łamie zasadę „zero długów".

Migracje: **surowy SQL recenzowany ręcznie**, para `NNNN_nazwa.up.sql` / `NNNN_nazwa.down.sql`.
Powód: `L8` wymaga odpowiednika `down`, a `K-06` zakłada częściowy indeks UNIQUE, który na żywej
tabeli wymaga `CONCURRENTLY` i decyzji operatora.

#### `DEC-017` · Protokół realtime → **natywny WebSocket** (`K-03`, `ADR-003`)

| Kryterium | Natywny WS + własny klient | socket.io |
|---|---|---|
| Waga klienta u gościa | **~1–2 kB gz** | ~15–20 kB gz = **jedna trzecia budżetu 60 kB** |
| Ponowne łączenie, heartbeat | Do napisania (~150 linii) | Wbudowane |
| Zapasowy transport (long-polling) | Brak | Jest |
| Kontrola nad `sequenceNo` i autoryzacją subskrypcji | Pełna | Przez warstwę pośrednią |

**Rozstrzygnięcie:** natywny WebSocket. Kryterium rozstrzygające to budżet 60 kB JS
u gościa ([`05`](../_docs_Nowa/05_System_Projektowy.md) §7.3) — jest twarde i mierzone w CI.
Brak zapasowego transportu jest akceptowany: gość bez WebSocket dostaje pełne odświeżenie
stanu przez HTTP, a to i tak jest ścieżka po ponownym połączeniu ([`04`](../_docs_Nowa/04_Architektura_Moduly.md) §6.1).

#### `DEC-021` · Uwierzytelnianie personelu → **hasło + sesja, plus indywidualny PIN zmiany** (`K-04`, `ADR-004`)

| Wariant | Wada rozstrzygająca |
|---|---|
| Samo hasło | Kelner wpisuje je 20 razy na zmianę. `SCR-K-01` to „start zmiany", nie „logowanie" |
| Sam PIN na urządzeniu lokalu | Log potwierdzenia wieku wskazywałby **urządzenie**, nie osobę — traci wartość dowodową (`LEG-010`) |
| **Kombinacja** | Brak wady rozstrzygającej |

**Rozstrzygnięcie:**
- **Hasło (argon2id) + sesja w `httpOnly` cookie** — pierwsze uwierzytelnienie na urządzeniu,
  zmiana hasła, dostęp do panelu.
- **PIN zmiany, 6 cyfr, indywidualny per `StaffUser`, hasz argon2id** — szybkie przełączenie
  osoby w obrębie urządzenia lokalu.
- **PIN musi być indywidualny.** Wspólny PIN lokalu unieważnia log `LEG-010`.
- Ograniczenie prób: 5 na 15 minut per (`venue_id`, `staff_user_id`); po przekroczeniu
  wymagane pełne hasło.

### 2.3. Rejestr ADR Fazy A

`docs/adr/` — format: kontekst, decyzja, konsekwencje, warianty odrzucone. Krótko.

| ADR | Tytuł | Powstaje w | Utrwala |
|---|---|---|---|
| `ADR-000` | Hosting, region przetwarzania, dostawca infrastruktury | `K-00` | `DEC-020` |
| `ADR-001` | Monolit modularny zamiast mikroserwisów | `K-01` | [`04`](../_docs_Nowa/04_Architektura_Moduly.md) §1.1 |
| `ADR-002` | ORM i narzędzie migracji | `K-01` | `DEC-016` + ryzyko bety v1 |
| `ADR-003` | Protokół realtime | `K-03` | `DEC-017` |
| `ADR-004` | Model uwierzytelniania personelu | `K-04` | `DEC-021` + `LEG-010` |
| `ADR-005` | Tabele platformowe jako jawny wyjątek od `I9` | `K-04` | Rozbieżność §10.4 |

### 2.4. Konwencje

**Nazwy pakietów.** Zakres `@rastaran/`: `money`, `formats`, `design-tokens`, `ui`, `contracts`,
`realtime-client`. Aplikacje bez zakresu: `api`, `guest`, `waiter`, `kds`, `panel`.

**Skrypty w korzeniu** (jednolite, uruchamiane przez Turborepo):

| Skrypt | Co robi |
|---|---|
| `pnpm dev` | Podnosi `docker compose` + API + 4 aplikacje |
| `pnpm build` · `pnpm lint` · `pnpm typecheck` · `pnpm test` | Bramki CI, uruchamialne lokalnie |
| `pnpm test:prop` | Wyłącznie testy właściwościowe (`fast-check`) |
| `pnpm size` | Budżet wagi `apps/guest` |
| `pnpm db:generate` · `pnpm db:migrate` · `pnpm db:rollback` | **Osobne zadania.** Nigdy przy starcie aplikacji |

**Język.** Dokumentacja i copy UI — polski. Identyfikatory encji, zdarzeń, tabel, kolumn
i wartości enumeracji — angielski ([`00_INDEX.md`](../_docs_Nowa/00_INDEX.md) §5 jest wiążący).
Tabele: liczba mnoga, `snake_case`.

**Enumeracje w bazie:** `TEXT` z ograniczeniem `CHECK`. **Nie** typ `ENUM` PostgreSQL
(powód: `O4` — `Payment.method` musi być rozszerzalny bez migracji typu).

**Commity:** `feat:` `fix:` `refactor:` `test:` `docs:` `chore:` `perf:`, treść po angielsku,
ciało wyjaśnia **dlaczego**. Jeden commit = jedna logiczna zmiana. Nigdy `--no-verify`.
Gałąź na krok: `krok/K-01-repo`, `krok/K-02-rdzen`, `krok/K-03-szkielet`, `krok/K-04-identity`.

**Kanoniczne nazwy stanów** — `ROADMAP.md` §4.3. W Fazie A używane są w typach, nawet gdy
maszyna stanów powstaje później: `TableSession` (`reserved` · `open` · `active` · `billing` ·
`closed` · `abandoned` · `needs_attention`), `Order`, `OrderItem` (`awaiting_staff_confirmation`
— **nie skracać**), `Bill`, `Payment`, `TipPayout` (**nie istnieje** `pooled` ani `on_venue_account`).

---

## 3. `K-01` · Repozytorium, monorepo, CI/CD, środowiska

| | |
|---|---|
| **Wydanie** | — (fundament) |
| **Zależy od** | `BRAMKA-0` (`DEC-014`, `DEC-020`) |
| **Odblokowuje** | `K-02`, `K-03` |
| **Budżet lektury** | ~12 k tokenów |

**Cel.** Powstaje puste, ale kompletnie uzbrojone repozytorium: monorepo z pięcioma aplikacjami
i sześcioma pakietami, potok CI z bramkami jakości, trzy środowiska i szkielet dokumentacji
decyzji. **Zero kodu domenowego.**

**Wczytaj przed startem**
- [`00_INDEX.md`](../_docs_Nowa/00_INDEX.md) — całość
- [`04_Architektura_Moduly.md`](../_docs_Nowa/04_Architektura_Moduly.md) §1, §1.2
- [`05_System_Projektowy.md`](../_docs_Nowa/05_System_Projektowy.md) §7
- `ROADMAP.md` §3, §4, §5, §6

---

### `K-01.1` · Szkielet monorepo

**Powstaje**

```
package.json            pnpm-workspace.yaml      turbo.json
.nvmrc                  .npmrc                   .editorconfig
```

`package.json` (korzeń): `"private": true`, `"packageManager": "pnpm@11.21.0"`,
`"engines": { "node": ">=24 <25" }`, skrypty z §2.4 delegujące do `turbo run`.

`pnpm-workspace.yaml`:
```yaml
packages:
  - "apps/*"
  - "packages/*"
```

`turbo.json` — zadania `build` (`dependsOn: ["^build"]`, `outputs: ["dist/**", ".next/**"]`),
`lint`, `typecheck`, `test`, `size` (`dependsOn: ["build"]`).

`.npmrc`: `strict-peer-dependencies=true`, `auto-install-peers=false`, `shamefully-hoist=false`.
Powód: `apps/guest` ma zakaz importu czegokolwiek, co nie przeszło budżetu — hoisting ukrywa,
skąd pochodzi zależność.

**Weryfikacja**
```bash
pnpm install && pnpm -r list --depth -1
```
Oczekiwane: 11 pakietów roboczych (5 aplikacji + 6 pakietów) + korzeń.

---

### `K-01.2` · Konfiguracja TypeScript

**Powstaje:** `tsconfig.base.json` (korzeń) + `tsconfig.json` w każdym pakiecie roboczym (`extends`).

Ustawienia obowiązkowe w `tsconfig.base.json`:

| Opcja | Wartość | Dlaczego |
|---|---|---|
| `strict` | `true` | `ROADMAP.md` §3.2 |
| `noUncheckedIndexedAccess` | `true` | jw. |
| `exactOptionalPropertyTypes` | `true` | jw. |
| `noImplicitOverride` | `true` | `BaseRepository` w `K-03` jest dziedziczony przez każde repozytorium |
| `noFallthroughCasesInSwitch` | `true` | Maszyny stanów są typami sumarycznymi |
| `useUnknownInCatchVariables` | `true` | `any` jest długiem `L2` |
| `verbatimModuleSyntax`, `isolatedModules` | `true` | Spójność ESM w pakietach i aplikacjach |
| `target` / `lib` | `ES2023` | `bigint` jest w rdzeniu typu `Money` |

`apps/api`: `"module": "NodeNext"`. Aplikacje Next: `"moduleResolution": "bundler"`.

**Weryfikacja:** `pnpm typecheck` → 0 błędów na czystym klonie.

---

### `K-01.3` · `apps/api` — pusty NestJS z kontrolą stanu

**Powstaje**

```
apps/api/
├── package.json  nest-cli.json  tsconfig.json  tsconfig.build.json
└── src/
    ├── main.ts                    # bootstrap, helmet, globalny potok walidacji (nestjs-zod)
    ├── app.module.ts
    ├── config/env.ts              # schemat zod dla zmiennych środowiskowych, fail-fast przy starcie
    ├── health/health.controller.ts
    └── modules/.gitkeep           # katalog na moduły — PUSTY w K-01
```

`health.controller.ts` — dwa punkty, celowo różne:

| Ścieżka | Znaczenie | Zależności |
|---|---|---|
| `GET /healthz` | Żywotność procesu | **Żadne.** Zawsze 200, jeśli proces żyje |
| `GET /readyz` | Gotowość do przyjmowania ruchu | `SELECT 1` na PG + `PING` na Redis. 503, gdy któreś nie odpowiada |

Rozdzielenie jest istotne: `K-04` wprowadza **fail-closed** przy braku Redisa (granica `S3`).
Instancja bez Redisa **żyje**, ale **nie jest gotowa** — orkiestrator musi to rozróżniać.

`config/env.ts` — walidacja zod przy starcie, proces kończy się z kodem ≠ 0 przy braku zmiennej.
Brak `process.env.X ?? 'domyślna'` rozsianego po kodzie.

**Weryfikacja**
```bash
pnpm --filter api dev
curl -s localhost:3000/healthz    # {"status":"ok"}
curl -s -o /dev/null -w "%{http_code}" localhost:3000/readyz   # 200
docker compose stop redis && curl -s -o /dev/null -w "%{http_code}" localhost:3000/readyz   # 503
```

---

### `K-01.4` · `apps/guest` — linia bazowa budżetu wagi

> **To jest najważniejsze zadanie w `K-01`.** Jeśli pusta aplikacja gościa nie mieści się
> w budżecie, jest to problem `K-01`, a nie `K-10` — i rozwiązuje się go **zanim** ktokolwiek
> doda pierwszą bibliotekę.

**Powstaje**

```
apps/guest/
├── package.json  next.config.ts  tsconfig.json  .size-limit.json
└── src/app/
    ├── layout.tsx    # <html lang="pl">, ZERO next/font, krytyczny CSS wbudowany
    └── page.tsx      # statyczna, serwerowa; zero komponentów klienckich
```

Budżety z [`05`](../_docs_Nowa/05_System_Projektowy.md) §7.3 wpisane do `.size-limit.json`:

| Zasób | Limit |
|---|---|
| HTML pierwszej odpowiedzi (po kompresji) | ≤ **14 kB** |
| Krytyczny CSS wbudowany | ≤ **8 kB** |
| JavaScript początkowy (gzip) | ≤ **60 kB** |
| Obrazy pierwszego ekranu | ≤ **120 kB** |
| **Całość pierwszego widoku** | ≤ **200 kB** |
| Pobrane kroje pisma | **0 kB** |

**Zadanie pomiarowe:** zbudować, zmierzyć, zapisać wynik do `docs/budgets/baseline-guest.md`
razem z datą, wersją Next.js i rozbiciem na porcje.

> ⚠️ **Punkt decyzyjny.** Jeśli pusty `apps/guest` przekracza 60 kB JS, w `K-01` zapada decyzja:
> (a) Next.js zostaje z twardym ograniczeniem „zero komponentów klienckich na pierwszym ekranie",
> (b) `apps/guest` schodzi na lżejsze rozwiązanie. Decyzja idzie do ADR. Przełożenie jej
> na `K-10` oznacza przepisanie aplikacji gościa.

**Weryfikacja**
```bash
pnpm --filter guest build && pnpm --filter guest size
```

---

### `K-01.5` · `apps/waiter`, `apps/kds`, `apps/panel`

| Aplikacja | Renderowanie | Konfiguracja w `K-01` |
|---|---|---|
| `waiter` | PWA, offline-first do odczytu (logika w `K-11`) | Manifest, ikony, zarejestrowany pusty Service Worker |
| `kds` | SPA, sesja 14 h | Bez SSR; wersja aplikacji wystawiona w `window` — brama realtime jej użyje w `K-03` |
| `panel` | SPA, bez limitu wagi | Może importować ciężkie biblioteki |

> **Cztery aplikacje nie dzielą konfiguracji Tailwinda ani listy zależności.**
> Wspólne są wyłącznie `@rastaran/design-tokens` i `@rastaran/ui`.
> Każda aplikacja ma własny `tailwind.config` i własne wejście CSS.

**Weryfikacja:** `pnpm build` buduje cztery aplikacje; `apps/guest/package.json` nie ma
zależności spoza listy zatwierdzonej budżetem.

---

### `K-01.6` · Sześć pustych pakietów

**Powstaje:** `packages/{money,formats,design-tokens,ui,contracts,realtime-client}/`

Każdy pakiet:
```jsonc
{
  "name": "@rastaran/money",
  "type": "module",
  "sideEffects": false,
  "exports": { ".": { "types": "./dist/index.d.ts", "import": "./dist/index.js" } },
  "scripts": { "build": "tsc -p tsconfig.json", "test": "vitest run" }
}
```
plus `tsconfig.json` (`extends` bazowego, `composite: true`) i `src/index.ts` z pustym eksportem.

`"sideEffects": false` jest wymogiem budżetu — bez tego bundler nie usunie nieużywanego kodu
z `@rastaran/ui` w `apps/guest`.

**Weryfikacja:** `pnpm build` generuje `dist/` w każdym pakiecie; import `@rastaran/money`
z `apps/api` rozwiązuje się i typuje.

---

### `K-01.7` · Środowisko lokalne — `docker-compose.yml`

**Powstaje:** `docker-compose.yml`, `.env.example`

| Usługa | Obraz | Uwagi |
|---|---|---|
| `postgres` | `postgres:16-alpine` | Wolumen nazwany, kontrola stanu `pg_isready`, `POSTGRES_INITDB_ARGS="--locale=pl_PL.UTF-8"` |
| `redis` | `redis:7-alpine` | `--appendonly yes`, kontrola stanu `redis-cli ping` |
| storage plików | zależne od `ADR-000` | Domyślnie **wolumen powiązany** `./storage` — najmniej ruchomych części. Jeśli `DEC-020` wybierze magazyn obiektowy, dochodzi `minio` dla zgodności ze środowiskiem produkcyjnym |

> Storage w `K-01` to **katalog**, nie interfejs. Port `FileStorage` powstaje w `K-05`,
> razem z pierwszym rzeczywistym plikiem (zdjęcie pozycji menu).

**Weryfikacja**
```bash
docker compose up -d && docker compose ps
pnpm dev
```
Oczekiwane: usługi w stanie `healthy`, `pnpm dev` podnosi API + 4 aplikacje jedną komendą.

---

### `K-01.8` · Lint, formatowanie, reguły architektoniczne

**Powstaje:** `eslint.config.js` (płaska konfiguracja ESLint 9), `.prettierrc`,
`stylelint.config.js`, `.dependency-cruiser.cjs`

| Reguła | Konfiguracja | Egzekwuje |
|---|---|---|
| `@typescript-eslint/no-explicit-any` | `error` | `L2`, `ROADMAP.md` §6 |
| `@typescript-eslint/ban-ts-comment` | `error`, bez wyjątków dla `@ts-ignore` | `L2` |
| `no-restricted-syntax` na `as unknown as` | `error` | `L2` |
| `import/order` + `no-restricted-imports` | Szkielet; reguły modułowe dochodzą w `K-03` | `Z-A1` |
| stylelint: `outline: none` | Zakaz | WCAG 2.4.7, `LEG-011` |
| stylelint: surowe wartości kolorów | Zakaz poza `packages/design-tokens` | `ROADMAP.md` §6 |
| `dependency-cruiser`: `no-circular`, `no-orphans` | `error` | Higiena; reguła `index.ts` w `K-03` |

**Weryfikacja:** `pnpm lint` → 0. Dopisanie `const x: any = 1` do dowolnego pliku → błąd lintera.

---

### `K-01.9` · Potok CI

**Powstaje:** `.github/workflows/ci.yml`, `.github/workflows/deploy-staging.yml`,
`.github/workflows/migrate.yml`, `lighthouserc.json`

Zadania w `ci.yml` (nazwy = wymagane kontrole statusu z `K-00.2`):

| Zadanie | Zawartość | Próg |
|---|---|---|
| `quality` | `pnpm typecheck`, `pnpm lint`, `pnpm format:check` | 0 błędów |
| `test` | `pnpm test` (Vitest + Testcontainers) | Progi pokrycia — patrz niżej |
| `build` | `pnpm build` | Sukces |
| `budget` | `pnpm size` + Lighthouse CI, profil mobilny, dławienie 3G | Wagi i czasy z `K-01.4` |
| `security` | `gitleaks`, `osv-scanner`, `pnpm audit --audit-level=high` | 0 znalezisk krytycznych i wysokich |

Progi pokrycia (`ROADMAP.md` §6), konfigurowane w `K-01` i egzekwowane od pierwszego kodu:

| Obszar | Próg |
|---|---|
| `packages/money`, `MOD-billing`, `MOD-payments`, `MOD-tips`, `MOD-fiscal` | **≥ 90%** |
| Pozostałe moduły domenowe | **≥ 80%** |
| Reszta | **≥ 60%** |

`lighthouserc.json`: `FCP ≤ 1000 ms`, `LCP ≤ 2000 ms`, `TTI ≤ 2500 ms`. Asercje jako `error`,
nie `warn` — czerwona bramka to niezaliczona kompilacja.

> Na pustych aplikacjach progi są spełniane trywialnie. **O to chodzi:** progi mają być
> obecne, zanim zaczną boleć, żeby moment ich przekroczenia był widoczny w konkretnym commicie.

**Weryfikacja:** patrz `K-01.12`.

---

### `K-01.10` · Środowiska i migracje

**Powstaje:** `docs/runbooks/environments.md`, skrypty `db:*`, `.github/workflows/migrate.yml`

| Środowisko | Uruchamianie | Migracje |
|---|---|---|
| Lokalne | `docker compose` + `pnpm dev` | `pnpm db:migrate` ręcznie |
| Staging | Wdrożenie z potoku po scaleniu do `main` | Osobne zadanie, `workflow_dispatch` |
| Produkcja | Wdrożenie z potoku, zatwierdzane ręcznie | Osobne zadanie. **Forward-only** |

> ⚠️ **Migracje nie startują z aplikacją.** Ani w `main.ts`, ani w punkcie wejścia kontenera.
> Powód: `K-06` wprowadza częściowy indeks UNIQUE (`I6`), którego zakładanie na żywej tabeli
> wymaga `CONCURRENTLY` i świadomej decyzji operatora.

Każda migracja ma parę `NNNN_nazwa.up.sql` / `NNNN_nazwa.down.sql`. Test migracji na kopii
schematu jest bramką CI (`L8`).

**Weryfikacja:** start API przy pustej bazie **nie** tworzy tabel. `pnpm db:migrate` je tworzy.
`pnpm db:rollback` je usuwa bez utraty danych w tabelach nietkniętych migracją.

---

### `K-01.11` · Dokumentacja decyzji

**Powstaje**
- `docs/adr/TEMPLATE.md`
- `docs/adr/ADR-001-monolit-modularny.md` — utrwala [`04`](../_docs_Nowa/04_Architektura_Moduly.md) §1.1:
  cel pilotu to 10 lokali (skala nie wymusza rozdzielenia procesów), fiskalizacja musi być
  synchroniczna w granicy < 5 s (`LEG-003`), zespół jest mały. Kandydaci do wydzielenia w v3:
  `MOD-analytics`, `MOD-crm`, `MOD-pos-sync`.
- `docs/adr/ADR-002-orm-i-migracje.md` — `DEC-016` wg §2.2, z jawnym zapisem ryzyka bety v1.
- `docs/runbooks/` — katalog, na razie z `environments.md`.

> ⚠️ **Rozbieżność.** Definicja Ukończenia `K-01` w `ROADMAP.md` żąda, by `DEC-016` było
> „dopisane do [`10`](../_docs_Nowa/10_Tuning_Decyzje_Ryzyka.md) §3.2", podczas gdy `ROADMAP.md` §4.1
> opisuje `_docs_Nowa/` jako „ŹRÓDŁO PRAWDY, **nie edytować w krokach**". Patrz §10.1.

---

### `K-01.12` · Weryfikacja bramek celowo zepsutym commitem

**Powstaje:** `docs/runbooks/ci-gates-verification.md` z dowodami (odnośniki do przebiegów CI).

Trzy osobne próby, każda na własnej gałęzi, każda odrzucona przez **inną** bramkę:

| Próba | Oczekiwana czerwona bramka |
|---|---|
| `const x: any = 1` w `apps/api/src/main.ts` | `quality` — `no-explicit-any` |
| Klucz API w postaci jawnej w pliku źródłowym | `security` — `gitleaks` |
| Import ciężkiej biblioteki do `apps/guest/src/app/page.tsx` | `budget` — `size-limit` |

Dodatkowo: próba scalenia gałęzi z czerwonym CI do `main` musi być zablokowana przez regułę
ochrony z `K-00.2`.

---

### `K-01` — Definicja ukończenia

- [ ] `pnpm install && pnpm dev` uruchamia komplet: API + 4 aplikacje + PostgreSQL + Redis.
- [ ] `pnpm build`, `pnpm lint`, `pnpm typecheck`, `pnpm test` przechodzą na **czystym klonie**.
- [ ] CI blokuje scalenie przy czerwonej bramce — zweryfikowane trzema celowo zepsutymi commitami.
- [ ] Budżet wagi `apps/guest` zmierzony i zapisany jako linia bazowa (`docs/budgets/baseline-guest.md`).
- [ ] `gitleaks` i skan zależności w potoku, 0 znalezisk.
- [ ] Wdrożenie na staging działa **z potoku, nie z laptopa**.
- [ ] Migracje uruchamiane osobnym zadaniem; start aplikacji ich nie wywołuje — test.
- [ ] `DEC-016` rozstrzygnięte, zapisane jako `ADR-002`.
- [ ] `ADR-001` napisany.
- [ ] Globalna DoD (`ROADMAP.md` §5) w części dotyczącej tego kroku.

### `K-01` — Czego NIE robić

- Żadnych encji, migracji domenowych, punktów końcowych biznesowych.
- Żadnego uwierzytelniania — to `K-04`.
- **Nie instalować bibliotek UI „na zapas".** Każda zależność w `apps/guest` musi mieć uzasadnienie w budżecie.
- Nie konfigurować RLS w PostgreSQL — to druga warstwa izolacji, planowana na v2.
  Pierwsza warstwa (`K-03`) musi działać sama.

### `K-01` — Ryzyka

| Ryzyko | Przeciwdziałanie |
|---|---|
| Pusty `apps/guest` nie mieści się w 60 kB JS | Punkt decyzyjny w `K-01.4`, rozstrzygany **w tym kroku** |
| Testcontainers na Windows bywa wolny | Ustalić, że `pnpm test` w CI działa na Linuksie; lokalnie dopuścić `docker compose` jako alternatywę |
| Beta Drizzle wymusza zmianę linii wersji w trakcie fazy | Pin na `0.4x` (§2.2); migracja na `1.x` jako osobne zadanie z ADR |

---

## 4. `K-02` · Rdzeń wspólny: `Money`, formaty polskie, tokeny projektowe, biblioteka UI

| | |
|---|---|
| **Wydanie** | — (fundament) |
| **Zależy od** | `K-01` |
| **Odblokowuje** | `K-03` i każdy krok dotykający kwot lub interfejsu |
| **Budżet lektury** | ~18 k tokenów |

**Cel.** Powstaje warstwa, której **nie da się później podmienić**: typ pieniężny, formaty
polskie, tokeny projektowe spełniające WCAG 2.1 AA w obu motywach i komponenty wspólne.

**Wczytaj przed startem**
- [`00_INDEX.md`](../_docs_Nowa/00_INDEX.md) — całość
- [`03_Model_Domenowy.md`](../_docs_Nowa/03_Model_Domenowy.md) §6 **w całości**, §7 (`RULE-001`–`RULE-005`), §8 (`I1`–`I4`)
- [`05_System_Projektowy.md`](../_docs_Nowa/05_System_Projektowy.md) **w całości** — dokument źródłowy tego kroku
- `ROADMAP.md` §4.4, §6

**Reguły obowiązkowe:** `RULE-001` · `RULE-002` · `RULE-003` · `RULE-027` · `I1`–`I4` jako testy właściwościowe.

---

### `K-02.1` · `packages/money` — typ i arytmetyka

**Powstaje:** `packages/money/src/{money.ts,rounding.ts,index.ts}`

Kontrakt publiczny — **kompletny**, nie ma innego wyjścia z typu:

```ts
declare const brand: unique symbol
export type Money = bigint & { readonly [brand]: 'Money' }   // grosze
export type Currency = 'PLN'          // RULE-027; typ dopuszcza rozszerzenie (O10)
export type VatRateBps = number       // punkty bazowe: 800 = 8,00% · 2300 = 23,00%

export const Money: {
  // konstrukcja
  fromGrosze(g: bigint): Money
  fromJSON(v: { amount: string; currency: Currency }): Money
  zero(): Money

  // arytmetyka
  add(a: Money, b: Money): Money
  sub(a: Money, b: Money): Money
  mulQty(a: Money, qty: number): Money          // qty: dodatnia liczba całkowita
  percent(a: Money, bps: number): Money         // HALF_UP w stronę od zera
  negate(a: Money): Money
  abs(a: Money): Money

  // domena
  allocate(a: Money, n: number, initiatorIndex?: number): Money[]   // RULE-002, I2
  vatFromGross(gross: Money, rate: VatRateBps): Money               // RULE-003
  netFromGross(gross: Money, rate: VatRateBps): Money

  // porównania
  compare(a: Money, b: Money): -1 | 0 | 1
  isNegative(a: Money): boolean

  // wyjście — TYLKO te trzy drogi
  toGrosze(a: Money): bigint
  toJSON(a: Money, currency: Currency): { amount: string; currency: Currency }
  format(a: Money, locale: Locale): string      // delegacja do @rastaran/formats
}
```

> **`Money` nie ma metody `toNumber()`.** Jeśli ktoś jej potrzebuje, robi coś źle.

Zaokrąglenie — **HALF_UP w stronę od zera**, jak `BigDecimal.ROUND_HALF_UP`.
Ma znaczenie, bo `Modifier.price_delta_gross` **może być ujemny** ([`03`](../_docs_Nowa/03_Model_Domenowy.md) §3.2):

```ts
// packages/money/src/rounding.ts
export function divHalfUpAwayFromZero(num: bigint, den: bigint): bigint {
  if (den === 0n) throw new RangeError('division by zero')
  const negative = (num < 0n) !== (den < 0n)
  const n = num < 0n ? -num : num
  const d = den < 0n ? -den : den
  const q = n / d
  const r = n % d
  const rounded = r * 2n >= d ? q + 1n : q
  return negative ? -rounded : rounded
}
```

**Weryfikacja:** `pnpm --filter @rastaran/money test` — pokrycie ≥ 95%.
Test jawny: `divHalfUpAwayFromZero(-5n, 2n) === -3n` (nie `-2n`).

---

### `K-02.2` · `packages/money` — VAT i podział kwoty

**VAT liczony wstecz od brutto** (ceny w menu są brutto, [`03`](../_docs_Nowa/03_Model_Domenowy.md) §6.3),
mnożenie **przed** dzieleniem, wyłącznie na `bigint`:

```
vat = round_half_up( gross × rate / (10000 + rate) )
net = gross − vat
```

Stawki jako liczby całkowite w punktach bazowych — `800`, `2300`. Powód: `O10` wymaga stawek
jako **danych**, a rynki CZ/SK/RO mają stawki ułamkowe. Stawka **jest atrybutem pozycji**,
nie rachunku (`RULE-003`) — alkohol 23%, żywność 8%, service charge 8%, napiwek **bez VAT** (`RULE-004`).

**Podział kwoty** (`RULE-002`, `RULE-017`, `I2`):

```ts
allocate(total, n, initiatorIndex = 0):
  base = total / n                 // dzielenie całkowite ku zeru
  rem  = total - base * n          // reszta niesie znak total
  out  = Array(n).fill(base)
  out[initiatorIndex] += rem
  return out
```

> ⚠️ **Uwaga do przykładu z dokumentacji.** `ROADMAP.md` i [`03`](../_docs_Nowa/03_Model_Domenowy.md) §6.2
> podają `100,00 zł` na 3 jako `33,33 + 33,33 + 33,34`. Kolejność w tym zapisie jest ilustracyjna.
> Wiążące są dwa warunki: **suma udziałów równa się kwocie dzielonej co do grosza** oraz
> **reszta trafia do uczestnika inicjującego podział**. Test asercjonuje oba warunki,
> nie kolejność elementów tablicy.

**Weryfikacja** — testy jawne wymagane przez `ROADMAP.md`:
- `allocate(10000n, 3)` → suma `10000n`, `out[0] === 3334n`
- `vatFromGross(12300n, 2300)` → `2300n` (brutto 123,00 zł przy 23% → VAT 23,00 zł)
- `vatFromGross(10800n, 800)` → `800n`
- wartości graniczne: `1n`, `0n`, kwoty ujemne, `n = 1`

---

### `K-02.3` · `packages/money` — testy właściwościowe `I1`–`I4`

**Powstaje:** `packages/money/tests/invariants.property.test.ts` (`fast-check`)

`ROADMAP.md` §6 czyni je bramką CI bez wyjątków. `ROADMAP.md` §9.6 przypisuje pełne
niezmienniki domenowe do kroków `K-09`/`K-16`/`K-17`/`K-23`. W `K-02` powstaje ich
**fundament arytmetyczny** — właściwości, bez których niezmiennik domenowy nie ma szans zajść:

| Niezmiennik | Właściwość testowana w `K-02` | Dowodzony w pełni |
|---|---|---|
| `I1` — `Bill.total = Σ lines + service_charge` | Suma dowolnej listy `Money` nie zależy od kolejności i nie gubi grosza | `K-09` |
| `I2` — `Σ SplitShare = Bill.total` | `Σ allocate(a, n, i) === a` dla **dowolnego** `a` (w tym ujemnego i zerowego) i `n ≥ 1`; `out[i] === base + rem` | `K-23` |
| `I3` — `Σ captured ≤ Bill.total` | `compare` jest totalne, antysymetryczne i przechodnie; akumulacja nie przepełnia | `K-16`, `K-23` |
| `I4` — `Tip` nigdy nie wchodzi do `Bill.total` | Sumowanie dwóch rozłącznych kolekcji nie zmienia sumy żadnej z nich; `Money` nie miesza kontekstów | `K-17` |

Generatory: `fc.bigInt({ min: -10n ** 12n, max: 10n ** 12n })` dla kwot,
`fc.integer({ min: 1, max: 64 })` dla `n`, `fc.constantFrom(800, 2300, 0, 500)` dla stawek.

> **Test przykładowy przepuści tu błąd.** Podział `100,00 zł` na 3 działa w każdej naiwnej
> implementacji. Podział `−1 gr` na 7 — nie.

**Weryfikacja:** `pnpm test:prop` → wszystkie właściwości przechodzą, `numRuns` ≥ 1000.

---

### `K-02.4` · Reguła ESLint blokująca arytmetykę zmiennoprzecinkową na kwotach

**Powstaje:** `packages/eslint-plugin-money/` (pakiet roboczy, nie publikowany)
+ `packages/eslint-plugin-money/tests/no-float-money.test.ts` (`RuleTester`)

Reguła `@rastaran/money/no-float-money` — zakres wg `ROADMAP.md` §6:

| Wykrywa | Na identyfikatorach pasujących do |
|---|---|
| `parseFloat(…)` | — (zawsze) |
| `Number(…)` na wyrażeniu pieniężnym | `*price*`, `*amount*`, `*total*`, `*vat*`, `*tip*`, `*cost*`, `*margin*`, `*gross*` |
| `.toFixed(…)` jako mechanizm zaokrąglania | jw. |
| Arytmetyka `+ - * /` z operandem typu `number` | jw. |
| Literał zmiennoprzecinkowy przypisany do takiego identyfikatora | jw. |

Wyjątki: `packages/formats` (granica renderu) i `packages/money` (implementacja) — jawnie
wyłączone w konfiguracji, nie przez komentarz w kodzie.

**Weryfikacja:** `RuleTester` z zestawem `valid`/`invalid`; dodatkowo test integracyjny —
plik `apps/api/src/__lint-fixture__.ts` z `const totalPrice = parseFloat(x)` psuje `pnpm lint`.

---

### `K-02.5` · `packages/formats` — jedenaście formatów polskich

**Powstaje:** `packages/formats/src/{money,date,time,duration,table,phone,nip,percent,locale}.ts`

| Funkcja | Wyjście | Pułapka implementacyjna |
|---|---|---|
| `formatMoney` | `123,45 zł` | **Spacja nierozdzielająca** (U+00A0) przed `zł`. `zł` małą literą |
| `formatMoneyGrouped` | `1 234,56 zł` | Separator tysięcy to też **U+00A0**, nie zwykła spacja |
| `formatDate` | `16.08.2026` | `dd.MM.yyyy` |
| `formatDateDescriptive` | `16 sierpnia` | **Dopełniacz.** `Intl` dla `pl` bywa niespójny — własna tablica miesięcy |
| `formatTime` | `20:30` | 24-godzinny, dwucyfrowy |
| `formatDateTime` | `16.08 · 20:30` | Separator **U+00B7** ze spacjami. **Bez roku** |
| `formatDuration` | `45 min` / `2 h 15 min` | Próg przełączenia: **90 minut** |
| `formatTableNumber` | `Stolik 12` | **Bez znaku `#`** |
| `formatPhone` | `+48 573 568 812` | Grupy po trzy |
| `formatNip` | `123-456-78-90` | Dywiz, grupowanie `3-3-2-2` |
| `formatPercent` | `10%` | **Bez spacji** przed znakiem |

Lokalizacje: `pl` (podstawowa), `uk`, `en`, `de`.
**Interfejs ukraiński używa polskiego formatu waluty** — gość płaci w złotych.

Dodatkowo: `formatMoney` i `formatMoneyGrouped` wystawiają klasę/właściwość wymuszającą
`font-variant-numeric: tabular-nums` ([`05`](../_docs_Nowa/05_System_Projektowy.md) §3.3 — kwoty zawsze tabelarycznie).

**Weryfikacja:** testy migawkowe dla każdego formatu × 4 lokalizacje; asercje na **kody znaków**
(` `, `·`), nie na wygląd — zwykła spacja przechodzi wizualnie i psuje łamanie wiersza.

---

### `K-02.6` · `packages/design-tokens` — źródło tokenów w TS

**Powstaje:** `packages/design-tokens/src/{colors,typography,space,motion,index}.ts`

Źródłem prawdy jest **TypeScript**; CSS jest generowany (`K-02.7`). Odwrotny kierunek
uniemożliwia test kontrastu i eksport typów.

Zawartość wg [`05`](../_docs_Nowa/05_System_Projektowy.md):

| Grupa | Liczba | Sekcja źródłowa |
|---|---|---|
| Kolory — motyw jasny | 30 tokenów (`--color-primary-*`, `--color-accent-*`, neutralne, statusy) | §2.2 |
| Kolory — motyw ciemny | 22 nadpisania pod `[data-theme="dark"]` | §2.3 |
| Typografia — skala gość/panel | `--text-xs` … `--text-3xl` (7 pozycji, rozmiar + interlinia) | §3.2 |
| Typografia — skala KDS | `--kds-text-sm` 20 px · `-base` 26 px · `-lg` 34 px · `-xl` 48 px | §3.2 |
| Kroje | `--font-sans` (stos systemowy), `--font-mono` | §3.1 |
| Odstępy | `--space-1,2,3,4,5,6,8,10,12,16` (skala 4 px) | §4 |
| Promienie | `--radius-sm/md/lg/full` | §4 |
| Cienie | `--shadow-sm/md/lg` | §4 |
| Cel dotykowy | `--touch-min: 48px` | §4 |
| Ruch | `--motion-fast/base/slow`, `--ease-out` | §5 |

**Uzupełnienia wobec `05`** — dokumentacja ich nie zawiera, a kod ich potrzebuje.
Każde wymaga jawnej decyzji i wpisu w §10:

| # | Luka | Propozycja |
|---|---|---|
| 1 | Motyw ciemny nie nadpisuje `--color-primary-50/100/700/900` i `--color-accent-50/100/500/700` | Albo uzupełnić wartości ciemne, albo oznaczyć te tokeny jako „wyłącznie powierzchnie jasne" i zablokować lintem |
| 2 | Skala `--kds-text-*` nie ma interlinii | Dodać, wyliczone z odległości obserwacji (KDS czyta się z 2 m) |
| 3 | Kelner Pro nie ma własnych tokenów | To **reguła użycia** (`--text-sm` jako baza), nie nowa skala. Zapisać jako warstwę aliasów semantycznych |
| 4 | `--font-sans` / `--font-mono` nie są w bloku `:root` w §3.1 | Wciągnąć do pakietu |
| 5 | Brak tokenów wagi kroju | Dodać minimalny zestaw (`--font-weight-normal/medium/semibold`) |
| 6 | `--color-info` / `--color-info-bg` bez przypisanej roli | Przypisać rolę albo usunąć — jedyne tokeny statusu bez zastosowania |

**Weryfikacja:** `pnpm --filter @rastaran/design-tokens build` generuje typy;
brak tokenu użytego w `packages/ui` = błąd kompilacji, nie cicha `undefined`.

---

### `K-02.7` · `packages/design-tokens` — generowanie CSS i test kontrastu

**Powstaje:** `packages/design-tokens/build/generate-css.ts` → `dist/tokens.css`,
`packages/design-tokens/src/contrast.ts`, `packages/design-tokens/tests/contrast.test.ts`

`dist/tokens.css`: blok `:root` (motyw jasny) + blok `[data-theme="dark"]` (nadpisania).
Mechanizm to **atrybut na elemencie**, nie `prefers-color-scheme` — bo reguły trybu z [`05`](../_docs_Nowa/05_System_Projektowy.md) §2.3
wymagają wymuszenia (Kelner Pro i KDS **zawsze ciemne**, gość ciemny po zmroku wg godzin
serwisowych lokalu, panel domyślnie jasny).

Test kontrastu — macierz par musi być **zdefiniowana**, bo `ROADMAP.md` mówi tylko
„wszystkie pary tokenów". Propozycja zakresu:

| Grupa par | Próg | Uzasadnienie |
|---|---|---|
| `--color-text`, `-muted`, `-subtle` × `--color-surface`, `-sunken`, `-raised` | ≥ 4,5:1 | WCAG 1.4.3, tekst |
| `--color-text-inverse` × `--color-primary-600`, `-accent-600`, `-success`, `-warning`, `-danger`, `-info` | ≥ 4,5:1 | Tekst na przyciskach i znacznikach |
| `--color-success`, `-warning`, `-danger`, `-info` × odpowiadające `-bg` | ≥ 4,5:1 | Tekst statusu na tle statusu |
| `--color-border-strong` × `--color-surface`, `-sunken`, `-raised` | ≥ 3:1 | WCAG 1.4.11, elementy |
| Obrys fokusu × każde tło | ≥ 3:1 | WCAG 2.4.7 |

**W obu motywach.** Wynik testu to tabela w raporcie CI, nie samo „przeszło".

> ⚠️ Jeśli któraś para nie przechodzi, **nie wolno po cichu zmienić wartości z `05`**.
> `05` jest źródłem prawdy — rozbieżność idzie do §10 i do właściciela dokumentacji.

**Weryfikacja:** `pnpm --filter @rastaran/design-tokens test` — 0 par poniżej progu.

---

### `K-02.8` · `packages/ui` — dwadzieścia cztery komponenty wspólne

**Powstaje:** `packages/ui/src/<Nazwa>/` — komponent + testy + historia stanów.

Lista z [`05`](../_docs_Nowa/05_System_Projektowy.md) §8.1, w trzech partiach:

| Partia | Komponenty |
|---|---|
| **A · formularz** | `Button`, `IconButton`, `Input`, `Textarea`, `Select`, `Checkbox`, `Radio`, `Switch` |
| **B · prezentacja** | `Badge`, `Chip`, `Card`, `Avatar`, `Money`, `Timer`, `Skeleton`, `Spinner` |
| **C · struktura i stan** | `Sheet` (panel dolny), `Dialog`, `Toast`, `Tabs`, `EmptyState`, `ErrorState`, `LanguageSwitcher`, `ThemeToggle` |

**Każdy komponent ma sześć stanów:** domyślny · hover · focus (**widoczny obrys 2 px**) ·
aktywny · wyłączony · ładowanie.

Ograniczenia strukturalne — wyrażone w **typach**, nie w komentarzu:

| Ograniczenie | Realizacja |
|---|---|
| `--touch-min: 48px` to nie sugestia | `Button` nie ma wariantu „small". Wysokość minimalna wpisana w komponent, nie w klasę użytkownika |
| Kolory statusu są zarezerwowane | `Button` przyjmuje `variant: 'primary' \| 'accent' \| 'secondary' \| 'ghost' \| 'danger'`. **`success` nie istnieje jako wariant przycisku** |
| Status kodowany trzema środkami naraz (WCAG 1.4.1) | Komponent statusu KDS wymaga jednocześnie koloru, licznika liczbowego i grubości obramowania — sygnatura nie pozwala podać samego koloru |
| `prefers-reduced-motion: reduce` zeruje przejścia | Reguła globalna w `tokens.css` + test |
| Zakaz `outline: none` | stylelint (`K-01.8`) |

Skala statusu KDS ([`05`](../_docs_Nowa/05_System_Projektowy.md) §2.4): w normie (< 80% czasu normatywnego)
`--color-success` / licznik / obramowanie 2 px · uwaga (80–100%) `--color-warning` / 4 px ·
przekroczony (> 100%) `--color-danger` / licznik **pulsujący** / 6 px.

**Weryfikacja:** test na komplet stanów per komponent; `axe-core` na stronie z galerią
komponentów — 0 naruszeń krytycznych i poważnych; test `prefers-reduced-motion`.

---

### `K-02.9` · Bramka „zero pobieranych krojów pisma"

**Powstaje:** `.github/workflows/ci.yml` — krok w zadaniu `budget`

Dwie kontrole:
1. Statyczna: brak `@font-face` w zbudowanym CSS wszystkich czterech aplikacji.
2. Dynamiczna: przebieg Playwright na `apps/guest` przechwytuje żądania sieciowe;
   0 żądań do `fonts.googleapis.com`, `fonts.gstatic.com`, `use.typekit.net` oraz 0 żądań
   o zasoby `.woff`, `.woff2`, `.ttf`, `.otf`.

> Krój marki kosztowałby 30–120 kB — **jedną trzecią budżetu 60 kB JS**. Stos systemowy
> ma pełne wsparcie polskich diakrytyków i cyrylicy dla wersji ukraińskiej.

---

### `K-02` — Definicja ukończenia

- [ ] `Money` pokryty w ≥ 95%, z testami właściwościowymi dla `I1`–`I4`.
- [ ] Podział `100,00 zł` na 3 sumuje się do `100,00 zł`, a reszta trafia do inicjatora — test jawny.
- [ ] VAT liczony wstecz od brutto dla stawek `800` i `2300` — test na wartościach granicznych.
- [ ] Wszystkie zdefiniowane pary tokenów przechodzą test kontrastu w motywie jasnym **i** ciemnym.
- [ ] Każdy z 24 komponentów `packages/ui` ma sześć stanów.
- [ ] `prefers-reduced-motion: reduce` zeruje wszystkie przejścia — test.
- [ ] Reguła ESLint dla kwot wykrywa `parseFloat`, `toFixed`, `Number(` i arytmetykę `number` —
      zweryfikowana na celowo błędnym kodzie.
- [ ] Zero pobieranych krojów pisma — obie kontrole z `K-02.9` w CI.
- [ ] Sześć uzupełnień tokenów z `K-02.6` rozstrzygniętych i zapisanych w §10.
- [ ] Globalna DoD (`ROADMAP.md` §5).

### `K-02` — Czego NIE robić

- **Nie budować komponentów specyficznych dla powierzchni** (`MenuItemCard`, `TableCard`,
  `TicketCard`) — powstają w `K-10` … `K-14`, bo ich kształt wynika z makiet.
- **Nie dodawać obsługi wielu walut.** `RULE-027`: PLN do v3. Schemat dopuszcza, kod nie implementuje.
- **Nie wprowadzać systemu ikon** — to `K-10`, razem z alergenami, gdzie piktogram ma wymóg prawny.
- Nie dodawać metody `toNumber()` do `Money`. Nigdy.

### `K-02` — Ryzyka

| Ryzyko | Przeciwdziałanie |
|---|---|
| Para tokenów nie przechodzi kontrastu | Zgłoszenie do §10, nie cicha zmiana wartości z `05` |
| Generatory `fast-check` na `bigint` dają zakresy nierealistyczne | Ograniczyć do ±10¹² groszy (10 mld zł) — poza tym zakresem test bada arytmetykę, nie domenę |
| `packages/ui` przyciąga zależności psujące budżet gościa | `sideEffects: false` + pomiar `apps/guest` po każdej partii komponentów |
| Reguła ESLint na kwoty daje fałszywe alarmy | Wyjątki wyłącznie w konfiguracji (`packages/money`, `packages/formats`), nigdy komentarzem w kodzie |

<!-- SEKCJA-5 -->
