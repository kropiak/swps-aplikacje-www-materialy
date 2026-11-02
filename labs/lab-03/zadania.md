# Lab 3: zadania

---

## Część A: ćwiczenia z labu

### A1. Rozbudowa bazy specjalistów w `/admin/` i `shell`
Upewnij się, że w Twojej bazie są wszystkie 4 specjalizacje i 4 specjaliści z labu, a następnie:
1. dodaj jeszcze **2 specjalizacje** (np. *Interwencja kryzysowa*, *Terapia uzależnień*),
2. dodaj **2 nowych specjalistów** (jednego pracującego w trybie `hybrydowo`, drugiego `online`) o różnych cenach wizyty,
3. przypisz przynajmniej dwóm specjalistom po **2 lub 3 specjalizacje**.

### A2. Trening zapytań ORM w `manage.py shell`
Uruchom `python manage.py shell` i przetestuj zapytania ORM (podpowiedzi znajdziesz w [ściągawce Django](../../sciagawki/django.md)):
1. wypisz wszystkich specjalistów pracujących stacjonarnie (`mode="stacjonarnie"`),
2. wypisz specjalistów, których cena wizyty jest mniejsza niż 200 zł (`price__lt=200`),
3. wypisz specjalistów posortowanych od najdroższego do najtańszego (`order_by("-price")`),
4. pobierz jedną specjalizację (np. `pary = Specialization.objects.get(name="Terapia par")`) i wypisz wszystkich specjalistów, którzy ją posiadają (`pary.specialists.all()`).

### A3. Polskie nazwy w panelu admina (`verbose_name` i `class Meta`)
Domyślnie w panelu `/admin/` Django wyświetla angielskie nazwy klas i pól (*Specialists*, *First name*, *Price*).
Możesz to łatwo zmienić w `booking/models.py`:
1. dodaj argument `verbose_name` do pól modelu (np. `first_name = models.CharField("Imię", max_length=50)`),
2. wewnątrz klas `Specialization` i `Specialist` dodaj podklasę `Meta` z polskimi nazwami w liczbie pojedynczej i mnogiej oraz domyślnym sortowaniem:

   ```python
   class Meta:
       verbose_name = "Specjalista"
       verbose_name_plural = "Specjaliści"
       ordering = ["last_name", "first_name"]
   ```

3. wykonaj `python manage.py makemigrations` oraz `python manage.py migrate` i obejrzyj efekt w `/admin/`.

### A4. Zmiana na gałęzi i Pull Request
Wykonaj zmiany z zadania **A3** zgodnie z przepływem pracy z labu 2:
1. utwórz nową gałąź `polskie-nazwy-modeli`,
2. zatwierdź zmiany w `booking/models.py` i nowo wygenerowany plik migracji w `booking/migrations/`,
3. wyślij gałąź na GitHuba, otwórz **Pull Request**, przejrzyj zakładkę *Files changed*, scal PR do `main` i zsynchronizuj lokalne repozytorium (`git pull`).

### A5. ⭐ Trochę poza labem: agregacje ORM (`Avg`, `Min`, `Max`)
W zadaniu A4 z labu 1 liczyliśmy średnią cenę konsultacji w pętli Pythona.
Gdy dane leżą w bazie danych, dużo szybciej policzy to sama baza za pomocą funkcji agregujących:
[Agregacje w dokumentacji Django 6.0](https://docs.djangoproject.com/pl/6.0/topics/db/aggregation/).

```python
from django.db.models import Avg, Count, Max, Min

Specialist.objects.aggregate(
    srednia_cena=Avg("price"),
    najtansza=Min("price"),
    najdrozsza=Max("price"),
)
```

**Zadanie:** uruchom powyższe zapytanie w `python manage.py shell`, a następnie sprawdź w dokumentacji, jak za pomocą `annotate(liczba=Count("specialists"))` wypisać przy każdej specjalizacji liczbę przypisanych do niej specjalistów.

---

## Część B: Twój projekt

W tej części stawiasz fundament swojego projektu zaliczeniowego (odpowiada za pierwszą połowę obszaru **2. Model domeny i panel admina – 25 pkt**):

1. Otwórz folder swojego projektu zaliczeniowego w VS Code, aktywuj `.venv`, wykonaj `git pull` i utwórz nową gałąź `start-django`.
2. Zainstaluj Django 6 (`pip install "django>=6.0,<7.0"`), zaktualizuj `requirements.txt`, utwórz projekt `config` (`django-admin startproject config .`) oraz aplikację o nazwie pasującej do Twojego tematu (np. `catalog`, `workouts`, `shelter`, `events`).
3. W `config/settings.py` dodaj swoją aplikację do `INSTALLED_APPS` oraz ustaw `LANGUAGE_CODE = "pl"` i `TIME_ZONE = "Europe/Warsaw"`.
4. Na podstawie opisu z labu 2 zdefiniuj w `models.py` **2 pierwsze powiązane ze sobą modele** (z relacją `ForeignKey` lub `ManyToManyField` oraz metodą `__str__`), np.:
   - *Kategoria* i *Gra planszowa*,
   - *Gatunek* i *Zwierzę w schronisku*,
   - *Partia mięśniowa* i *Ćwiczenie*.
   *(Pozostałe 2–4 modele z opisu dodamy na labie 4).*
5. Wykonaj `python manage.py makemigrations` i `python manage.py migrate`, zarejestruj oba modele w `admin.py`, utwórz konto `superuser` i dodaj w `/admin/` po kilka przykładowych rekordów (pamiętaj, że `db.sqlite3` zostaje tylko u Ciebie lokalnie!).
6. Zaktualizuj `README.md` swojego projektu (instrukcja uruchomienia z `migrate` i `runserver`), wyślij gałąź `start-django` na GitHuba i scal ją z `main` przez **Pull Request**.
