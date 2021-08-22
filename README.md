#### 05 nginx

Länkar: https://docs.gunicorn.org/en/stable/index.html

http://nginx.org/en/docs/

https://www.youtube.com/watch?v=vJAfq6Ku4cI&t=797s&ab_channel=DotJA

https://www.youtube.com/watch?v=YnrgBeIRtvo&ab_channel=DotJAhttp://nginx.org/en/docs/

https://www.youtube.com/watch?v=hcw-NjOh8r0&ab_channel=HusseinNasser

https://www.digitalocean.com/community/tutorials/understanding-nginx-http-proxying-load-balancing-buffering-and-caching

https://www.youtube.com/watch?v=AuINJdBPf8I&ab_channel=JavaBrains

#### 0 Teori

###### 0.1 Överblick

- Hur körs ett Django projekt i development ocg produktion?
- Vad är Nginx?
- Vad är en WSGI server(Gunicorn)?

Hittills har vi startat upp vår server genom att köra:

```
python manage.py runserver
```

Det funkar kanon i dev, men för produktion behöver vi något bättre.

**WSGI** Web Server Gateway Interface, beskriver en grupp web servers som kan fungerara som mellanhand (gränssnitt) mellan en python-applikation och och en web server.

**gunicorn** är ett exempel på en WSGI server.

**nginx** är ett exempel på en web server.

**django** är ett exempel på en python applikation.

Nginx kommer att vara ditt användare skickar requests. När användare surfar in på vår hemsida ock trycker på enter så kommer en request att skickas till nginx.

Nginx kommer att läsa av requesten och antingen *serva* statiskt innehåll så som bilder eller css, eller, om requesten söker dynamiskt innehåll, skicka vidare requesten till WSGI.

WSGI interagerar med vår python-applikation, och skickar tillbaks en response till web servern, som i sin tur skickar den till client.

###### 0.2 WSGI vs runserver

WSGI, **gunicorn**, kan ha fler workers, har logg-support, är säker och är snabb.

Vad den inte kan göra är att serva static files, vilket dev servern kan göra.

Det är därför vi behöver en web server så som **nginx**.

###### 0.3 nginx

Nginx är en *reverse-proxy-server* som tar emot requests from client och bestämmer vart dessa ska delegeras.

###### 0.3.1 vad är proxy?

"the authority to act on the behalf of someone else".

*proxying* betyder att man passar vidare requests. Det kan vara bra att använda sig av proxy då man har servers i projektet som inte är byggt för att hantera stora mängder av requests. Med proxy kan man då fördela dessa requests: **load balancing**.

Nginx är **point-of-contact** för clients. Istället för att client överröser frontend servern med requests, så skickas istället requesten till nginx som i lugn och ro kan fördela ansvaret.

De servers som nginx sänder vidare requests till kallas för **upstream servers**.

###### 0.3.2 proxy vs reverse-proxy

För att förstå *proxying* behöver vi först vem som skickar data, och till vem.

Vi kan etablera att client skickar en request till server.

Exempel på proxy är när du loggar in på din mail. Du skickar en request till en proxy som autentiserar dig och kanske skickar med en cookie med info. Proxy skickar vidare information till mail-servern. Mail-servern har bara informationen från proxy. Detta brukar kallas för **forward proxy**.

**Reverse proxy** hanterar requests som kommer in. Forward proxys är för clients. Reverse proxys är för servers.

Tänk att du har en applikationen med flera services som alla har olika end-points. Vi sätter upp en reverse-proxy-server framför dessa services. Den server ansvarar för att ta request utifrån, och skicka rätt request till rätt server, få response, och skicka tillbaks responsen till client.

Så med forward proxy hade email-servern ingen aning om vem som utförde requesten. Med reverse-prxy har client ingen aning om vem som hanterar dess request.

Fördelar med reverse-proxy:

- säkerhet
- caching
- load balancing
  - Fördela inkommande trafik jämt

