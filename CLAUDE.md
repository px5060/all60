# all60

Dwa rodzaje pracy w tym repo:

- **Appka T60 RAZEM** (kod, PR) — `index.html` (appki jako JSON w `APPS = […]`); zasady rodziny appek w repo `szukaj60` (`RULES.md`).
- **Analiza modeli do gry** (MOJE / SZUKAJ / RAZEM / ZESZYT, wybór modeli, Nr i stawki, sesja jednego modelu, propozycje) — projekt
  jest **jeden, w repo `all50`** (folder `analiza/`). Na start:
  1. `[ -d ../all50 ] || git clone -q --depth 1 https://github.com/px5060/all50 ../all50` (repo publiczne, odczyt wystarczy),
  2. przeczytaj `../all50/analiza/PROJEKT.md` i postępuj według niego; narzędzia uruchamiaj z `../all50/analiza/narzedzia/`
     (np. `bash ../all50/analiza/narzedzia/pobierz.sh`, `python3 ../all50/analiza/narzedzia/model.py …`).
  3. Zmiany w `analiza/` (np. nowe modele w `MODELE.json`) zapisuje się w repo `all50` — jeśli ta sesja nie może tam pushować,
     podaj zmianę użytkownikowi do przeniesienia albo poproś o dodanie repo `all50` do sesji.
  4. **Sesja ocen** (tylko „oceń …” / „wyszukaj modele …”) — `../all50/analiza/SESJA_OCEN.md`.

Rozmowa po polsku. PR scalany dopiero po słowie „scal” od użytkownika.
