# Ściągawka: terminal na Windows i macOS

Na **Windows** używamy **PowerShell**: w VS Code to domyślny terminal, poza nim otwórz menu Start i wpisz „Terminal”.
Na **macOS** używamy **zsh** (aplikacja *Terminal* albo terminal w VS Code).

> 💡 W VS Code terminal otworzysz skrótem **Ctrl + `** (Windows) / **Cmd + `** (macOS) albo z menu *Terminal → New Terminal*.

## Podstawowe polecenia

| Co chcę zrobić | Windows (PowerShell) | macOS (zsh) |
|---|---|---|
| Gdzie jestem? | `pwd` | `pwd` |
| Co jest w tym folderze? | `ls` (lub `dir`) | `ls` (ukryte pliki: `ls -a`) |
| Wejdź do folderu | `cd nazwa_folderu` | `cd nazwa_folderu` |
| Wyjdź poziom wyżej | `cd ..` | `cd ..` |
| Folder domowy | `cd ~` | `cd ~` |
| Utwórz folder | `mkdir nazwa` | `mkdir nazwa` |
| Utwórz pusty plik | `New-Item plik.txt` | `touch plik.txt` |
| Wyświetl zawartość pliku | `cat plik.txt` | `cat plik.txt` |
| Usuń plik | `Remove-Item plik.txt` (lub `rm`) | `rm plik.txt` |
| Usuń folder z zawartością | `Remove-Item -Recurse folder` | `rm -r folder` |
| Wyczyść ekran | `cls` (lub `clear`) | `clear` |
| Otwórz bieżący folder w VS Code | `code .` | `code .` |
| Otwórz folder w eksploratorze / Finderze | `explorer .` | `open .` |

## Ścieżki

| | Windows | macOS |
|---|---|---|
| Separator | `\` (PowerShell przyjmie też `/`) | `/` |
| Przykład ścieżki | `C:\Users\anna\projekty` | `/Users/anna/projekty` |
| Folder domowy | `~` = `C:\Users\anna` | `~` = `/Users/anna` |
| Bieżący folder | `.` | `.` |
| Folder nadrzędny | `..` | `..` |

## Sztuczki, które oszczędzają czas

- **Tab** podpowiada i uzupełnia nazwy plików i folderów. Używaj go cały czas!
- **Strzałka w górę** przywołuje poprzednie polecenia.
- **Ctrl + C** przerywa działający program (np. serwer Django). Na macOS też **Ctrl**, nie Cmd!
- Nazwy folderów ze spacjami bierz w cudzysłów: `cd "Moje projekty"`. Najlepiej w ogóle unikaj spacji i polskich znaków w nazwach folderów z kodem.

## Python: `python` czy `python3`?

| | Windows | macOS |
|---|---|---|
| Uruchom Pythona (poza venv) | `python` lub `py` | `python3` |
| Po aktywacji venv | `python` | `python` |

## Typowe problemy

| Objaw | Przyczyna i rozwiązanie |
|---|---|
| Windows: `python` otwiera Microsoft Store | Ustawienia → Aplikacje → Zaawansowane ustawienia aplikacji → **Aliasy wykonywania aplikacji** → wyłącz `python.exe` i `python3.exe`. Zamknij i otwórz terminal ponownie. |
| Windows: „running scripts is disabled on this system” | Jednorazowo: `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` (potwierdź `T`/`Y`). |
| `code` – polecenie nie znalezione (macOS) | W VS Code: **Cmd + Shift + P** → „Shell Command: Install 'code' command in PATH”. |
| Polecenie nie działa zaraz po instalacji programu | Zamknij **wszystkie** okna terminala i VS Code, otwórz ponownie (terminal musi odczytać nową zmienną PATH). |