###### 04. nginx configuration file

Nginxs's konfigurations-fil innehåller direktiv på hur nginx kommer att köras.

Ett direktiv är ett kommando som accepterar en eller flera parametrar **och avslutas med semikolon**. Dessa direktiv kan grupperas i kontext. Det kan finnas flera kontexter inom samma konfigurations-fil.

###### 04.01 server context

```
## main context

worker_processes auto;

error_log /path/to/error error;

## server block -> http context
http {
	server {
			listen 80;
			server_name mydomain.com www.mydomain.com;
			location / {
			root /path/to/folder;
			index index.html;
			}
	}
}
```

###### 04.02 proxy server

Nginx hanterar static files, och proxy server alla andra requests.

Exempel med nginx och gunicorn.

```
http {
	server {
		location /static {
			root /path/to/root;
		}
	}
}
```

Om "/server" är en del av uri (GET [www.mydomain.com/static/img.png](http://www.mydomain.com/static/img.png)) säger vi åt nginx vart den ska leta efter filen/filerna (root /path/to/root;).

Vi kan använda **alias** istället för root.

```
http {
	server {
		location /static {
			alias /path/to/root;
		}
	}
}
```

Alias använder path, istället för location.

Proxy server:

```
server {
  listen 80;

  location / {
    proxy_pass http://127.0.0.0.1:8000;
  }
  location /static {
    alias /static/;
  }
}
```

#### 1. setup and unicorn

Vi kommer att ha

- en Django Container som körs på Gunicorn
- en nginx container för proxy

```
app
	app
	pages
nginx
	Dockerfile
	default.conf
docker-compose.yml
entrypoint.sh
Dockerfile
requirements.txt
```

#### 

###### 1.1 requirements.txt

```
Django==3.2.6
gunicorn==20.0.4
```

###### 1.2 Dockerfile

**django/Dockerfile**

```
FROM python:3.8.5-alpine

RUN pip install --upgrade pip

COPY ./requirements.txt .
RUN pip install -r requirements.txt

COPY ./app /app

WORKDIR /app

COPY ./entrypoint.sh /
ENTRYPOINT ["sh", "/entrypoint.sh"]
```

###### 1.3 entrypoint.sh

```
#!/bin/sh

python manage.py migrate --no-input
python manage.py collectstatic --no-input

gunicorn app.wsgi:application --bind 0.0.0.0:8000
```

###### 1.4 docker-compose.yml

i root

```
version: "3.7"

services: 
  django:
    volumes:
      - static:/static
    env_file:
      - .env
    build:
      context: .
    ports:
      - "8000:8000"

volumes:
  static:
```

###### 1.5 .env

**.env**

```
SECRET_KEY = 'INSTEAD OF HAVING IT IN settings.py'
DEBUG = True
```

skapa en .gitignore i root

```

.env 
```

###### 1.6 app/app/settings.py

```
SECRET_KEY=os.getenv('SECRET_KEY')
DEBUG=os.getenv('DEBUG')
...
ALLOWED_HOSTS = ['*']
```

#### 2. NGINX

###### 2.1 nginx

Skapa en ny mapp i root som heter nginx.

###### 2.2 Dockerfile

```
FROM nginx:1.19-alpine
COPY ./default.conf /etc/nginx/conf.d/default.conf
```

###### 2.3 default.conf

```
upstream django {
  server django:8000;
}

server {
  listen 80;

  location / {
    proxy_pass http://django;
  }
  location /static {
    alias /static/;
  }
}
```

###### 2.4 docker-compose.yml

```
version: "3.7"

services: 
  django:
    volumes:
      - static:/static
    env_file:
      - .env
    build:
      context: ./django
    ports:
      - "8000:8000"
  nginx:
    build: ./nginx
    volumes:
      - static:/static
    ports:
      - "80:80"
    depends_on: 
      - django

volumes:
  static:
```

#### 3. Docker Compose

```
docker-compose up --build
```

