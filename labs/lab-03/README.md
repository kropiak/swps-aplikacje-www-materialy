# Lab 3: Start z Django

**Cel:** po tych zajęciach Twój folder `konsultacje` jest działającą aplikacją webową w Django 6:
masz uruchomiony serwer deweloperski, bazę danych SQLite z dwoma powiązanymi modelami (`Specialization` i `Specialist`),
umiesz odpytywać bazę przez ORM w konsoli `shell` oraz zarządzać danymi w panelu `/admin/`.

**Kod po labie:** gałąź [`lab-03`](https://github.com/kropiak/swps-aplikacje-www-labs/tree/lab-03)
**Slajdy:** [lab-03](https://kropiak.github.io/swps-aplikacje-www-materialy/slides/lab-03.html)
**Ściągawki:** [django](../../sciagawki/django.md) · [venv i pip](../../sciagawki/venv-pip.md) · [git](../../sciagawki/git.md)

| Część | Czas | Temat |
|---|---|---|
| 1 | 45 min | Instalacja Django, projekt `config`, aplikacja `booking`, serwer `runserver` |
| 2 | 45 min | Modele `Specialization` i `Specialist`, migracje i ORM w `manage.py shell` |
| 3 | 45 min | Panel administracyjny `/admin/`, konto `superuser` i rejestracja modeli |

**Wymagania:** ukończony [lab 2](../lab-02/README.md): folder `konsultacje` ze środowiskiem `.venv`,
połączony z repozytorium na GitHubie.

> 📌 **Zanim zaczniesz:** upewnij się, że pracujesz w terminalu VS Code w folderze `konsultacje`,
> a na początku wiersza poleceń widzisz **`(.venv)`**. Po aktywacji środowiska polecenia są identyczne
> na 💻 Windows i 🍎 macOS (`python`, nie `python3`).

---

## Część 1: Pierwszy projekt i aplikacja Django

### 1.1. Dlaczego `hello.py` już nie wystarcza?

W `hello.py` lista specjalistów była zapisana na sztywno w kodzie i wyświetlała się tylko w Twoim terminalu.
Prawdziwa aplikacja webowa działa inaczej:

```text
Przeglądarka  ──żądanie HTTP (np. GET /)──►  Django (Python)  ◄──►  Baza danych (SQLite)
użytkownika   ◄──odpowiedź (HTML / JSON)───
```

**Django** to framework webowy w Pythonie z filozofią *„baterie w zestawie”* (*batteries included*):
daje gotowy serwer deweloperski, komunikację z bazą danych (ORM), system szablonów, logowanie użytkowników
i gotowy panel administracyjny.

### 1.2. Rozgrzewka i instalacja Django 6

Otwórz folder `konsultacje` w VS Code, uruchom terminal i sprawdź, czy środowisko `.venv` jest aktywne.
Jeśli nie widzisz `(.venv)`:

💻 Windows (PowerShell):
```powershell
.venv\Scripts\Activate.ps1
```

🍎 macOS (zsh):
```bash
source .venv/bin/activate
```

Pobierz ewentualne zmiany z GitHuba i zainstaluj **Django**:

```bash
git pull
pip install django
python -m django --version
```

Powinieneś zobaczyć wersję z serii **`6.x`** (np. `6.1.2`). Zapisz zaktualizowaną listę bibliotek do `requirements.txt`:

```bash
pip freeze > requirements.txt
```

### 1.3. Tworzymy projekt `config` (`startproject`)

W Django rozróżniamy dwa pojęcia:

- **projekt** (`config`): cała witryna i jej główne ustawienia (baza danych, język, główna mapa adresów URL),
- **aplikacja** (`booking`): konkretny moduł tematyczny (u nas: rezerwacja konsultacji).

Utwórz projekt `config` **w bieżącym folderze**:

```bash
django-admin startproject config .
```

> ⚠️ **Kropka `.` na końcu jest bardzo ważna!** Mówi ona Django: „utwórz plik `manage.py` i folder `config/`
> tutaj, w obecnym folderze `konsultacje`, zamiast tworzyć dodatkowy podfolder `config/config/`”.
> Jeśli zapomniałeś kropki, zobacz sekcję *Typowe problemy* na dole instrukcji.

Sprawdź `ls` (lub drzewo plików po lewej w VS Code). Pojawiły się:

```text
konsultacje/
├── config/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py      ← główne ustawienia projektu
│   ├── urls.py          ← mapa adresów URL
│   └── wsgi.py
├── hello.py             ← nasz skrypt z labu 1 (zostawiamy go na pamiątkę)
├── manage.py            ← centrum dowodzenia projektem Django
└── requirements.txt
```

### 1.4. Pierwsze uruchomienie serwera (`runserver`)

Od tej chwili prawie wszystko w Django robimy przez `python manage.py <polecenie>`. Uruchom serwer deweloperski:

```bash
python manage.py runserver
```

W terminalu pojawi się m.in.:

```text
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
```

> Na czerwony komunikat *„You have 18 unapplied migration(s)”* na razie nie zwracaj uwagi: zajmiemy się nim w części 2.

Otwórz w przeglądarce adres **http://127.0.0.1:8000/** (możesz kliknąć link w terminalu z wciśniętym **Ctrl** / **Cmd**).
Widzisz rakietę startową Django? Działa! 🚀

- `127.0.0.1` (czyli `localhost`) to Twój własny komputer, a `8000` to numer portu.
- Dopóki serwer działa, ten terminal jest zajęty i wypisuje każde wejście na stronę.
- Żeby zatrzymać serwer i wrócić do wpisywania poleceń, naciśnij w terminalu **Ctrl + C**
  (albo otwórz drugi terminal przyciskiem **`+`** w prawym górnym rogu panelu terminala VS Code).

Zatrzymaj na chwilę serwer (**Ctrl + C**).

### 1.5. Tworzymy aplikację `booking` (`startapp`)

Utwórz aplikację `booking`:

```bash
python manage.py startapp booking
```

Powstał folder `booking/`, a w nim najważniejsze dla nas dziś pliki:

- `booking/models.py`: tu opiszemy strukturę danych (tabele w bazie),
- `booking/admin.py`: tu włączymy modele w panelu administratora,
- `booking/views.py`: tu w labie 4 napiszemy widoki stron.

### 1.6. Konfiguracja `config/settings.py`

Samo utworzenie folderu `booking` nie wystarczy: trzeba jeszcze powiedzieć projektowi `config`, że taka aplikacja istnieje.
Otwórz plik `config/settings.py`:

1. Znajdź listę `INSTALLED_APPS` i dopisz na końcu `"booking",`:

   ```python
   INSTALLED_APPS = [
       "django.contrib.admin",
       "django.contrib.auth",
       "django.contrib.contenttypes",
       "django.contrib.sessions",
       "django.contrib.messages",
       "django.contrib.staticfiles",
       "booking",
   ]
   ```

2. Niżej w tym samym pliku zmień język i strefę czasową na polskie:

   ```python
   LANGUAGE_CODE = "pl"

   TIME_ZONE = "Europe/Warsaw"
   ```

Zapisz plik, uruchom ponownie `python manage.py runserver` i odśwież **http://127.0.0.1:8000/**.
Strona z rakietą wita Cię teraz po polsku! 🇵🇱

Zatrzymaj serwer (**Ctrl + C**) i zapisz pierwszy krok w gicie:

```bash
git status
git add .
git commit -m "Zainstaluj Django i utwórz projekt config oraz aplikację booking"
```

✅ **Punkt kontrolny 1:** pod adresem http://127.0.0.1:8000/ wyświetla się polska strona powitalna Django,
w `config/settings.py` na liście `INSTALLED_APPS` jest `"booking"`, a `git status` pokazuje czyste drzewo robocze.

---

## Część 2: Modele, migracje i ORM w `shell`

### 2.1. Od listy w `hello.py` do tabel w bazie danych

W `hello.py` trzymaliśmy krotki:

```python
("Anna", "Nowak", "Terapia poznawczo-behawioralna", "online")
```

W prawdziwym systemie jeden specjalista może pracować w **kilku** nurtach (np. terapia poznawczo-behawioralna **i** terapia par),
a jedną specjalizację może mieć **wielu** specjalistów. To klasyczna relacja **wiele-do-wielu** (*many-to-many*).

Zbudujemy dziś dwie pierwsze klasy (modele) w naszej domenie:

```text
┌──────────────────────┐          wiele-do-wielu          ┌──────────────────────┐
│    Specialization    │ ◄──────────────────────────────► │      Specialist      │
├──────────────────────┤     (pole: specializations)      ├──────────────────────┤
│ id (auto)            │                                  │ id (auto)            │
│ name                 │                                  │ first_name           │
└──────────────────────┘                                  │ last_name            │
                                                          │ bio                  │
                                                          │ mode                 │
                                                          │ price                │
                                                          └──────────────────────┘
```

> Na labie 4 dopiszemy do nich kolejne dwa modele: `Appointment` (terminy wizyt) i `Review` (opinie pacjentów).

### 2.2. Piszemy modele w `booking/models.py`

W Django **model** to zwykła klasa Pythona dziedzicząca po `models.Model`.
Każda klasa to osobna tabela w bazie danych, a każde jej pole to kolumna w tej tabeli.

Otwórz plik `booking/models.py` i zastąp jego zawartość poniższym kodem:

```python
from django.db import models


class Specialization(models.Model):
    name = models.CharField(max_length=100, unique=True)

    def __str__(self) -> str:
        return self.name


class Specialist(models.Model):
    MODE_CHOICES = [
        ("online", "Online"),
        ("stacjonarnie", "Stacjonarnie"),
        ("hybrydowo", "Hybrydowo"),
    ]

    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    bio = models.TextField(blank=True)
    mode = models.CharField(max_length=20, choices=MODE_CHOICES, default="online")
    price = models.PositiveIntegerField(default=180)
    specializations = models.ManyToManyField(
        Specialization,
        related_name="specialists",
        blank=True,
    )

    def __str__(self) -> str:
        return f"{self.first_name} {self.last_name}"
```

Co oznaczają poszczególne elementy?

| Element w `models.py` | Znaczenie |
|---|---|
| `CharField(max_length=...)` | krótki tekst o ograniczonej długości (np. imię, nazwisko, nazwa) |
| `TextField(blank=True)` | dłuższy tekst (np. biogram); `blank=True` oznacza, że pole może pozostać puste |
| `PositiveIntegerField(default=180)` | dodatnia liczba całkowita (cena wizyty w zł, domyślnie `180`) |
| `choices=MODE_CHOICES` | ogranicza wartości do podanej listy par `("wartość_w_bazie", "Etykieta dla człowieka")` |
| ` unique=True` | baza nie pozwoli dodać dwóch specjalizacji o tej samej nazwie |
| `ManyToManyField(Specialization, ...)` | relacja wiele-do-wielu; `related_name="specialists"` pozwoli pytać też w drugą stronę: `specjalizacja.specialists.all()` |
| `def __str__(self)` | jak obiekt ma się przedstawiać jako tekst (w konsoli i w panelu admina); bez tego widzielibyśmy bezduszne `Specialist object (1)` |
| pole `id` (`pk`) | Django dodaje je **automatycznie** jako klucz główny (1, 2, 3, …) |

### 2.3. Migracje: `makemigrations` i `migrate`

Samo zapisanie `booking/models.py` jeszcze nie zmienia bazy danych. Zmiana struktury bazy odbywa się zawsze w **dwóch krokach**:

1. **`makemigrations`**: Django porównuje `models.py` z dotychczasową historią i generuje plik z „przepisem” migracji.
2. **`migrate`**: Django wykonuje ten przepis na bazie danych (tworzy lub zmienia tabele w pliku `db.sqlite3`).

Wykonaj krok pierwszy:

```bash
python manage.py makemigrations
```

```text
Migrations for 'booking':
  booking/migrations/0001_initial.py
    + Create model Specialization
    + Create model Specialist
```

Zajrzyj z ciekawości, jaki kod SQL wygenerowało Django dla tej migracji:

```bash
python manage.py sqlmigrate booking 0001
```

Nie musiałeś pisać ani jednej linijki `CREATE TABLE` ręcznie! Teraz zastosuj wszystkie migracje (naszą oraz wbudowane migracje Django, o które pytał czerwony komunikat na początku):

```bash
python manage.py migrate
```

Zobaczysz listę zielonych `OK`. W folderze projektu pojawił się plik `db.sqlite3`.

> 📌 **Co trafia do gita, a co nie?**
> - Pliki migracji (`booking/migrations/0001_initial.py`) **zawsze commitujemy** do gita: to część kodu aplikacji.
> - Plik bazy `db.sqlite3` jest w `.gitignore` i **nie trafia do gita**: każdy programista ma własną lokalną bazę testową.

Zróbmy commit z modelami i migracją:

```bash
git status                       # zauważ: db.sqlite3 nie ma na liście (dzięki .gitignore!)
git add booking/
git commit -m "Dodaj modele Specialization i Specialist wraz z migracją"
```

### 2.4. ORM w praktyce (`python manage.py shell`)

**ORM** (*Object-Relational Mapper*) pozwala rozmawiać z bazą danych za pomocą zwykłych obiektów Pythona zamiast zapytań SQL.
Otwórz interaktywną konsolę Django:

```bash
python manage.py shell
```

> 💡 **Nowość w Django 6:** tuż po uruchomieniu `shell` zobaczysz informację `... objects imported automatically`.
> Django 6 automatycznie importuje modele ze wszystkich zainstalowanych aplikacji (`Specialist`, `Specialization`),
> więc możesz od razu z nich korzystać! W skryptach (poza `shell`) importujemy je jawnie:
> `from booking.models import Specialist, Specialization`.

Wpisuj kolejno w konsoli `>>>`:

**1. Tworzymy specjalizacje:**

```python
cbt = Specialization.objects.create(name="Terapia poznawczo-behawioralna")
dzieci = Specialization.objects.create(name="Psychologia dziecięca")
pary = Specialization.objects.create(name="Terapia par")
Specialization.objects.all()
```

**2. Tworzymy specjalistów i przypisujemy im specjalizacje:**

```python
anna = Specialist.objects.create(
    first_name="Anna",
    last_name="Nowak",
    bio="Psychoterapeutka CBT z 8-letnim doświadczeniem.",
    mode="online",
    price=200,
)
anna.specializations.add(cbt, pary)

piotr = Specialist.objects.create(
    first_name="Piotr",
    last_name="Wiśniewski",
    bio="Psycholog dziecięcy i młodzieżowy.",
    mode="stacjonarnie",
    price=180,
)
piotr.specializations.add(dzieci)

maria = Specialist.objects.create(
    first_name="Maria",
    last_name="Zielińska",
    mode="hybrydowo",
    price=220,
)
maria.specializations.add(pary)
```

**3. Odpytujemy bazę (filtrowanie, sortowanie, modyfikacja):**

```python
Specialist.objects.all()                                # wszyscy specjaliści
Specialist.objects.count()                              # ilu ich jest?
Specialist.objects.filter(mode="online")                # tylko pracujący online
Specialist.objects.filter(price__lte=200)               # cena <= 200 zł (__lte = less than or equal)
Specialist.objects.filter(last_name__icontains="now")   # nazwisko zawiera "now" (bez wielkości liter)
Specialist.objects.order_by("price")                    # od najtańszego (z minusem "-price": od najdroższego)

# Relacja wiele-do-wielu w obie strony:
anna.specializations.all()                              # jakie specjalizacje ma Anna?
pary.specialists.all()                                  # jacy specjaliści prowadzą terapię par?

# Zmiana ceny i zapis do bazy:
piotr.price = 190
piotr.save()
```

Wyjdź z konsoli:

```python
exit()
```

W przeciwieństwie do `hello.py`, wszystkie dodane osoby **zostały trwale zapisane** w pliku `db.sqlite3`.
Za chwilę zobaczymy je w przeglądarce!

✅ **Punkt kontrolny 2:** `python manage.py showmigrations` pokazuje `[X]` przy wszystkich migracjach (w tym `booking.0001_initial`),
a w `shell` polecenie `Specialist.objects.count()` zwraca `3`.

---

## Część 3: Panel administracyjny `/admin/`

### 3.1. Konto administratora (`createsuperuser`)

Django ma wbudowany gotowy panel administracyjny pod adresem `/admin/`. Żeby się do niego zalogować,
utwórz konto administratora (*superuser*) w swojej lokalnej bazie:

```bash
python manage.py createsuperuser
```

Program zapyta kolejno o:

1. **Nazwa użytkownika** (*Username*): wpisz np. `admin` i Enter.
2. **Adres e-mail**: możesz wpisać np. `admin@example.com` albo zostawić puste (Enter).
3. **Hasło** (*Password*) i powtórzenie: wpisz np. `haslo1234` i Enter.
   - ⚠️ **Podczas wpisywania hasła na ekranie nie pojawiają się żadne znaki ani gwiazdki!** To celowe zabezpieczenie terminala — po prostu wpisz hasło i naciśnij Enter.
   - Jeśli Django ostrzeże, że hasło jest zbyt proste (*Bypass password validation and create user anyway? [y/N]*), wpisz **`y`** i Enter (na lokalnym komputerze proste hasło wystarczy).

Uruchom serwer:

```bash
python manage.py runserver
```

Wejdź na **http://127.0.0.1:8000/admin/** i zaloguj się utworzonym kontem.

### 3.2. Rejestracja modeli w `booking/admin.py`

Po zalogowaniu widzisz tylko *Grupy* i *Użytkownicy*. Gdzie są nasi specjaliści?
Trzeba jeszcze zgłosić modele do panelu admina. Otwórz plik `booking/admin.py` i wpisz:

```python
from django.contrib import admin

from .models import Specialist, Specialization

admin.site.register(Specialization)
admin.site.register(Specialist)
```

Zapisz plik i odśwież stronę **http://127.0.0.1:8000/admin/** w przeglądarce (serwer sam wykrył zmianę w kodzie!).
Pojawiła się sekcja **BOOKING**, a w niej **Specialists** oraz **Specializations** 🎉

### 3.3. Zarządzanie danymi w przeglądarce

1. Kliknij **Specializations**: widzisz 3 specjalizacje utworzone wcześniej w `shell`!
   Kliknij **Dodaj specialization** (prawy górny róg) i dodaj czwartą z `hello.py`: **`Psychoterapia psychodynamiczna`** → **Zapisz**.
2. Wróć i kliknij **Specialists**: widzisz Annę Nowak, Piotra Wiśniewskiego i Marię Zielińską.
3. Kliknij **Dodaj specialist** i dodaj czwartą osobę z `hello.py`:
   - *First name:* `Ewa`
   - *Last name:* `Lis`
   - *Bio:* `Psychoterapeutka pracująca w nurcie psychodynamicznym.`
   - *Mode:* `Stacjonarnie`
   - *Price:* `210`
   - *Specializations:* zaznacz `Psychoterapia psychodynamiczna` (z przytrzymanym **Ctrl** / **Cmd** możesz zaznaczyć kilka pozycji)
   - Kliknij **Zapisz**.

Zauważ, jak wiele dostałeś od Django za darmo: gotowe formularze, walidację (np. spróbuj wpisać ujemną cenę lub dodać drugą identyczną specjalizację),
listę rozwijaną dla pola `mode` i obsługę relacji wiele-do-wielu!

### 3.4. Zaktualizuj `README.md` i wyślij zmiany na GitHuba

W pliku `README.md` zaktualizuj sekcję `## Uruchomienie`, aby opisywała uruchomienie aplikacji Django:

```markdown
## Uruchomienie

    python -m venv .venv
    pip install -r requirements.txt
    python manage.py migrate
    python manage.py runserver
```

Zatrzymaj serwer (**Ctrl + C**), sprawdź stan repozytorium, zrób commit i wyślij kod na GitHuba:

```bash
git status
git add booking/admin.py README.md
git commit -m "Zarejestruj modele w panelu admina i zaktualizuj README"
git push
```

✅ **Punkt kontrolny 3:** w panelu http://127.0.0.1:8000/admin/ masz min. 4 specjalizacje i 4 specjalistów,
a Twoje commity z kodem projektu `config`, aplikacji `booking` i migracją `0001_initial.py` są na GitHubie (bez pliku `db.sqlite3`).

---

## Podsumowanie

- [ ] Zainstalowałem Django 6 w `.venv` i zaktualizowałem `requirements.txt`.
- [ ] Utworzyłem projekt `config` (`django-admin startproject config .`) oraz aplikację `booking` (`python manage.py startapp booking`) i dodałem ją do `INSTALLED_APPS`.
- [ ] Zdefiniowałem modele `Specialization` i `Specialist` połączone relacją `ManyToManyField`.
- [ ] Wykonałem `makemigrations` i `migrate` oraz rozumiem różnicę między nimi.
- [ ] Umiem tworzyć, filtrować i edytować obiekty przez ORM w `python manage.py shell`.
- [ ] Utworzyłem konto `superuser`, zarejestrowałem modele w `booking/admin.py` i dodałem dane w `/admin/`.

**Zadania po labie:** [zadania.md](zadania.md)

## Typowe problemy

| Objaw | Rozwiązanie |
|---|---|
| `No module named 'django'` lub `django-admin: command not found` | Środowisko `.venv` nie jest aktywne. Aktywuj je (`.venv\Scripts\Activate.ps1` lub `source .venv/bin/activate`) i upewnij się, że na początku wiersza widzisz `(.venv)` |
| Zapomniałem kropki w `django-admin startproject config .` (powstało `config/config/settings.py`) | Usuń nowo utworzony folder `config` (💻 `Remove-Item -Recurse -Force config`, 🍎 `rm -rf config`) i wykonaj polecenie ponownie z kropką na końcu: `django-admin startproject config .` |
| `CommandError: 'config' conflicts with the name of an existing Python module` | Próbujesz uruchomić `startproject config .` drugi raz, gdy folder `config` już istnieje |
| `makemigrations` mówi `No changes detected` mimo edycji `booking/models.py` | Zapomniałeś zapisać plik `booking/models.py` (**Ctrl/Cmd + S**) albo nie dopisałeś `"booking"` do `INSTALLED_APPS` w `config/settings.py` |
| `OperationalError: no such table: booking_specialist` | Utworzyłeś model, ale nie wykonałeś migracji: `python manage.py makemigrations`, a potem `python manage.py migrate` |
| Podczas `createsuperuser` nie mogę wpisać hasła (klawiatura „nie działa”) | Działa! Terminal celowo nie pokazuje wpisywanych znaków hasła. Wpisz hasło „w ciemno” i naciśnij Enter |
| W `/admin/` nie widać modeli `Specialist` i `Specialization` | Sprawdź, czy zapisałeś `booking/admin.py` z `admin.site.register(...)` oraz czy logujesz się kontem `superuser` |
| `Error: That port is already in use` | Serwer `runserver` już działa w innej zakładce terminala VS Code. Zamknij tamten proces (**Ctrl + C**) albo uruchom na innym porcie: `python manage.py runserver 8001` |
