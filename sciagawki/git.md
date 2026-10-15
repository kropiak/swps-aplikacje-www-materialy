# Ściągawka: git

Polecenia są takie same na Windows i macOS.

## Jak myśli git: trzy miejsca

```text
 folder roboczy        poczekalnia (staging)        repozytorium (historia)
 (Twoje pliki)   --git add-->   (co wejdzie     --git commit-->   (zapisane
                                 do commita)                       migawki)
```

## Konfiguracja (raz na komputer)

```bash
git config --global user.name "Imię Nazwisko"
git config --global user.email "twoj@email.pl"     # ten sam e-mail co na GitHubie
git config --global init.defaultBranch main
git config --global core.editor "code --wait"      # VS Code jako edytor gita
# końce linii:
git config --global core.autocrlf true             # Windows
git config --global core.autocrlf input            # macOS
git config --global --list                         # sprawdź ustawienia
```

## Codzienna praca

| Co chcę zrobić | Polecenie |
|---|---|
| Utwórz repozytorium w bieżącym folderze | `git init` |
| Co się zmieniło? | `git status` |
| Pokaż zmiany w plikach (jeszcze nie w poczekalni) | `git diff` |
| Dodaj plik do poczekalni | `git add plik.py` |
| Dodaj wszystkie zmiany | `git add .` |
| Zapisz migawkę (commit) | `git commit -m "Krótki opis zmiany"` |
| Historia | `git log --oneline` |
| Cofnij niezapisane zmiany w pliku | `git restore plik.py` |
| Wyjmij plik z poczekalni | `git restore --staged plik.py` |

## Gałęzie

| Co chcę zrobić | Polecenie |
|---|---|
| Lista gałęzi | `git branch` |
| Utwórz gałąź i przejdź na nią | `git switch -c nowa-funkcja` |
| Przejdź na istniejącą gałąź | `git switch main` |
| Scal gałąź do bieżącej | `git merge nowa-funkcja` |
| Usuń scaloną gałąź | `git branch -d nowa-funkcja` |

## GitHub (repozytorium zdalne)

| Co chcę zrobić | Polecenie |
|---|---|
| Sklonuj repozytorium | `git clone https://github.com/uzytkownik/repo.git` |
| Sklonuj konkretną gałąź | `git clone -b lab-03 https://github.com/uzytkownik/repo.git` |
| Podłącz lokalne repo do GitHuba | `git remote add origin https://github.com/uzytkownik/repo.git` |
| Pierwsze wysłanie | `git push -u origin main` |
| Wyślij commity | `git push` |
| Pobierz i scal zmiany z GitHuba | `git pull` |
| Pokaż podłączone repozytoria zdalne | `git remote -v` |

## Dobry commit

- Mały i dotyczy jednej rzeczy.
- Opis w trybie rozkazującym albo opisowym: `Dodaj model Specialist`, `Popraw literówkę w README`.
- Commituj często. Commit to punkt zapisu, do którego zawsze możesz wrócić.

## Typowy `.gitignore` dla projektu Django

```gitignore
.venv/
__pycache__/
*.pyc
db.sqlite3
.env
.DS_Store
media/
```

## Ratunku!

| Sytuacja | Co zrobić |
|---|---|
| Otworzył się dziwny edytor (vim) przy commicie | Wpisz `:q!` i Enter, potem commituj z `-m "opis"` |
| `Please tell me who you are` | Skonfiguruj `user.name` i `user.email` (patrz wyżej) |
| Konflikt przy `merge`/`pull` | Otwórz plik w VS Code, wybierz *Accept Current / Incoming / Both*, zapisz, `git add .`, `git commit` |
| `rejected ... fetch first` przy `push` | Ktoś (albo Ty z innego komputera) wysłał zmiany: `git pull`, potem `git push` |
| Wrzuciłem `.venv` do repo | Dopisz `.venv/` do `.gitignore`, `git rm -r --cached .venv`, commit |
