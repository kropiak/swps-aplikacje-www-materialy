# Lab 2: zadania

---

## Część A: ćwiczenia z labu

### A1. Gałąź eksperymentalna, której nie scalamy
W repozytorium `konsultacje` utwórz gałąź `eksperyment`, zrób na niej 2 commity, które psują `hello.py` (np. usuń tabelę).
Wróć na `main` i sprawdź, że skrypt działa. Usuń gałąź **bez scalania**.
Zwykłe `git branch -d` odmówi. Dlaczego? Przeczytaj komunikat i użyj opcji, którą proponuje git.

### A2. Konflikt w dwóch miejscach naraz
Wywołaj konflikt, w którym na dwóch gałęziach zmieniasz **dwie różne linie** `hello.py` (np. nazwisko jednego specjalisty i tytuł tabeli)
w różny sposób na obu gałęziach. Rozwiąż go w **Merge Editorze** VS Code: przy jednej zmianie wybierz wersję z jednej gałęzi, przy drugiej z drugiej.
Wyślij wynik na GitHuba.

### A3. Zmiany w kilku plikach, commit wybiórczy
Zmień jednocześnie `hello.py` (dodaj specjalistę) i `README.md` (dopisz funkcję do planu).
W panelu Source Control zrób z tego **dwa osobne commity**: najpierw tylko `hello.py`, potem tylko `README.md`.
Zrób to samo w terminalu przy kolejnej zmianie.

### A4. Pull request z poprawką
Utwórz gałąź `opis-uruchomienia`, rozbuduj w `README.md` sekcję *Uruchomienie*: osobne polecenia dla Windows i macOS
w blokach kodu (<code>```powershell</code>, <code>```bash</code>). Wyślij gałąź, otwórz PR, w zakładce *Files changed*
dodaj do jednej linii komentarz, scal PR i zsynchronizuj `main` lokalnie.

### A5. ⭐ Trochę poza labem: `git stash`
Zdarza się, że jesteś w połowie zmian, a musisz pilnie przełączyć się na inną gałąź lub zrobić `pull`.
Git nie zawsze pozwoli przełączyć gałąź z niezapisanymi zmianami. Ratunkiem jest **schowek**:
[git stash, dokumentacja](https://git-scm.com/docs/git-stash) · [rozdział w Pro Git (PL/EN)](https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning).

```bash
git stash            # odłóż niezapisane zmiany na bok (folder wraca do stanu z ostatniego commita)
git switch inna-galaz
# ... pilna praca, commit ...
git switch main
git stash list       # co jest w schowku?
git stash pop        # przywróć odłożone zmiany
```

**Zadanie:** zacznij edytować `hello.py` (bez commita). Odłóż zmiany do schowka, wykonaj `git pull`, przywróć zmiany i dokończ commit.
Znajdź też w VS Code, gdzie są polecenia *Stash* (podpowiedź: menu `…` w panelu Source Control).

---

## Część B: Twój projekt

1. Wyślij repozytorium projektu na GitHuba (sekcja *Repozytorium projektu zaliczeniowego* w instrukcji).
   Jeśli repozytorium jest prywatne, dodaj prowadzącego (`kropiak`) jako współpracownika.
2. Wybierz **jeden** temat i opisz go w `README.md` projektu:
   - **tytuł i 2–3 zdania opisu:** co robi aplikacja i dla kogo,
   - **lista „rzeczy”** w aplikacji (4–6), opisana zwykłym językiem, np.:
     > *Gra planszowa: ma tytuł, liczbę graczy, czas gry i kategorię.
     > Kategoria: nazwa (np. strategiczne, imprezowe).
     > Wypożyczenie: kto pożyczył którą grę i kiedy ją oddał.
     > Recenzja: ocena i komentarz do gry.*
   - **powiązania** między nimi, np. *„gra należy do jednej kategorii”, „gra może mieć wiele recenzji”*,
   - **planowane funkcje** (lista punktowana).

   Na labach 3–4 zamienimy te opisy na modele Django, więc nie przejmuj się na razie szczegółami technicznymi.
3. Pracuj w gałęzi `opis-projektu` i scal ją przez **pull request**.
4. **Zgłoś temat:** prześlij prowadzącemu link do repozytorium w sposób podany na zajęciach.
