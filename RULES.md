# RULES — T60 RAZEM (repo `all60`, adres `/all60/`)

Zasady appki **T60 RAZEM**: jak zbudowany jest plik, jak liczy każda tabela, co znaczą stany na kartach, z czego korzysta MOJE
i jak bezpiecznie wprowadzać zmiany. Zasady całej rodziny appek (SZUKAJ, MOJE) — `RULES.md` w repo `moje` / `szukaj60`;
analiza modeli — `all50/analiza/PROJEKT.md`. Źródłem prawdy o liczeniu jest kod silników w `index.html`; ten plik opisuje go słowami.

## Sposób pracy

- Rozmowa po polsku. **PR scalany dopiero po słowie „scal”** od użytkownika.
- Przy każdej zmianie podbić `APP_VER` powłoki (nagłówek) i wersję zmienionej tabeli (`ver` w `APPS` + wersja wewnątrz jej kodu).
- Test przed PR: Playwright z Chromium na **stanie z telefonu** (`stan.json` z prywatnego repo z kodami → `storage` do localStorage),
  serwer plików z katalogami jak na px5060.github.io (`/all60/`, `/moje/`, …), brak błędów JS, wynik porównany z silnikiem tabeli.
- Kopia zapasowa poza GitHubem: paczka `px5060_kopia_<data>.zip` (wszystkie repo z historią + `stan.json` + `ODBUDOWA.md`) — Claude na prośbę
  „zrób kopię zapasową”; stan telefonu także w MOJE → KODY → 💾 Zapisz pełną kopię do pliku.

## Budowa pliku `index.html`

- **Jeden plik**: powłoka (pole kodu, ekran GRA wspólny dla tabel, przełącznik BET, 💰 zakłady na następny wiersz, DANE: eksport / import /
  auto-kopia) + tabele jako JSON w linii `const APPS = [ … ]`. Każda tabela: `{k, short, name, ver, store, key, …, html}` — `html` to cała
  appka tabeli (silnik bez zmian + nakładka), osadzona w ramce `#fr_<k>`.
- **Edycja tabeli** (bez build.py): w Pythonie wczytać linię `APPS` (`json.loads`), zmienić `html` tabeli, zapisać
  `json.dumps(arr, ensure_ascii=False).replace('</', '<\\/')` (+ `;`). Przed zmianą sprawdzić, że ten zapis odtwarza obecną linię **bajt w bajt**.
- Nakładka każdej tabeli wystawia: `window.T60` (`cards`, `lines`, `sync`, `add`, `undo`, `go`, `render`, `selfTest`) i `UT.A`
  (`models`, `cell`, `stats`, `fc`, `desc`, `codeRole`, …) — z nich korzystają powłoka (GRA, TABELA, STATY) i **appka MOJE**.

## Kody

- Wspólne z MOJE, SZUKAJ i ZESZYTEM (ten sam adres px5060.github.io): baza `MASTER_SEED` (Nr 1 – 12 837) + kody dopisane w telefonie
  `t60razem_v1_added`. Powłoka podaje ten sam ciąg każdej tabeli.
- Kod T60: 1. cyfra — 0 brak gola 40–50', 1 gol 40–45', 2 gol 46–50', 3 oba; 2. cyfra — gol 50–60' (**x1x**); 3. cyfra — gol 60–70' (**xx1**).
- Dziennik, w której appce wpisano kod: `codesrc_v1_t60` (MOJE pokazuje znaczek appki przy ostatnim kodzie).

## Przełącznik BET i progresje

- **BET xx1** — PIKOFF, BUST, TRÓJKA, MIX3, SZUKAJ (modele xx1); **BET x1x** — TOP5, TOP7. Przełącznik pokazuje tylko tabele wybranego celu.
- Kurs 3,0, WIN = +2 × stawka. Progresja K8: **8 · 8 · 16 · 32 · 64 · 128 · 256 · 512 zł** (PIKOFF, TOP5, TOP7, BUST, MIX3, SZUKAJ);
  **TRÓJKA: 8 · 16 · 32 · 64 · 128 · 256 · 512 · 1024 zł** (przegrany k8 = −2 040 zł).
