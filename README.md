# Projektowanie aplikacji webowych – materiały do laboratoriów

**SWPS, Psychologia i informatyka, rok 2, semestr zimowy 2026/2027**
Prowadzący: mgr inż. Krzysztof Ropiak

Tutaj znajdziesz instrukcje do laboratoriów, zadania, ściągawki i slajdy.
Kod aplikacji, którą piszemy razem na zajęciach, jest w osobnym repozytorium:
👉 [kropiak/swps_aplikacje_www_labs](https://github.com/kropiak/swps_aplikacje_www_labs)

Slajdy w wersji online: 👉 [kropiak.github.io/swps_aplikacje_www_materialy](https://kropiak.github.io/swps_aplikacje_www_materialy/)

---

## Co budujemy?

Przez cały semestr rozwijamy jedną aplikację: **system rezerwacji konsultacji psychologicznych**.
Pacjent może przejrzeć listę specjalistów, sprawdzić wolne terminy, zarezerwować wizytę i wystawić opinię.
Wszystkie dane są fikcyjne.

Równolegle tworzysz **własny projekt** na wybrany temat. Po każdym labie dodajesz do niego podobną funkcjonalność
(część B zadań).

## Plan zajęć

| # | Forma | Temat | Instrukcja | Zadania | Slajdy | Kod po labie |
|---|---|---|---|---|---|---|
| 1 | online | Środowisko pracy: terminal, Python, venv, pip, VS Code, git | [instrukcja](labs/lab-01/README.md) | [zadania](labs/lab-01/zadania.md) | [slajdy](https://kropiak.github.io/swps_aplikacje_www_materialy/slides/lab-01.html) | [`lab-01`](https://github.com/kropiak/swps_aplikacje_www_labs/tree/lab-01) |
| 2 | online | Git i GitHub: terminal i VS Code | wkrótce | | | `lab-02` |
| 3 | online | Start z Django: projekt, aplikacja, modele, ORM, admin | wkrótce | | | `lab-03` |
| 4 | online | Model domeny, panel admina, widoki, routing | wkrótce | | | `lab-04` |
| 5 | online | HTML, CSS (Tailwind), szablony Django, formularz | wkrótce | | | `lab-05` |
| 6 | online | REST API: Django REST Framework, OpenAPI, Swagger | wkrótce | | | `lab-06` |
| 7 | online | Użytkownicy i bezpieczeństwo: logowanie, tokeny, uprawnienia | wkrótce | | | `lab-07` |
| 8 | pracownia | Konsultacje projektów, pomoc przy błędach | – | – | – | – |
| 9 | pracownia | Prezentacje projektów | – | – | – | – |

## Ściągawki

- [Terminal: PowerShell (Windows) i zsh (macOS)](sciagawki/terminal.md)
- [Python: venv i pip](sciagawki/venv-pip.md)
- [Git](sciagawki/git.md)
- [Django: polecenia `manage.py`](sciagawki/django.md)

## Jak korzystać z repozytorium z kodem?

Repozytorium [`swps_aplikacje_www_labs`](https://github.com/kropiak/swps_aplikacje_www_labs) ma osobną gałąź dla każdego labu.
Gałąź `lab-04` zawiera kod w takim stanie, w jakim był **na koniec** labu 4.

Aplikację piszesz **we własnym repozytorium**. Repozytorium kursowe służy do porównania kodu albo skopiowania brakujących plików,
gdy coś nie działa lub opuściłeś zajęcia:

- na GitHubie wybierz gałąź (przycisk z nazwą gałęzi nad listą plików) i przeglądaj pliki,
- albo pobierz całą gałąź: **Code → Download ZIP**,
- albo w terminalu: `git clone -b lab-04 https://github.com/kropiak/swps_aplikacje_www_labs.git lab-04`.

> Masz problem z instalacją na swoim komputerze? W repozytorium z kodem jest konfiguracja **GitHub Codespaces**.
> Kliknij **Code → Codespaces → Create codespace** i pracuj w przeglądarce. Instrukcja jest w README tamtego repozytorium.

## Literatura

- [Oficjalna dokumentacja Django](https://docs.djangoproject.com/pl/)
- [Dokumentacja Django REST Framework](https://www.django-rest-framework.org/)
- Antonio Melé, *Django 5. Praktyczne tworzenie aplikacji internetowych w Pythonie*, Helion 2025
