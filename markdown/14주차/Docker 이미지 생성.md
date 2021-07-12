<h1>Docker 이미지 생성</h1>

1. 준비사항

   * Gitpod.io 계정

   * Gitpod 설정 수정
     * Gitpod > Settings
       * Feature Preview > Enable Feature Preview 체크
       * Default IDE 선택

   

   * Gitpod 인스턴스 생성

   * GitHub 레포지토리

   * 레포지토리 주소 앞에 gitpod.io

   * DockerHub 계정 생성

   

2. requirements 설치

   * pip install -r requirements.txt

   

3. Whitenoise 설치

   * pip install whitenoise

   * MIDDLEWARE > SecurityMiddleware

   * 'whitenoise.middleware.WhiteNoiseMiddleware',

   

4. Gunicorn 설치

   * pip install gunicorn

   

5. Dockerfile 생성

```
FROM python:3.8
ENV PYTHONUNBUFFERED=1
RUN mkdir /app
WORKDIR /app
COPY . /app
RUN apt-get update \
   && apt-get install -y \
   python3 python3-pip python3-dev python3-venv build-essential libpq-dev \
   && rm -rf /var/lib/apt/lists/*
RUN pip install -r requirements.txt
RUN chmod +x /app/run.sh
EXPOSE 8000

ENTRYPOINT ["/app/run.sh"]
```



* run sh 파일 생성

  ```
  #!/bin/bash
  
  python manage.py migrate
  
  python manage.py collectstatic
  
  gunicorn lionproject.wsgi -b 0.0.0.0:8000
  ```

  

  * pip freeze > requirements.txt 
  * sudo docer-up
  * Ctrl + Shift + `
  * docker build -t DockerHubld/django-app .
  * docker run -it -p 8000:8000 DockerHubld/django-app
  * docker login
  * docker push DockerHubld/django-app
  * https://hub.docker.com/r/DockerHubld/django-app 에서 확인