- Na ekranie „BUST” piszemy „LOST” (tylko napisy), z wyjątkiem nazw tabeli i modeli BUST.

## Tabele

### PIKOFF TOP (xx1 · 2-slot · K8) — 6 modeli `MkOo`: M5O12, M6O3, M7O3, M7O13, M8O5, M8O11
- **Baza 2-slot** na xx1: kod xx1 = BUILDUP → następne 2 wiersze to SLOT 1 i SLOT 2; xx1 w slocie = WIN bazy z K (akumulator bazy),
  pudło w SLOT 2 → K + 2, czekam na kolejny xx1 (BUILDUP).
- Model `MkOo`: TRIGGER = WIN bazy z K = k → START po `o` kolejnych WIN-ach bazy → okno gry: zakłady xx1 w slotach, krok gry 1, 2, …
  (stawki K8) do WIN w slocie; krok > 8 = LOSS przy najbliższym WIN bazy. Każde okno od k1.
- **Krok teraz** (1.2.7): okno w grze → `krok_gry + 1` (jak prognoza `fc`), bez okna → następne okno od k1.

### TOP5 / TOP7 (x1x · silnik 1T · K8)
- Model: **STEP** (zbiór kodów) **× min pod rząd → TRIGGER → zakład x1x na wierszu TRIGGER + off**. Progresja **trwała**: przegrany krok →
  następny zakład kolejnym krokiem (także w kolejnym cyklu), WIN → k1, przegrany k8 = LOST → k1.
- TOP5:
  - M1: 2× 010∪110∪111∪200∪310 · TRIGGER x1x · +6
  - M2: 2× 010∪200∪210∪310∪311 · TRIGGER x1x · +8
  - M3: 2× 010∪200∪210∪310 · TRIGGER x1x · +8
  - M4: 2× 010∪201∪210∪211∪300 · TRIGGER x1x · +8
  - M5: 2× 010∪201∪211∪300 · TRIGGER x1x · +8
- TOP7:
  - M1: 3× 001∪010∪200 · TRIGGER 000∪201 · +4
  - M2: 2× 001∪110∪310 · TRIGGER 100∪301 · +3
  - M3: 3× 001∪010∪200 · TRIGGER 000∪101 · +4
  - M4: 2× 001∪110∪300 · TRIGGER 100∪301 · +3
  - M5: 6× xx0 · TRIGGER 011∪100 · +6
  - M6: 4× 000∪010∪300 · TRIGGER 001∪011 · +8
  - M7: 5× 000∪001∪100 · TRIGGER 100∪101 · +3
