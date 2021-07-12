<h1>Ubuntu 배포하기</h1>

1. EC2 인스턴스 만들기

   1. [https://console.aws.amazon.com/](http://console.aws.amazon.com/)

   2. 우측 상단 지역이 Seoul 인지 확인

   3. EC2 검색

   4. Instances > Launch Instances

   5. Ubuntu Server 20.04 LTS (64-bit x86)

   6. t2.micro (Free tier eligible)

   7. Storage 설정 (Free Tier 은 30GB까지)

   8. Security Group > Add Rule

      * HTTP

      * HTTPS

      * Custom -> TCP -> 8000 ->0.0.0.0/0, ::/0

      

   9. Review and Launch > Launch

      * Create a new key pair

      * 원하는 이름 설정

      * Download Key Pair

      * Launch Instance

      

2. Elastic IP 받기
   * Network & Security > Elastic IPs
   * Allocate Elastic IP address
   * Allocate
   * Associate Elastic IP address
   * Instance > 아까 생성된 인스턴스 선택
   * Associate



3. SSH 연결하기
   * ssh ubuntu@"Elastic_IP" -i "PEM_FILE"



4. 필요한 패키지 설치
   * sudo apt update && sudo apt -y upgrade
   * sudo apt install -y python3 python3-pip python3-dev python3-venv build-essential libpq-dev vim git
   * sudo reboot
     * 업그레이드 도중 일부 시스템 파일이 변경되므로 재부팅 추천



5. 패키지 설치하는 동안 settings.py 수정
   1. 환경 변수 적용

      * Debug
        * DEBUG = (os.environ.get('DEBUG', 'True') != 'False')

        * ALLOWED_HOSTS = []를 ALLOWED_HOSTS = ['*****']로 수정

          

   2. pip freeze > requirements.txt

   3. git add -A

   4. git commit -m "edit for deployment"

      

6. 우리가 매번 하던것들

   * python3 -m venv venv

   * git clone "my_repo" django-app

   * cd django-app

   * source ../venv/**bin**/activate`

     * Windows 처럼 Scripts 가 아닌 **bin** 임에 주의

       

   * pip install -r requirements.txt

   * python manage.py runserver 0.0.0.0:8000

   * http://Elastic_IP:8000으로 접속해서 Django 화면이 표시되는지 확인

   * deactivate로 가상환경 밖으로 탈출

     

7. PostgreSQL 설치

   * sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

   * wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

   * sudo apt update

   * sudo apt -y install postgresql

   * sudo systemctl enable --now postgresql@13-main

     * 시스템 시작 시 자동 실행

       

   * sudo -u postgres createdb django

     * django 는 원하는 DB 이름 입력

       

   * sudo -u postgres psql django

     * DB 접속

       

   * create user django with password 'p@ssword!';

     * 원하는 username과 password 입력

       

   * alter role django set client_encoding to 'utf-8';

     * 인코딩 수정 (한글깨짐 방지)

       

   * alter role django set timezone to 'Asia/Seoul';

     * 시간 수정

       

   * grant all privileges on database django to django;

     * django 데이터베이스의 모든 권한을 django 유저에게 부여

     

8. settings.py 에 DB 설정
   * settings.py > DATABASES 수정

```
DATABASES = {
   'default': {
       'ENGINE': 'django.db.backends.postgresql_psycopg2',
       'NAME': os.environ.get('DB_NAME'),
       'USER': os.environ.get('DB_USER'),
       'PASSWORD': os.environ.get('DB_PASSWORD'),
       'HOST': os.environ.get('DB_HOST'),
       'PORT': '',
   }
}
```

1. git add -A

2. git commit -m "edit database"

3. git push

4. 업데이트된 설정 서버에 가져오기 (서버에서 실행)

   1. cd /home/ubuntu/django-app

      * git 레포 클론한 위치로 가기

        

   2. git pull

   3. source ../venv/bin/activate

   4. pip install psycopg2

      * PostgreSQL 접속에 필요한 패키지 설치

        

5. 환경 변수 설정 (임시 / 테스트용)

   1. export DB_NAME=django

   2. export DB_USER=django

   3. export DB_PASSWORD='p@ssword!'

   4. export DB_HOST=localhost

      

6. 잘 작동하는지 테스트

   1. python manage.py makemigrations

   2. ``python manage.py migrate`

   3. python manage.py runserver 0.0.0.0:8000

   4. [http://내_Elastic_IP:8000](http://xn--_elastic_ip-v455b:8000/) 로 접속해서 Django 화면이 표시되는지 확인

      

7. gunicorn 으로 Django App 실행

   1. pip install gunicorn

      * 반드시 venv 안에서 실행

   2. deactivate

   3. ../venv/bin/gunicorn 프로젝트명.wsgi -b 0.0.0.0

   4. [http://내_Elastic_IP:8000](http://xn--_elastic_ip-v455b:8000/) 로 접속해서 Django 화면이 표시되는지 확인

      * 사진 등 static files는 표시 안되는게 정상

        

8. systemd 에 gunicorn 등록

   - vi 는 에디터

   - 파일을 연 후 i / insert키 누르면 내용 입력 가능

   - 수정한 사항을 저장하고 싶으면 Esc → :w → Enter

   - 종료하고 싶으면 Esc → :q → Enter

   - 합하면 Esc → :wq → Enter 로 저장하고 종료

   - 저장하지 않고 강제로 종료하려면 Esc → :q! → Enter

     - !는 강제 옵션

       

   - sudo vi /etc/systemd/system/gunicorn.service

```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
Type=notify
RuntimeDirectory=gunicorn
WorkingDirectory=/home/ubuntu/django-app
ExecStart=/home/ubuntu/venv/bin/gunicorn 프로젝트명.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
KillMode=mixed
TimeoutStopSec=5
PrivateTmp=true
EnvironmentFile=/etc/gunicorn/env.conf

[Install]
WantedBy=multi-user.target
```



- sudo vi /etc/systemd/system/gunicorn.socket

```
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock
SocketUser=www-data

[Install]
WantedBy=sockets.target
```



- sudo mkdir /etc/gunicorn
- sudo vi /etc/gunicorn/env.conf

```
DB_NAME=django
DB_USER=django
DB_PASSWORD='p@ssword!'
DB_HOST=localhost
#기타 환경변수 여기에 입력
```

1. 생성한 서비스 실행

   1. sudo systemctl daemon-reload

   2. sudo systemctl enable --now gunicorn.socket

   3. sudo systemctl enable --now gunicorn

      

2. 테스트

   1. curl --unix-socket /run/gunicorn.sock http

   - 실행시 HTML 코드가 나오면 성공

     

3. Nginx 설치

- Nginx는 웹서버로 외부 세상 <-> Gunicorn 을 연결하는 역할

  1. sudo apt -y install nginx

  2. sudo vi /etc/nginx/sites-available/django_app

     

```
server {
       listen 80;
       server_name 내_Elastic_IP_주소;

       location = /favicon.ico { access_log off; log_not_found off; }

       location /static/ {
               root /home/ubuntu/django-app;
       }

       location / {
               include proxy_params;
               proxy_pass http://unix:/run/gunicorn.sock;
       }
}
```

1. sudo ln -s /etc/nginx/sites-available/django_app /etc/nginx/sites-enabled
2. sudo nginx -t
3. sudo systemctl restart nginx
4. [http://내_Elastic_IP](http://xn--_elastic_ip-v455b/) 접속시 Django 화면이 보이면 성공!
5. source ../venv/bin/activate
6. python manage.py collectstatic
   * 이미지나 CSS등 Static 파일들