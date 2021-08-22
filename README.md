### 03_bootstrap

##### 1.1 länka till filer

**base.html**

```
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My App</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" integrity="sha512-1ycn6IcaQQ40/MKBW2W4Rhis/DbILU74C1vSrLJxCq57o941Ym01SwNsOMqvEBFlcgUa6xLiPY/NS5R+E6ztJQ==" crossorigin="anonymous" referrerpolicy="no-referrer" />
  <link rel="stylesheet" href="{% static 'css/style.css' %}">
</head>
<body>
<div class="container">
  {% include 'partials/_navbar.html' %}
  {% block content %} {% endblock %}
  {% include 'partials/_footer.html' %}
</div>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
</body>
</html>
```

###### 1.2 testa css

skapa en mapp **css** i static och i den mappen en fil **style.css**

```
body {
  background-color: paleturquoise;
}
```

##### 2 layout

Detta är layouten vi kommer att använda till vår startsida:

[![img](https://camo.githubusercontent.com/480e212f91923d8ecc0218fd55c60bafd5df25499040b4421c1b4a168493b1be/68747470733a2f2f73686172652e62616c73616d69712e636f6d2f632f7544396f6553517672614754696878454258673637782e706e67)](https://camo.githubusercontent.com/480e212f91923d8ecc0218fd55c60bafd5df25499040b4421c1b4a168493b1be/68747470733a2f2f73686172652e62616c73616d69712e636f6d2f632f7544396f6553517672614754696878454258673637782e706e67)

###### 2.1

**partials/_navbar**

```
{% load static %}
<nav class="navbar navbar-expand-md navbar-light">
  <!-- <a href="{% url 'index' %}" class="navbar-brand">Min Portfolio</a> -->
  <!-- Om man istället vill ha en logga -->
  <a href="{% url 'index' %}" class="navbar-brand">
    <img class="d-inline-block align-top"src="{% static 'img/logo.png' %}" alt="portolio logo" width="100" height="100">
  </a>
  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#toggleMobileMenu"
    aria-controls="#toggleMobileMenu" aria-expanded="false" aria-label="Toggle Navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="toggleMobileMenu">
    <ul class="navbar-nav ms-auto text-center">
      <li><a class="nav-link" href="{% url 'index' %}">Home</a></li>
      <li><a class="nav-link" href="{% url 'about' %}">About</a></li>
      <li><a class="nav-link" href="{% url 'contact' %}">Contact</a></li>
    </ul>
  </div>
</nav>
```

##### 2.2 hero section

Hero-sektion är uppdela i 2 sektioner; en med text och en med bild.

I desktop-versionen så ligger dessa horisontellt, men på mindre skärmar lägger dom sig vertikalt.

Vi behöver därför lite mer förståelse om **bootstraps grid system**.

https://www.youtube.com/watch?v=DZKf9l42WCo&ab_channel=Keepcoding

**index.html**

```
{% extends 'base.html' %}
{% block content %}

<div class="row">
<div class="col-md">Text</div>
<div class="col-md">Bild</div>
</div>

{% endblock %}
```

Detta skapar en rad och i raden två kolumner. Med class="col-md" menas att de kollapsar vid break-point md och vi får den layout vi är ute efter.

**bild inspiration**

https://undraw.co/

**se layout i browsern**

https://chrome.google.com/webstore/detail/pesticide-for-chrome-with/neonnmencpneifkhlmhmfhfiklgjmloi

**index.html** fortsättning

```
{% extends 'base.html' %}
{% block content %}
{% load static %}
<section class="hero row d-flex justify-content-center page-80">
<div class="row d-flex flex-sm-row-reverse justify-content-center my-auto">
  <div class="col-md text-center">
    <img src="{% static 'img/undraw_Online_party_re_7t6g.svg' %}" alt="hero-image" height="250"/>
   </div>
<div class="col-md">
<div class="text-center justify-content-center p-5">
  <h2 class="hero-title">Min portfolio</h2>
  <p class="hero-text">En gubbe i <span class="red">mängden</span></p>
  <a type="button" class="btn btn-light" href="{% url 'about' %}">Läs mer</a>
</div>
</div>
</div>
</section>

{% endblock %}
```

**style.css**

```
.page-80 {
  height: 80vh;
}

.red {
  color: red;
}

.hero-text {
  font-size: 2.5em;
}

.hero-sub-title {
  font-weight: 700;
}
```

###### 2.3 about och contact

**about.html**

```
{% extends 'base.html' %}
{% load static %}
{% block content %}
<section>
  <div class="col-8 col-md-6 mx-auto">
<h1>Om mig</h1>

<p>Fife ipsum yahoor jocky Wilson Ladybank minter hey min yer spavers doon Manage Wormit can't believe ma een The Skids Jock Stein aye right. Kincardine Silver Sands Dunfermline sitooterie Tayport Ballingry shut it Benarty Wormit aye right Buckhaven. The Skids ben the scullary Kelty shite, Inverkeithing Cupar yahoor like Rab Noakes. Rezillos totally shan Glenrothes hunners Leven Silver Sands off to get the messages cadge a lift. Anstruther howf Burntisland, a bogie scoor oot Levenmouth Benarty pish Ian Rankin.</p>
<p>Fife ipsum yahoor jocky Wilson Ladybank minter hey min yer spavers doon Manage Wormit can't believe ma een The Skids Jock Stein aye right. Kincardine Silver Sands Dunfermline sitooterie Tayport Ballingry shut it Benarty Wormit aye right Buckhaven. The Skids ben the scullary Kelty shite, Inverkeithing Cupar yahoor like Rab Noakes. Rezillos totally shan Glenrothes hunners Leven Silver Sands off to get the messages cadge a lift. Anstruther howf Burntisland, a bogie scoor oot Levenmouth Benarty pish Ian Rankin.</p>
<p>Fife ipsum yahoor jocky Wilson Ladybank minter hey min yer spavers doon Manage Wormit can't believe ma een The Skids Jock Stein aye right. Kincardine Silver Sands Dunfermline sitooterie Tayport Ballingry shut it Benarty Wormit aye right Buckhaven. The Skids ben the scullary Kelty shite, Inverkeithing Cupar yahoor like Rab Noakes. Rezillos totally shan Glenrothes hunners Leven Silver Sands off to get the messages cadge a lift. Anstruther howf Burntisland, a bogie scoor oot Levenmouth Benarty pish Ian Rankin.</p>
</div>
</div>

{% endblock %}
```

**contact.html**

```
{% extends 'base.html' %}
{% load static %}
{% block content %}
<section>
  <div class="col-8 col-md-6 mx-auto">
<h1>Kontakta mig</h1>

<p>Fife ipsum yahoor jocky Wilson Ladybank minter hey min yer spavers doon Manage Wormit can't believe ma een The Skids Jock Stein aye right. Kincardine Silver Sands Dunfermline sitooterie Tayport Ballingry shut it Benarty Wormit aye right Buckhaven. The Skids ben the scullary Kelty shite, Inverkeithing Cupar yahoor like Rab Noakes. Rezillos totally shan Glenrothes hunners Leven Silver Sands off to get the messages cadge a lift. Anstruther howf Burntisland, a bogie scoor oot Levenmouth Benarty pish Ian Rankin.</p>

</div>
</div>

{% endblock %}
```

**footer.html**

```
<footer class="text-center text-lg-start bg-light fixed-bottom">
  <section
    class="d-flex justify-content-center justify-content-lg-between p-4 border-bottom"
  >
    <div class="me-5 d-none d-lg-block">
      <span>Sociala länkar:</span>
    </div>
    <div>
      <a href="#" class="me-4">
        <i class="fab fa-instagram"></i>
      </a>
      <a href="#" class="me-4">
        <i class="fab fa-linkedin"></i>
      </a>
      <a href="#" class="me-4">
        <i class="fab fa-github"></i>
      </a>
    </div>
  </section>
</footer>
```