- Krok teraz: `results[i].live.nextK` (karta: „cykl#… · Kn jeśli wpadnie …”).

### BUST (xx1 · K8) — BUST 012 (12 modeli `B…`) + BUST ODM (14 modeli `P…`)
- Silnik jak SZUKAJ: STEP **SERIA** (× x pod rząd) albo **UKŁAD** (p → q) → TRIGGER → zakład xx1 na TRIGGER + off; progresja K8 trwała.
- BUST 012:
  - B0A: SERIA 000∪101∪110 ×3 · TRIGGER 0xx → xx0 · +3
  - B0C: UKŁAD 0xx∪2xx∪3xx → 011 · TRIGGER xx0 → xx0 · +4
  - B0E: SERIA 010∪100∪200 · TRIGGER x01 → x10 · +3
  - B1A: UKŁAD x1x → x01 · TRIGGER x1x → x0x · +2
  - B1B: SERIA 001∪210∪300 · TRIGGER 1xx∪2xx∪3xx → x10 · +4
  - B1D: SERIA 010∪200∪201 ×2 · TRIGGER x00 → 0xx∪2xx · +1
  - B1E: UKŁAD x10 → 1xx∪2xx · TRIGGER 010 → 0xx∪2xx∪3xx · +3
  - B2A: SERIA 100∪101∪110 · TRIGGER 2xx∪3xx · +2
  - B2B: SERIA 001∪010∪310 ×2 · TRIGGER 0xx∪1xx∪2xx → x0x · +2
  - B2C: SERIA 201∪210∪301 · TRIGGER 0xx∪2xx∪3xx → x00 · +1
  - B2D: UKŁAD 2xx → x0x · TRIGGER x00 → 1xx∪2xx · +4
  - B2E: SERIA 011∪211∪311 · TRIGGER 1xx∪2xx → xx0 · +4
- BUST ODM:
  - P0A: UKŁAD x1x → x01 · TRIGGER x1x → x00 · +2
  - P0B: UKŁAD x10 → xx1 · TRIGGER 010 → x00 · +3
  - P0C: UKŁAD 2xx∪3xx → xx1 · TRIGGER 001 → 0xx∪2xx · +2
  - P0D: UKŁAD x10 → 1xx∪2xx∪3xx · TRIGGER 001 → x00 · +3
  - P0E: UKŁAD x11 → 000 · TRIGGER 000 → xx0 · +3
  - P1A: UKŁAD x1x → 1xx∪2xx · TRIGGER 010 → 0xx∪2xx∪3xx · +3
  - P1B: SERIA 110∪111∪311 · TRIGGER 000 → 0xx · +3
  - P1C: SERIA 0xx∪2xx ×2 · TRIGGER x01 → x10 · +3
  - P1D: SERIA 110∪210∪300 · TRIGGER 100 → 0xx∪1xx∪2xx · +3
  - P1E: UKŁAD 2xx∪3xx → xx1 · TRIGGER 0xx∪1xx∪2xx · +2
  - P2A: SERIA 100∪101∪110 · TRIGGER 2xx∪3xx · +2
  - P2B: UKŁAD x00 → 010 · TRIGGER 000 → 000 · +2
  - P2C: UKŁAD x01 → x1x · TRIGGER 1xx → 0xx∪3xx · +1
  - P2E: UKŁAD x10 → xx1 · TRIGGER x0x → 010 · +2
- Krok teraz: statystyki „Stan teraz: krok kN” (karta: „cykl#… · kN …”).

### TRÓJKA xx1 V1.3 (K8) — T1 +8, T1 +9, COMBO +8&+9
- **Baza V1.3** na xx1: dwa xx1 pod rząd = **para** (1.) + **SPRAWDŹ** (2.); **trzeci kod** rozstrzyga — xx1 = WIN cyklu z M = K bazy,
  inny = **miss** → K + 1 i szukam nowej pary. Pojedynczy xx1 bez drugiego zaraz po nim — pomijany (K bez zmian). Po WIN **cooldown**:
  kolejne xx1 pod rząd pomijane, aż padnie kod nie-xx1. „Baza teraz” w statystykach: `cykl otwarty K=N · szukam nowej pary` /
  `szukam SPRAWDŹ` / `po SPRAWDŹ — trzeci xx1 = WIN` / `cooldown`.
- TRIGGER = WIN cyklu z **M = 1**. Gałęzie: **T1 +8** gra na cyklu T+8 (off 9), **T1 +9** na T+9 (off 10); przed nimi cykle przygotowania
  i ★ START. W cyklu gry zakład stawiany na **trzecim kodzie po parze i SPRAWDŹ**, krokiem = K bazy (8 · 16 · … · 1024 zł); WIN = 3 × stawka;
  K > 8 = BUST −2 040 zł. Gra trwa „na najbliższy WIN” — rozbita para nic nie kosztuje, krok zostaje.
- **COMBO +8&+9**: obie gałęzie razem, jedna progresja (krok = K bazy), zdarzenia z prefiksem gałęzi.
- Krok teraz: linijka karty „zakład dopiero po SPRAWDŹ (krok N)” albo „Baza teraz … K=N” — TRÓJKA gra krokiem bazy.

### MIX3 v14 (K8) — T1+1, T1+3, T1+4, T1+5, T1+8, T1+9 · T2+2, T2+5, T2+6, T2+7
- Liczą się tylko **HIT = 101 · 201 · 301** i **STEP = 100 · 200 · 300**; pozostałe kody są pomijane.
- **Cykl**: od HIT (kotwica); STEP, a po nim HIT = kolejny krok cyklu; HIT bez STEP przed nim = koniec cyklu, M = liczba kroków + 1.
- Model T*m*+*o*: TRIGGER = cykl z **M = m** (1 albo 2) → START po *o* − 1 cyklach przygotowania → gra na cyklu T + *o*: krok uzbrojony
  (`armed`, ▼ GRA) — HIT = WIN na kroku k, STEP = pudło (na k8 = BUST −1 024 zł), potem po HIT krok k + 1 (`wait`); inne kody — bez
  rozstrzygnięcia. Każde okno od k1.
- **Krok teraz** (1.1.1): `armed` → n, `wait` → n + 1, bez okna (przygotowanie, BUST, brak) → k1.

### SZUKAJ (x1x i xx1 · K8)
- Modele przeniesione z appek SZUKAJ / SZUKAJ+ (lista w telefonie `t60razem_v1_szk`), silnik 1:1 z SZUKAJ+ (STEP ×x → [STEP2] → TRIGGER ×n →
  zakład +off), progresja K8 trwała. Na karcie krok i stawka („kN · X zł”).

## Karty GRA, statystyki i 💰

- Karta modelu: linijki `p` 1…6 — **p1 = GRA na następnym wierszu** (ze stawką); dalsze: gra w toku, warunek („TRIGGER jeśli…”), gra za N /
  START, przygotowanie, brak. Dotknięcie → statystyki modelu (przełącznik Statystyki ⇄ Zeszyt — bieżąca gra na kartach ZESZYTU), przytrzymanie
  → tabela modelu.
- **„Stan teraz”** w statystykach: PIKOFF i MIX3 (2.20.0), BUST („krok kN”); TRÓJKA — „Baza teraz”.
- 💰 na pasku: zakłady na następny wiersz, cofanie zapisanych; kwadraciki wyników (2. dotknięcie — co było po takim wyniku).
- Tryb offline (kopia w pamięci), dzień / noc, pasek appek na górze (MOJE, RAZEM, SZUKAJ, SZUKAJ+, LOST, SofaScore…).

## Appka MOJE korzysta z RAZEM T60 — nie zmieniać bez zmiany w MOJE

- MOJE otwiera RAZEM w ukrytej ramce i czyta: `__MOJE` (MOJE GRY `t60razem_v1_moje`, zakłady `t60razem_v1_bets`, `track`, `rowRes`, `betList`,
  `lab`), `T60.cards()` (linijki kart, a dla PIKOFF / MIX3 pola **`krok` / `stawka` / `okno`**), `UT.A` (`models`, `cell`, `stats`).
- Krok modelu w MOJE: pole `krok` na karcie → linijka karty z „kN” / „krok N” („off +N · K8” to limit, nie krok) → TRÓJKA „Baza teraz … K=N”
  → ostatni rozstrzygnięty zakład w tabeli. Sprawdzone na wszystkich modelach RAZEM T50 / T60 (MOJE 1.57.1–1.58.0).

## Historia (najważniejsze zmiany)

- 2.20.0 (26.11) — PIKOFF 1.2.7 i MIX3 1.1.1 podają krok wprost („Stan teraz” + pole krok na karcie).
- 29.10 — kwadraciki wyników: 2. dotknięcie pokazuje, co było po takim wyniku.
- 26.10 — Zeszyt z RAZEM (opisy jak w kolumnie GRA, zakład do gry), przełącznik Statystyki ⇄ Zeszyt także w oknie karty z GRA.
- 25.10 — praca offline. 24.10 — ostatnie 20 cykli w statystykach. 23.10 — dziennik, w której appce wpisano kod.
- 20–21.10 — 💰 zakłady na następny wiersz, cofanie, dotknięcie → statystyki. 17.10 — pasek appek, dzień / noc.
- 14.10 — 2.13.0: przełącznik BET xx1 / x1x; MOJE / MOJE ZAKŁADY przeniesione do appki MOJE; BUST → LOST na ekranie.
- 8–11.10 — TRÓJKA COMBO na GRA, po SPRAWDŹ ▼ GRA; MIX3 STATY jak w TRÓJCE; „w grze” / NIEBAWEM (później w MOJE).
