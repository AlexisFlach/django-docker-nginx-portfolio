#### 04_Docker

###### 1.1 requirements.txt -> vilke dependencies som ska installeras

```
python freeze > requirements.txt
```

**requirements.txt**

```
Django==3.2.6
```

###### 1.2 

###### **Dockerfile.dev**

```
FROM python:3.8-slim-buster

WORKDIR /app

COPY requirements.txt requirements.txt

RUN pip3 install -r requirements.txt

COPY . .

CMD [ "python3", "manage.py", "runserver", "0.0.0.0:8000" ]
```

###### 1.3 Docker Client och Docker hub

```
docker build -f Dockerfile.dev -t <Dockerhubusername>/django-portfolio .

docker login
docker push <Dockerhubusername>/django-portfolio 
```

###### 1.4 arbeta med containern

```
docker run -p 8000:8000 -v $(pwd):/app <Dockerhubusername>/django-portfolio
```

