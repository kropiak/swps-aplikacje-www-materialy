# Lab 1: Środowisko pracy

**Cel:** po tych zajęciach masz na swoim komputerze działający zestaw narzędzi programisty:
terminal, Python ze środowiskiem wirtualnym, VS Code i git. Masz też pierwszy projekt z kilkoma commitami.

**Kod po labie:** gałąź [`lab-01`](https://github.com/kropiak/swps-aplikacje-www-labs/tree/lab-01)
**Slajdy:** [lab-01](https://kropiak.github.io/swps-aplikacje-www-materialy/slides/lab-01.html)
**Ściągawki:** [terminal](../../sciagawki/terminal.md) · [venv i pip](../../sciagawki/venv-pip.md) · [git](../../sciagawki/git.md)

| Część | Czas | Temat |
|---|---|---|
| 1 | 45 min | Terminal bez strachu, instalacja Pythona |
| 2 | 45 min | Środowisko wirtualne, pip, pierwszy skrypt |
| 3 | 45 min | VS Code, instalacja i konfiguracja gita, pierwsze commity |

> 📌 W instrukcji polecenia dla obu systemów są oznaczone tak:
> 💻 **Windows** (PowerShell) · 🍎 **macOS** (zsh)
> Jeśli przy poleceniu nie ma oznaczenia, działa tak samo na obu systemach.

---

## Część 1: Terminal i Python

### 1.1. Dlaczego terminal?

Większość narzędzi programisty (Python, pip, git, Django) najwygodniej uruchamia się poleceniami tekstowymi.
Terminal to po prostu okno, w którym wpisujesz polecenie, naciskasz Enter i czytasz odpowiedź.
Nic nie zepsujesz, jeśli będziesz uważnie czytać, co wpisujesz 🙂

Otwórz terminal:

- 💻 menu Start → wpisz **Terminal** (lub **PowerShell**) → Enter
- 🍎 **Cmd + Spacja** → wpisz **Terminal** → Enter

### 1.2. Pierwsze polecenia

Wpisz po kolei i obserwuj wynik:

```bash
pwd             # gdzie jestem?
ls              # co jest w tym folderze?
cd Desktop      # wejdź do folderu Desktop (Pulpit). Na Windows z OneDrive może to być OneDrive\Desktop
cd ..           # wróć poziom wyżej
cd ~            # wróć do folderu domowego
```

> 💡 Naciśnij **Tab** w połowie nazwy folderu, a terminal dokończy ją za Ciebie. Strzałka **w górę** przywołuje poprzednie polecenie.

Utwórz folder na wszystkie projekty z tego przedmiotu i wejdź do niego:

```bash
cd ~
mkdir projekty_www
cd projekty_www
pwd
```

> ⚠️ W nazwach folderów z kodem **unikaj spacji i polskich znaków**. Oszczędzisz sobie wielu dziwnych błędów.

Pełna tabela poleceń dla obu systemów jest w [ściągawce terminala](../../sciagawki/terminal.md).

### 1.3. Instalacja Pythona

Potrzebujemy **Pythona 3.12 lub nowszego** (zalecany 3.13 albo 3.14). Najpierw sprawdź, czy już go masz:

💻 Windows:
```powershell
python --version
```

🍎 macOS:
```bash
python3 --version
```

Jeśli widzisz `Python 3.12.x` lub nowszy, przejdź do punktu 1.4. W przeciwnym razie zainstaluj Pythona:

💻 **Windows**
1. Wejdź na https://www.python.org/downloads/ i pobierz aktualną wersję dla Windows.
2. Uruchom instalator. Jeśli zobaczysz opcję **„Add python.exe to PATH”**, **zaznacz ją** (to najważniejszy krok!).
3. Po instalacji **zamknij wszystkie okna terminala** i otwórz nowe.
4. Sprawdź: `python --version`.

> Jeśli `python` otwiera Microsoft Store: Ustawienia → Aplikacje → Zaawansowane ustawienia aplikacji →
> **Aliasy wykonywania aplikacji** → wyłącz `python.exe` i `python3.exe`.

🍎 **macOS**
1. Wejdź na https://www.python.org/downloads/ i pobierz instalator dla macOS (plik `.pkg`).
2. Zainstaluj (Dalej, Dalej, …). Po instalacji w Finderze otworzy się folder Pythona; możesz kliknąć dwukrotnie `Install Certificates.command`.
3. Otwórz nowy terminal i sprawdź: `python3 --version`.

> Na macOS polecenie `python` (bez trójki) może nie istnieć. Do czasu aktywacji środowiska wirtualnego używaj `python3`.

### 1.4. Python w trybie interaktywnym

```bash
python      # 💻
python3     # 🍎
```

Pojawi się znak zachęty `>>>`. Wpisz:

```python
>>> 2 + 2
>>> imie = "Ala"
>>> print(f"Cześć, {imie}!")
>>> exit()
```

✅ **Punkt kontrolny 1:** masz folder `projekty_www`, a `python --version` (💻) lub `python3 --version` (🍎) pokazuje wersję 3.12+.

---

## Część 2: Środowisko wirtualne i pip

### 2.1. Po co środowisko wirtualne?

Każdy projekt potrzebuje swoich bibliotek, często w konkretnych wersjach.
Gdyby instalować wszystko „globalnie”, projekty zaczęłyby sobie przeszkadzać.
**Środowisko wirtualne (`venv`)** to osobny folder z prywatną kopią Pythona i bibliotek dla jednego projektu.

> Zasada na cały semestr: **jeden projekt = jeden folder = jedno środowisko `.venv`** w tym folderze.

### 2.2. Tworzymy projekt i środowisko

W tym folderze przez cały semestr będziemy budować aplikację do rezerwacji konsultacji.

```bash
cd ~/projekty_www
mkdir konsultacje
cd konsultacje
```

Utwórz środowisko wirtualne:

💻 `python -m venv .venv`
🍎 `python3 -m venv .venv`

Aktywuj je:

💻 Windows:
```powershell
.venv\Scripts\Activate.ps1
```
Jeśli pojawi się błąd *„running scripts is disabled on this system”*, wykonaj **jednorazowo**
`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, potwierdź i spróbuj ponownie.

🍎 macOS:
```bash
source .venv/bin/activate
```

Po aktywacji na początku linii terminala pojawi się `(.venv)`. Od teraz na **obu** systemach piszemy po prostu `python`:

```bash
python --version
python -c "import sys; print(sys.prefix)"   # ścieżka powinna kończyć się na .venv
```

### 2.3. Instalujemy bibliotekę przez pip

Zainstalujemy [**rich**](https://rich.readthedocs.io/), bibliotekę do kolorowego wypisywania tekstu i tabel w terminalu:

```bash
python -m pip install --upgrade pip
pip install rich
pip list
```

### 2.4. Pierwszy skrypt

Utwórz w folderze `konsultacje` plik `hello.py`. Na razie może to być dowolny edytor; za chwilę przejdziemy do VS Code.

```python
"""Skrypt powitalny: sprawdza, czy środowisko działa."""

import sys

from rich.console import Console
from rich.table import Table

console = Console()

SPECIALISTS = [
    ("Anna", "Nowak", "Terapia poznawczo-behawioralna", "online"),
    ("Piotr", "Wiśniewski", "Psychologia dziecięca", "stacjonarnie"),
    ("Maria", "Zielińska", "Terapia par", "online"),
]


def main() -> None:
    in_venv = sys.prefix != sys.base_prefix
    console.print(f"[bold green]Działa![/] Python {sys.version.split()[0]}")
    if in_venv:
        console.print("Środowisko wirtualne: [green]aktywne[/] ✅")
    else:
        console.print("Środowisko wirtualne: [red]nieaktywne[/] ❌")

    table = Table(title="Nasi specjaliści (dane fikcyjne)")
    table.add_column("Imię")
    table.add_column("Nazwisko")
    table.add_column("Specjalizacja")
    table.add_column("Forma", style="cyan")
    for first_name, last_name, specialization, mode in SPECIALISTS:
        table.add_row(first_name, last_name, specialization, mode)

    console.print(table)


if __name__ == "__main__":
    main()
```

Uruchom:

```bash
python hello.py
```

### 2.5. `requirements.txt`: lista zależności

Zapisz, jakich bibliotek używa projekt:

```bash
pip freeze > requirements.txt
cat requirements.txt
```

Dzięki temu plikowi każdy (np. Ty na innym komputerze) odtworzy środowisko jednym poleceniem
`pip install -r requirements.txt`. Folder `.venv` jest „jednorazowy”: zawsze można go usunąć i utworzyć od nowa.

✅ **Punkt kontrolny 2:** `python hello.py` wyświetla tabelę i komunikat „Środowisko wirtualne: aktywne ✅”.

---

## Część 3: VS Code i git

### 3.1. Instalacja i konfiguracja VS Code

1. Pobierz i zainstaluj VS Code: https://code.visualstudio.com/
   - 💻 w instalatorze zaznacz **„Add to PATH”** i **„Open with Code”** (menu kontekstowe).
   - 🍎 przeciągnij aplikację do folderu **Programy (Applications)**, uruchom ją i wykonaj
     **Cmd + Shift + P** → `Shell Command: Install 'code' command in PATH`.
2. Zainstaluj rozszerzenia (ikona klocków po lewej, **Ctrl/Cmd + Shift + X**):
   - **Python** (Microsoft): zawiera też Pylance i debugger,
   - opcjonalnie **Polish Language Pack** (my będziemy używać angielskiego interfejsu, tak jak w instrukcjach).
3. Otwórz projekt: w terminalu w folderze `konsultacje` wpisz `code .`
   (albo w VS Code: *File → Open Folder…*).
4. Wskaż interpreter z naszego środowiska: **Ctrl/Cmd + Shift + P** → `Python: Select Interpreter` → wybierz pozycję z `.venv`.
5. Otwórz terminal w VS Code: **Ctrl + `** (💻) / **Cmd + `** (🍎). Środowisko `(.venv)` powinno aktywować się samo.
6. Otwórz `hello.py` i uruchom go przyciskiem ▶ w prawym górnym rogu.

> 💡 Zmień coś w `hello.py`, np. dodaj czwartego specjalistę, i uruchom ponownie.

### 3.2. Czym jest git?

**Git** zapisuje historię zmian w projekcie jako serię **migawek (commitów)**.
Do każdej migawki możesz wrócić, porównać ją z obecnym stanem i zobaczyć, kto, kiedy i dlaczego coś zmienił.
To jak *Historia wersji* w Google Docs, tylko pod Twoją pełną kontrolą.
**GitHub** to serwis, w którym trzymamy kopię repozytorium w internecie. Zajmiemy się nim na labie 2.

### 3.3. Instalacja gita

Sprawdź, czy git jest zainstalowany: `git --version`.

💻 **Windows:** pobierz *Git for Windows* z https://git-scm.com/downloads/win i zainstaluj. Większość opcji zostaw domyślnych, ale:
- *Choosing the default editor used by Git* → **Use Visual Studio Code as Git's default editor**,
- *Adjusting the name of the initial branch* → **Override… → `main`**.

Po instalacji zamknij i otwórz ponownie VS Code i terminal.

🍎 **macOS:** wpisz w terminalu `git --version`. Jeśli gita nie ma, system zaproponuje instalację
**Command Line Developer Tools**. Zgódź się i poczekaj kilka minut. Możesz też wymusić instalację: `xcode-select --install`.

### 3.4. Konfiguracja gita (raz na komputer)

Użyj **tego samego adresu e-mail**, którego użyjesz (lub używasz) na GitHubie:

```bash
git config --global user.name "Imię Nazwisko"
git config --global user.email "twoj@email.pl"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
```

Końce linii (Windows i macOS zapisują je inaczej, to ustawienie zapobiega bałaganowi):

💻 `git config --global core.autocrlf true`
🍎 `git config --global core.autocrlf input`

Sprawdź: `git config --global --list`.

### 3.5. Pierwsze repozytorium

W terminalu VS Code, w folderze `konsultacje`:

```bash
git init
git status
```

Git widzi **wszystkie** pliki, także folder `.venv` z tysiącami plików bibliotek. Nie chcemy go w historii!
Utwórz plik `.gitignore` z listą rzeczy, które git ma ignorować:

```gitignore
# środowisko wirtualne
.venv/

# pliki tymczasowe Pythona
__pycache__/
*.pyc

# baza danych i sekrety (przydadzą się od labu 3)
db.sqlite3
.env

# śmieci systemowe
.DS_Store
Thumbs.db
```

```bash
git status          # .venv zniknął z listy 🎉
```

Pierwszy commit:

```bash
git add .gitignore
git commit -m "Dodaj .gitignore"

git add hello.py requirements.txt
git commit -m "Dodaj skrypt powitalny i listę zależności"

git log --oneline
```

### 3.6. Trzy miejsca w gicie

```text
 folder roboczy  --git add-->  poczekalnia (staging)  --git commit-->  historia
```

Zrób zmianę i prześledź ją:

1. Dodaj nowego specjalistę w `hello.py` i zapisz plik.
2. `git status`: plik jest *modified*.
3. `git diff`: widać dokładnie, co się zmieniło.
4. `git add hello.py`, potem `git status`: plik jest w poczekalni (*Changes to be committed*).
5. `git commit -m "Dodaj specjalistkę Ewę Lis"`.
6. `git log --oneline`: masz już trzy commity.

### 3.7. README

Utwórz plik `README.md`, wizytówkę projektu (na GitHubie wyświetla się na stronie głównej repozytorium):

```markdown
# Konsultacje

Aplikacja do rezerwacji konsultacji psychologicznych, tworzona na zajęciach
„Projektowanie aplikacji webowych” (SWPS).

## Uruchomienie

    python -m venv .venv
    pip install -r requirements.txt
    python hello.py
```

```bash
git add README.md
git commit -m "Dodaj README"
```

> 💡 To samo możesz robić w VS Code w panelu **Source Control** (ikona rozgałęzienia, **Ctrl/Cmd + Shift + G**):
> znak **+** to `git add`, pole tekstowe z przyciskiem **Commit** to `git commit`. Na labie 2 przećwiczymy to dokładniej.

✅ **Punkt kontrolny 3:** `git log --oneline` pokazuje co najmniej 4 commity, a `git status` mówi *nothing to commit, working tree clean*.

---

## Podsumowanie

- [ ] Umiem otworzyć terminal i poruszać się po folderach (`pwd`, `ls`, `cd`, `mkdir`).
- [ ] Mam Pythona 3.12+.
- [ ] Umiem utworzyć i aktywować środowisko `.venv` oraz instalować biblioteki przez `pip`.
- [ ] Wiem, po co jest `requirements.txt` i dlaczego `.venv` nie trafia do gita.
- [ ] Mam VS Code z rozszerzeniem Python i wybranym interpreterem z `.venv`.
- [ ] Mam skonfigurowanego gita i repozytorium z kilkoma commitami.

**Zadania po labie:** [zadania.md](zadania.md)

**Przed labem 2:** załóż konto na https://github.com (jeśli go nie masz), najlepiej na ten sam e-mail co w `git config`.
Studenci mogą też złożyć wniosek o darmowy pakiet [GitHub Education](https://education.github.com/pack)
(m.in. GitHub Copilot i więcej godzin Codespaces). Weryfikacja trwa kilka dni, więc warto zrobić to od razu.
