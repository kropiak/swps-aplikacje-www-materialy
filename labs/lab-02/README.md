# Lab 2: Git i GitHub

**Cel:** po tych zajęciach umiesz pracować z gałęziami, rozwiązać konflikt, wysłać projekt na GitHuba
i przeprowadzić pełny cykl zmiany przez *pull request*, najpierw w terminalu, potem w VS Code.

**Kod po labie:** gałąź [`lab-02`](https://github.com/kropiak/swps-aplikacje-www-labs/tree/lab-02)
**Slajdy:** [lab-02](https://kropiak.github.io/swps-aplikacje-www-materialy/slides/lab-02.html)
**Ściągawki:** [git](../../sciagawki/git.md) · [terminal](../../sciagawki/terminal.md)

| Część | Czas | Temat |
|---|---|---|
| 1 | 45 min | Gałęzie, scalanie i konflikt w terminalu |
| 2 | 45 min | GitHub: repozytorium zdalne, `push`, `pull`, `clone`, Markdown |
| 3 | 45 min | Git w VS Code, *pull request*, konflikt z GitHubem |

**Wymagania:** ukończony [lab 1](../lab-01/README.md): folder `konsultacje` z repozytorium git i kilkoma commitami
oraz konto na [GitHubie](https://github.com).

> 📌 Polecenia gita są identyczne na 🪟 Windows i 🍎 macOS. Różnice dotyczą tylko logowania do GitHuba (punkt 2.3).
> **Wszystkie polecenia wpisuj w terminalu VS Code** (Ctrl + \` / Cmd + \`), w folderze `konsultacje`.

---

## Część 1: Gałęzie i konflikty (terminal)

### 1.1. Rozgrzewka i jedno nowe ustawienie

Otwórz folder `konsultacje` w VS Code (`code .` lub *File → Open Folder…*) i terminal. Sprawdź stan repozytorium:

```bash
git status
git log --oneline
```

Dodaj jedno ustawienie, które przyda się dziś przy łączeniu zmian z GitHuba:

```bash
git config --global pull.rebase false
```

> Mówi ono gitowi: „przy `git pull` łącz zmiany zwykłym scaleniem (*merge*)”. Bez niego nowsze wersje gita
> w niektórych sytuacjach przerywają `pull` z komunikatem *„Need to specify how to reconcile divergent branches”*.

### 1.2. Czym jest gałąź?

Dotąd wszystkie commity trafiały na jedną linię, gałąź `main`.
**Gałąź** to osobna linia rozwoju: możesz na niej spokojnie eksperymentować, a gdy wszystko działa, **scalić** (*merge*) ją z `main`.
Jeśli eksperyment się nie uda, po prostu porzucasz gałąź, a `main` pozostaje nietknięty.

```text
main:         A───B───C───────────M
                       \         /
plan-funkcji:           D───E───┘
```

### 1.3. Pierwsza gałąź

```bash
git branch                      # lista gałęzi, * oznacza bieżącą
git switch -c plan-funkcji      # utwórz gałąź i przejdź na nią
git branch
```

Dopisz na końcu `README.md` sekcję:

```markdown
## Planowane funkcje

- przeglądanie listy specjalistów
- rezerwacja wizyty
- opinie po wizycie
```

```bash
git add README.md
git commit -m "Dodaj listę planowanych funkcji"
git log --oneline
```

Teraz mała magia. Wróć na `main` i zajrzyj do `README.md`:

```bash
git switch main
```

Sekcja zniknęła! 😮 Spokojnie: jest bezpieczna na gałęzi `plan-funkcji`. Git podmienia pliki w folderze tak,
żeby odpowiadały gałęzi, na której jesteś. Scal gałąź z `main`:

```bash
git merge plan-funkcji          # komunikat "Fast-forward": main po prostu "dogonił" gałąź
git log --oneline
git branch -d plan-funkcji      # gałąź spełniła swoje zadanie, usuwamy ją
```

### 1.4. Konflikt (nie bój się go!)

Konflikt pojawia się, gdy na dwóch gałęziach zmieniono **tę samą linię** w różny sposób. Git nie zgaduje, która wersja jest dobra, tylko pyta Ciebie.
Wywołamy go celowo.

**Krok 1: gałąź z nowym tytułem**

```bash
git switch -c nowy-tytul
```

Zmień **pierwszą linię** `README.md` na:

```markdown
# Konsultacje: rezerwacja wizyt
```

```bash
git commit -am "Zmień tytuł README"
```

> `-a` w `git commit -am` automatycznie dodaje zmiany w plikach, które git już śledzi (skrót od `git add` + `git commit`).
> Nowych plików nie dodaje!

**Krok 2: w tym czasie na `main` ktoś zmienia tę samą linię**

```bash
git switch main
```

Zmień **pierwszą linię** `README.md` na:

```markdown
# Konsultacje psychologiczne
```

```bash
git commit -am "Doprecyzuj tytuł README"
```

**Krok 3: scalamy**

```bash
git merge nowy-tytul
```

```text
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Otwórz `README.md`. Na górze zobaczysz znaczniki konfliktu:

```text
<<<<<<< HEAD
# Konsultacje psychologiczne
=======
# Konsultacje: rezerwacja wizyt
>>>>>>> nowy-tytul
```

- między `<<<<<<< HEAD` a `=======` jest wersja z bieżącej gałęzi (`main`),
- między `=======` a `>>>>>>> nowy-tytul` jest wersja ze scalanej gałęzi.

**Krok 4: rozwiązujemy.** Tym razem ręcznie: usuń wszystkie trzy linie ze znacznikami i zostaw połączoną wersję:

```markdown
# Konsultacje psychologiczne: rezerwacja wizyt
```

> VS Code nad konfliktem pokazuje przyciski *Accept Current Change / Accept Incoming Change / Accept Both Changes*.
> Wrócimy do nich w części 3. Dziś chcemy zobaczyć, że konflikt to po prostu tekst w pliku.

```bash
git status                       # README.md: both modified
git add README.md                # "rozwiązałem konflikt w tym pliku"
git commit -m "Scal gałąź nowy-tytul"
git log --oneline --graph        # widać rozgałęzienie i scalenie
git branch -d nowy-tytul
```

✅ **Punkt kontrolny 1:** `git log --oneline --graph` pokazuje rozgałęzienie i commit scalający, a `git status` mówi *working tree clean*.

---

## Część 2: GitHub

### 2.1. Repozytorium lokalne i zdalne

Dotąd cała historia była tylko na Twoim komputerze. **GitHub** przechowuje kopię repozytorium w internecie. Dzięki temu:

- masz kopię zapasową,
- możesz pracować z kilku komputerów (np. w domu i w pracowni),
- możesz współpracować z innymi i pokazać kod prowadzącemu.

```text
 Twój komputer                           GitHub
 (repozytorium lokalne)  --git push-->   (repozytorium zdalne "origin")
                         <--git pull--
```

### 2.2. Tworzymy repozytorium na GitHubie

1. Zaloguj się na https://github.com, kliknij **+** (prawy górny róg) → **New repository**.
2. **Repository name:** `konsultacje`
3. **Public** (prościej). Jeśli wolisz **Private**, dodaj potem prowadzącego (`kropiak`) w *Settings → Collaborators*.
4. ⚠️ **Nie zaznaczaj** *Add a README*, *.gitignore* ani *license*. Repozytorium musi być **puste**, bo nasze pliki już mamy.
5. **Create repository**.

GitHub pokaże stronę z instrukcjami. Interesuje nas sekcja **„…or push an existing repository from the command line”**.

### 2.3. Podłączamy i wysyłamy (`push`)

Skopiuj adres repozytorium (HTTPS), np. `https://github.com/twoj-login/konsultacje.git`, i w terminalu VS Code wpisz:

```bash
git remote add origin https://github.com/twoj-login/konsultacje.git
git remote -v                     # sprawdź, czy adres jest poprawny
git push -u origin main
```

- `origin` to umowna nazwa repozytorium zdalnego,
- `-u` zapamiętuje powiązanie, więc następnym razem wystarczy samo `git push`.

**Logowanie przy pierwszym `push`:**

🪟 **Windows:** otworzy się okno *Git Credential Manager* → **Sign in with your browser** → zaloguj się na GitHubie i zatwierdź.
Dane zostaną zapamiętane w systemie.

🍎 **macOS:** jeśli pracujesz w **terminalu VS Code**, VS Code zapyta: *„The extension 'GitHub' wants to sign in using GitHub”* → **Allow** →
zaloguj się w przeglądarce → wróć do VS Code. To jeden z powodów, dla których dziś pracujemy w terminalu VS Code.

> ⚠️ Jeśli terminal prosi o **hasło** do GitHuba: GitHub nie przyjmuje zwykłego hasła w terminalu.
> Przerwij (**Ctrl + C**) i wykonaj `push` z terminalu VS Code albo przyciskiem w panelu Source Control (część 3).
> Alternatywa na macOS: `brew install --cask git-credential-manager` (wymaga [Homebrew](https://brew.sh/)).

Odśwież stronę repozytorium na GitHubie: są Twoje pliki, a na dole wyświetla się `README.md` 🎉
Kliknij **commits** (nad listą plików), żeby zobaczyć całą historię, także z części 1.

### 2.4. Markdown i edycja na GitHubie

`README.md` jest napisany w **Markdown**, prostym języku formatowania tekstu. Najważniejsze elementy:

| Piszesz | Dostajesz |
|---|---|
| `# Tytuł`, `## Podtytuł` | nagłówki |
| `**pogrubienie**`, `*kursywa*` | **pogrubienie**, *kursywa* |
| `- element` | lista punktowana |
| `1. element` | lista numerowana |
| `[tekst](https://adres)` | link |
| `` `kod` `` | `kod` |
| <code>```python</code> … <code>```</code> | blok kodu z kolorowaniem |

Zmienimy README **bezpośrednio na GitHubie** (tak jakby zrobił to ktoś inny):

1. Kliknij plik `README.md` → ikona ołówka ✏️ (*Edit this file*).
2. Pod sekcją *Planowane funkcje* dopisz:

   ```markdown
   ## Technologie

   - [Python](https://www.python.org/): język programowania
   - [Django](https://www.djangoproject.com/): framework webowy (**od labu 3**)
   - [git](https://git-scm.com/) i [GitHub](https://github.com/): kontrola wersji
   ```

3. Zakładka **Preview** pokaże efekt.
4. **Commit changes…** → opis `Dodaj sekcję Technologie` → **Commit changes**.

### 2.5. Pobieramy zmiany (`pull`)

Na GitHubie jest teraz commit, którego nie masz lokalnie. Pobierz go:

```bash
git pull
git log --oneline
```

Sekcja *Technologie* pojawiła się w Twoim `README.md`.

> **Złota zasada:** zaczynasz pracę → `git pull`. Kończysz pracę → `git commit` + `git push`.

### 2.6. Klonowanie (`clone`)

Sklonowanie to pobranie **całego** repozytorium z historią, np. na drugi komputer (albo w pracowni!).
Sprawdźmy to na kopii obok:

```bash
cd ..
git clone https://github.com/twoj-login/konsultacje.git konsultacje-kopia
cd konsultacje-kopia
git log --oneline                # cała historia jest na miejscu
ls                               # 🍎 ls -a; nie ma folderu .venv! (był w .gitignore)
```

Żeby uruchomić projekt w kopii, trzeba odtworzyć środowisko (jak w zadaniu A3 z labu 1):
`python -m venv .venv` → aktywacja → `pip install -r requirements.txt`.

Usuń kopię i wróć do projektu:

```bash
cd ..
# 🪟 Remove-Item -Recurse -Force konsultacje-kopia
# 🍎 rm -rf konsultacje-kopia
cd konsultacje
```

✅ **Punkt kontrolny 2:** repozytorium `konsultacje` jest na GitHubie z całą historią, a lokalnie `git status` pokazuje
*Your branch is up to date with 'origin/main'*.

---

## Część 3: Git w VS Code i pull request

### 3.1. Panel Source Control

Otwórz panel **Source Control**: ikona rozgałęzienia po lewej albo **Ctrl/Cmd + Shift + G**.

| Element | Odpowiednik w terminalu |
|---|---|
| lista **Changes** | `git status` |
| kliknięcie pliku → widok porównania | `git diff` |
| **+** przy pliku | `git add plik` |
| **−** przy pliku w *Staged Changes* | `git restore --staged plik` |
| ↶ (*Discard Changes*) | `git restore plik` ⚠️ |
| pole *Message* + **Commit** | `git commit -m "..."` |
| **Sync Changes** / ikona ⟳ na pasku stanu | `git pull` + `git push` |
| nazwa gałęzi na **dolnym pasku stanu** | `git branch` / `git switch` |
| sekcja **Graph** (na dole panelu) | `git log --oneline --graph` |

### 3.2. Gałąź w VS Code

1. Kliknij nazwę gałęzi `main` na dolnym pasku stanu → **Create new branch…** → nazwa `licznik-specjalistow` → Enter.
2. W `hello.py`, na końcu funkcji `main()` (pod `console.print(table)`), dopisz:

   ```python
       console.print(f"Liczba specjalistów: [bold]{len(SPECIALISTS)}[/]")
   ```

3. Uruchom skrypt (▶) i sprawdź, czy działa.
4. W Source Control kliknij plik, aby zobaczyć zmiany, potem **+**, wpisz opis `Wyświetl liczbę specjalistów` i **Commit**.
5. Kliknij **Publish Branch**: gałąź trafi na GitHuba.

### 3.3. Pull request

**Pull request (PR)** to prośba: „przejrzyj moje zmiany z gałęzi X i scal je z `main`”.
W zespołach to główny sposób wprowadzania zmian: ktoś inny czyta kod, zanim trafi do `main`. Przećwiczymy go we własnym repozytorium.

1. Na GitHubie, w repozytorium `konsultacje`, pojawi się żółty pasek **„licznik-specjalistow had recent pushes”** → **Compare & pull request**.
   (Albo: zakładka **Pull requests → New pull request**, *base: main* ← *compare: licznik-specjalistow*.)
2. Tytuł: `Wyświetl liczbę specjalistów`, w opisie napisz krótko, co i dlaczego zmieniasz.
3. **Create pull request**.
4. Zajrzyj do zakładki **Files changed**: tak wygląda przegląd kodu. Możesz kliknąć linię i dodać komentarz.
5. **Merge pull request** → **Confirm merge** → **Delete branch**.

Wróć do VS Code:

1. Przełącz się na `main` (pasek stanu → `main`).
2. **Sync Changes** (lub `git pull`): zmiana z PR jest teraz w Twoim `main`.
3. Usuń lokalną gałąź: **Ctrl/Cmd + Shift + P** → `Git: Delete Branch…` → `licznik-specjalistow`.

### 3.4. Konflikt z GitHubem i Merge Editor

Częsty scenariusz: zmieniasz coś na jednym komputerze, a zapominasz o `pull` po pracy na drugim.

1. **Na GitHubie** edytuj `README.md` (ołówek ✏️) i w *Planowanych funkcjach* dopisz **na końcu listy**:
   `- przypomnienia e-mail o wizycie`. Zatwierdź commit.
2. **W VS Code (bez pull!)** dopisz w tym samym miejscu, na końcu listy: `- panel administratora`. Zapisz i zrób commit `Dodaj panel administratora do planu`.
3. Kliknij **Sync Changes**. VS Code zgłosi konflikt w `README.md`.
4. Otwórz plik. Nad konfliktem są przyciski:
   - **Accept Current Change**: zostaw moją wersję,
   - **Accept Incoming Change**: weź wersję z GitHuba,
   - **Accept Both Changes**: zostaw obie ← tego chcemy.

   Możesz też kliknąć **Resolve in Merge Editor**: trzy panele (moje / przychodzące / wynik) i pola wyboru przy każdej zmianie.
5. Upewnij się, że na liście są obie funkcje, zapisz plik, w Source Control kliknij **+** przy `README.md` (lub **Complete Merge** w Merge Editorze),
   potem **Commit** (opis jest już przygotowany) i **Sync Changes**.

Na koniec obejrzyj sekcję **Graph** w panelu Source Control albo na GitHubie: *Insights → Network*.

✅ **Punkt kontrolny 3:** `README.md` na GitHubie zawiera obie nowe funkcje, `hello.py` wyświetla liczbę specjalistów, a w zakładce *Pull requests → Closed* jest Twój scalony PR.

---

## Repozytorium projektu zaliczeniowego

Na koniec zajęć (lub w domu, w ramach [części B zadań](zadania.md)) wyślij na GitHuba również repozytorium swojego projektu
z labu 1 (folder z projektem, `git init` i pierwszy commit już masz):

1. Na GitHubie: **New repository** → nazwa projektu → **puste** repozytorium.
2. `git remote add origin https://github.com/twoj-login/nazwa-projektu.git`
3. `git push -u origin main`

---

## Podsumowanie

- [ ] Umiem tworzyć gałęzie, przełączać się między nimi i scalać je (`switch -c`, `switch`, `merge`, `branch -d`).
- [ ] Wiem, skąd biorą się konflikty, i umiem je rozwiązać ręcznie i w VS Code.
- [ ] Mam repozytorium `konsultacje` na GitHubie i umiem używać `push`, `pull` i `clone`.
- [ ] Znam podstawy Markdown.
- [ ] Umiem zrobić commit, gałąź i synchronizację w panelu Source Control.
- [ ] Przeprowadziłem pełny cykl *pull request*.

**Zadania po labie:** [zadania.md](zadania.md)

## Typowe problemy

| Objaw | Rozwiązanie |
|---|---|
| `remote origin already exists` | Adres był już dodany. Popraw go: `git remote set-url origin https://github.com/twoj-login/konsultacje.git` |
| `src refspec main does not match any` | Nie masz jeszcze żadnego commita albo gałąź nazywa się `master`. Sprawdź `git branch`; zmiana nazwy: `git branch -M main` |
| `! [rejected] ... (fetch first)` przy `push` | Na GitHubie są zmiany, których nie masz: `git pull`, potem `git push` |
| `Need to specify how to reconcile divergent branches` | `git config --global pull.rebase false` i ponownie `git pull` |
| Terminal prosi o *Password* | Przerwij i zrób `push` z terminalu VS Code / panelu Source Control (patrz 2.3) |
| `Repository not found` | Literówka w adresie albo repozytorium prywatne na innym koncie. Sprawdź `git remote -v` |
| `GH007: Your push would publish a private email address` | Na GitHubie: *Settings → Emails* → skopiuj adres `…@users.noreply.github.com` i ustaw go: `git config --global user.email "…@users.noreply.github.com"`, popraw ostatni commit: `git commit --amend --reset-author --no-edit` |
| Utknąłem w edytorze vim (`~` na ekranie) | `:q!` + Enter (wyjście bez zapisu); `:wq` + Enter (zapisz i wyjdź) |
| `git merge` zrobił bałagan, chcę się wycofać | `git merge --abort`: wraca do stanu sprzed scalania |
