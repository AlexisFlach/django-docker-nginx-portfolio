## 01_setup

##### 1. Virtual Environment Setup

------

Vi vill hålla alla våra packages och dependencies isolerade inuti vårt projekt.

https://docs.python.org/3/library/venv.html?highlight=venv

###### 1.1 - skapa en mapp

```
mkdir django-project
cd django-project
mkdir django
cd django
```

###### 1.2 skapa miljön:

```
python -m venv ./venv
```

###### 1.3 Aktivera miljön:

Ha gärna detta kommando lättillgängligt då det kommer att användas frekvent.

**Mac**

```
source ./venv/bin/activate
```

Om det inte funkar använd istället *absolute path*

**Windows**

```
python ./venv/Scripts/activate.bat
```

**Lämna miljön**

```
deactivate
```

Output installed packages in requirement format:

```
pip freeze
```

##### 2. Django Install and project setup

------

###### 2.1 installera Django

```
pip install django
```

###### 2.2 skapa en app

```
django-admin startproject app . 
```

###### 2.3 .gitignore

Länk: gitignore.io -> django

```
# .gitignore

*.log
*.pot
*.pyc
__pycache__/
local_settings.py
db.sqlite3
db.sqlite3-journal
media
venv
/static
```

###### 2.4 köra servern

```
python manage.py runserver
# -> http://localhost:8000/	
```

