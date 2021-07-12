<h1>Docker 서버에 배포하기</h1>

1. 준비사항

   * EC2 인스턴스

   * Docker Hub에 등록된 이미지

   

2. 서버 세팅

   1. Docker 설치

      * curl -fsSL [https://get.docker.com](https://get.docker.com/) | bash

        

   2. Docker Compose 설치

      * sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose$(uname -s )-$(uname-m)" -o/usr/local/bin/docker-compose

      * sudo chmod +x /usr/local/bin/docker-compose

        

   3. 앱 설치 폴더 지정

      * sudo mkdir /opt/django-app

      * cd /opt/django-app

        

   4. docker-compose.yml

      * vi docker-compose.yml

   ```
   version: "3.9"
   
   services:
    db:
      image: postgres
      environment:
        - POSTGRES_DB=${DB_NAME}
        - POSTGRES_USER=${DB_USER}
        - POSTGRES_PASSWORD=${DB_PASSWORD}
    web:
      image: DockerHubId/django-app
      environment:
        - DB_NAME=${DB_NAME}
        - DB_USER=${DB_USER}
        - DB_PASSWORD=${DB_PASSWORD}
        - DB_HOST=db
        - DEBUG=${DEBUG}
      ports:
        - "8000:8000"
      depends_on:
        - db
   ```

   

   * pastebin.com 등의 서비스에 업로드 후 wget 사용

   * sudo wget https://pastebin.com/raw/pasteld -0 docker-compose.yml

   ```
   DB_NAME=django
   DB_USER=django
   DB_PASSWORD=django
   DEBUG=False
   ```

   1. DB 최초 실행 (db 생성 , 사용자 등록)

      * sudo docker-compose up db

      

   2. 테스트

      * sudo docker-compse up

      * IP:8000 접속 되는 확인

        

   3. 백그라운드 모드 전환

      * sudo docker-compose up -d