# Ściągawka: Django, polecenia `manage.py`

Wszystkie polecenia wykonujesz w folderze z plikiem `manage.py`, **przy aktywnym środowisku `.venv`**.
Na macOS i Windows po aktywacji venv polecenia są identyczne (`python`, nie `python3`).

## Start projektu

| Co chcę zrobić | Polecenie |
|---|---|
| Zainstaluj Django | `pip install django` |
| Sprawdź wersję | `python -m django --version` |
| Utwórz projekt w **bieżącym** folderze | `django-admin startproject config .` |
| Utwórz aplikację | `python manage.py startapp booking` |
| Uruchom serwer deweloperski | `python manage.py runserver` → http://127.0.0.1:8000 |
| Zatrzymaj serwer | **Ctrl + C** |

> Po `startapp` dopisz aplikację do `INSTALLED_APPS` w `config/settings.py`!

## Baza danych i modele

| Co chcę zrobić | Polecenie |
|---|---|
| Przygotuj migracje po zmianie w `models.py` | `python manage.py makemigrations` |
| Zastosuj migracje w bazie | `python manage.py migrate` |
| Pokaż listę migracji i ich stan | `python manage.py showmigrations` |
| Pokaż SQL migracji | `python manage.py sqlmigrate booking 0001` |
| Konsola Pythona z dostępem do modeli | `python manage.py shell` |
| Utwórz administratora | `python manage.py createsuperuser` |
| Zmień hasło użytkownika | `python manage.py changepassword login` |
| Załaduj dane przykładowe (nasz projekt) | `python manage.py seed` |

**Cykl zmiany modelu:** edytuj `models.py` → `makemigrations` → `migrate` → (rejestracja w `admin.py`) → sprawdź w panelu.

## ORM w `shell`: najczęstsze operacje

```python
Specialist.objects.all()                        # wszystkie
Specialist.objects.filter(mode="online")        # filtrowanie
Specialist.objects.get(pk=1)                    # dokładnie jeden (błąd, gdy 0 lub >1)
Specialist.objects.filter(last_name__icontains="now")   # zawiera, bez wielkości liter
Specialist.objects.order_by("last_name")        # sortowanie
Specialist.objects.count()                      # liczba
s = Specialist.objects.create(first_name="Anna", last_name="Nowak")
s.last_name = "Kowalska"; s.save()              # zmiana
s.delete()                                      # usunięcie
s.specializations.all()                         # relacja M2M
```

## Przydatne adresy na serwerze deweloperskim

| Adres | Co tam jest |
|---|---|
| http://127.0.0.1:8000/ | strona główna aplikacji |
| http://127.0.0.1:8000/admin/ | panel administracyjny |
| http://127.0.0.1:8000/api/ | REST API (od labu 6) |
| http://127.0.0.1:8000/api/docs/ | Swagger UI (od labu 6) |

## Typowe błędy

| Komunikat | Co zrobić |
|---|---|
| `No module named django` | Aktywuj `.venv` |
| `no such table: booking_...` | Brakuje migracji: `makemigrations` + `migrate` |
| `You have N unapplied migration(s)` | `python manage.py migrate` |
| `That port is already in use` | Serwer już działa w innym terminalu albo: `python manage.py runserver 8001` |
| `TemplateDoesNotExist` | Sprawdź nazwę i położenie pliku szablonu oraz `INSTALLED_APPS` |
| `NoReverseMatch` | Literówka w nazwie adresu w `{% url %}` / `reverse()` albo brak parametru |
| Strona żółta z błędem | Czytaj od góry: typ błędu, plik i linia są podane. To Twój przyjaciel 🙂 |
