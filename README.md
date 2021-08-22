#### 02_pages

###### 1.1

```
python manage.py startapp pages
```

###### 1.2

**app/settings.py**

```
INSTALLED_APPS = [
    'pages.apps.PagesConfig',
    ...
    ]
```

###### 1.3

**Skapa: pages/urls.py**

```
from django.urls import path
from . import views

urlpatterns = [
  path('', views.index, name="index")
]
```

###### 1.4

**pages/views.py**

```
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
  return HttpResponse('<h1>Hello World</h1>')
```

###### 1.5

**app/urls.py**

```
from django.contrib import admin
from django.urls import path, include


urlpatterns = [
    path('', include('pages.urls')),
    path('admin/', admin.site.urls),
]
```

Om vi kör vår server och kollar localhost:8000 bör vi nu se

```
python manage.py runserver
```

"Hello World"

##### 2. Templates

###### 2.1

**app/settings.py**

Tala om för Django var den kan hitta våra templates:

```
import os

TEMPLATES = [
    {
        ...,
        'DIRS': [os.path.join(BASE_DIR, 'templates')],
        ...
```

###### 2.2 Skapa en template-mapp i root

```
django
	templates
		base.html
		pages
			index.html
			about.html
```

###### 2.3

**pages/urls.py**

```
urlpatterns = [
  path('', views.index, name="index"),
  path('about', views.about, name="about"),
  path('contact', views.contact, name="contact"),
]
```

###### 2.4

**pages/views.py**

```
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
  return render(request,'pages/index.html')

def about(request):
  return render(request, 'pages/about.html')
  
def contact(request):
  return render(request, 'pages/contact.html')
```

###### 2.5

**base.html**

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My App</title>
</head>
<body>
<div class="container">
  {% block content %} {% endblock %}
</div>
</body>
</html>
```

**index.html**

```
{% extends 'base.html' %}

{% block content %}
<h1>Home</h1>
{% endblock %}
```

Samma sak med about.html och contact.html

##### 3. Static files och dess paths

###### 3.1

**skapa mappar: app/static/img**

Lägg till bilder ni vill använda, exempelvis logga.

###### 3.2

**app/settings.py**

```
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATIC_URL = '/static/'
STATICFILES_DIRS = [
os.path.join(BASE_DIR, 'app/static')
]

```

```
python manage.py collectstatic
```



##### 4. Partials

###### 4.1

Skapa en mapp i templates och namnge den "partials".

```
templates
	pages
		...
	partials
		_navbar.html
		_footer.html
```

###### 4.2

**navbar.html**

```
<nav>
  <ul>
    <li>home</li>
    <li>about</li>
    <li>contact</li>
  </ul>
</nav>
```

###### 4.3 load static (bara som info)

```
{% load static %}

<img src=""{% static 'img/ex.jpg'%}"
```

###### 4.4 linking (bara som info)

```
<a href="{% url 'index' %}"
```

###### 4.5 

**base.html**

```
...
<div class="container">
{% include 'partials/_navbar.html' %}
{% block content %} {% endblock %}
{% include 'partials/_footer.html' %}
</div>
...
```

