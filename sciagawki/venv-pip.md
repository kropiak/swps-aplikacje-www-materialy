# Ściągawka: środowisko wirtualne (venv) i pip

**Środowisko wirtualne** (`venv`) to osobny „pojemnik” na biblioteki jednego projektu.
Dzięki niemu Django w wersji X dla jednego projektu nie gryzie się z wersją Y dla innego.
**pip** to program, który instaluje biblioteki z [PyPI](https://pypi.org/).

> Zasada: **jeden projekt = jeden folder = jedno środowisko `.venv`** w tym folderze.

## Cykl pracy

| Krok | Windows (PowerShell) | macOS (zsh) |
|---|---|---|
| 1. Utwórz venv (raz na projekt) | `python -m venv .venv` | `python3 -m venv .venv` |
| 2. Aktywuj (w każdym nowym terminalu) | `.venv\Scripts\Activate.ps1` | `source .venv/bin/activate` |
| 3. Sprawdź, że działa | `(.venv)` na początku linii | `(.venv)` na początku linii |
| 4. Pracuj | `python ...`, `pip ...` | `python ...`, `pip ...` |
| 5. Dezaktywuj (opcjonalnie) | `deactivate` | `deactivate` |

> 💡 VS Code, jeśli wybierzesz interpreter z `.venv` (**Ctrl/Cmd + Shift + P → Python: Select Interpreter**), sam aktywuje środowisko w każdym nowym terminalu.

## pip

| Co chcę zrobić | Polecenie |
|---|---|
| Zainstaluj bibliotekę | `pip install django` |
| Zainstaluj konkretną wersję | `pip install "django==6.1.*"` |
| Lista zainstalowanych | `pip list` |
| Informacje o bibliotece | `pip show django` |
| Odinstaluj | `pip uninstall django` |
| Zapisz listę zależności do pliku | `pip freeze > requirements.txt` |
| Zainstaluj wszystko z pliku | `pip install -r requirements.txt` |
| Zaktualizuj samego pip | `python -m pip install --upgrade pip` |

## Plik `requirements.txt`

To lista bibliotek potrzebnych w projekcie. Trzymamy go w repozytorium git.
**Folderu `.venv` do repozytorium NIE wrzucamy** (wpisujemy go do `.gitignore`).
Kolega po sklonowaniu projektu odtworzy środowisko poleceniem `pip install -r requirements.txt`.

```text
Django==6.1.2
rich==15.0.0
```

## Typowe problemy

| Objaw | Rozwiązanie |
|---|---|
| `No module named django` | Środowisko nie jest aktywne albo biblioteka jest zainstalowana w innym środowisku. Aktywuj `.venv` i zainstaluj ponownie. |
| `pip` instaluje „gdzieś indziej” | Używaj `python -m pip install ...`: wtedy pip na pewno należy do aktywnego Pythona. |
| Windows: błąd przy `Activate.ps1` | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`, potem otwórz nowy terminal. |
| Przeniosłem/zmieniłem nazwę folderu projektu i venv przestał działać | Usuń `.venv` i utwórz od nowa, potem `pip install -r requirements.txt`. |
| VS Code podkreśla importy na żółto/czerwono | Wybierz właściwy interpreter: **Python: Select Interpreter** → `.venv`. |
