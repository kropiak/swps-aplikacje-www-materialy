# Lab 1: zadania

Zadania nie są oceniane osobno, ale bez nich trudno będzie nadążyć na kolejnych zajęciach,
a część B to fundament Twojego projektu zaliczeniowego.

---

## Część A: ćwiczenia z labu

### A1. Spacer po terminalu
Używając **wyłącznie terminala**, w folderze `projekty_www` utwórz strukturę:

```text
cwiczenia/
├── notatki/
│   └── lab1.txt
└── skrypty/
```

Następnie wyświetl zawartość folderu `cwiczenia`, przejdź do `skrypty`, wypisz bieżącą ścieżkę (`pwd`) i wróć do `projekty_www`.
Na koniec usuń folder `cwiczenia` z całą zawartością.
💡 Polecenia znajdziesz w [ściągawce terminala](../../sciagawki/terminal.md).

### A2. Drugie środowisko
W folderze `projekty_www/cwiczenia_venv` utwórz **nowe** środowisko wirtualne, aktywuj je i sprawdź `pip list`.
Czy jest tam `rich`? Dlaczego nie? Zainstaluj w nim bibliotekę `cowsay` i uruchom:

```bash
python -c "import cowsay; cowsay.cow('Muu! Działa!')"
```

### A3. Odtwarzanie środowiska
W folderze `konsultacje` **usuń** folder `.venv` (🪟 `Remove-Item -Recurse .venv`, 🍎 `rm -r .venv`).
Utwórz środowisko od nowa, aktywuj je i odtwórz biblioteki z `requirements.txt`. Sprawdź, że `python hello.py` działa.

### A4. Rozbudowa skryptu + commity
W `hello.py`:
1. dodaj kolumnę **„Cena (zł)”** z ceną konsultacji każdego specjalisty,
2. pod tabelą wypisz średnią cenę konsultacji.

Każdy z tych dwóch kroków zapisz jako **osobny commit** z sensownym opisem. Sprawdź `git log --oneline`.

### A5. ⭐ Trochę poza labem: `git diff --staged` i `git restore`
Na labie poznaliśmy `git diff`. Przeczytaj w dokumentacji
[git diff](https://git-scm.com/docs/git-diff) czym różni się `git diff` od `git diff --staged`, a w
[git restore](https://git-scm.com/docs/git-restore) jak cofnąć zmianę w pliku.

Przykład:
```bash
# zmieniam plik i dodaję go do poczekalni
git add hello.py
git diff            # pusto! zmiana jest już w poczekalni
git diff --staged   # tu ją widać
# rozmyśliłem się:
git restore --staged hello.py   # wyjmij z poczekalni (zmiana zostaje w pliku)
git restore hello.py            # porzuć zmianę w pliku (UWAGA: nieodwracalne)
```

**Zadanie:** celowo „zepsuj” `hello.py` (np. usuń połowę kodu), zapisz, sprawdź `git diff`, a potem przywróć plik do stanu z ostatniego commita.

---

## Część B: Twój projekt

1. W folderze `projekty_www` utwórz folder na swój projekt zaliczeniowy (nazwa bez spacji i polskich znaków, np. `moj_projekt`).
2. Utwórz w nim środowisko `.venv`, plik `.gitignore` (jak na labie) i `README.md`.
3. W `README.md` zapisz roboczo 2–3 pomysły na temat aplikacji. Na labie 2 wybierzesz jeden i zgłosisz go prowadzącemu.
   Myśl o aplikacji, w której jest **4–6 rodzajów „rzeczy”** powiązanych ze sobą, np.:
   - *biblioteka gier planszowych*: gry, kategorie, wypożyczenia, recenzje,
   - *planer treningów*: ćwiczenia, plany, sesje treningowe, wyniki,
   - *schronisko dla zwierząt*: zwierzęta, gatunki, opiekunowie, adopcje, wizyty.
4. `git init` i pierwszy commit.
5. Załóż konto na [GitHubie](https://github.com) (jeśli go nie masz) i rozważ wniosek o [GitHub Education](https://education.github.com/pack).
