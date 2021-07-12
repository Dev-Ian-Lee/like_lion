<h1>Heroku 배포하기</h1>

1. Heroku 회원가입
2. Heroku CLI 설치
3. 환경변수 적용
   * Debug
     * DEBUG = (os.environ.get('DEBUG', 'True') != 'False')



4. .gitignore 파일 적용

   * gitignore.io 페이지에서 Django 선택 후 나온 텍스트 .gitignore 파일에 복사

     

5. Heroku용 파일 작성

   * Profile 파일

     * web: gunicorn 프로젝트명.wsgi --log-file -

       

   * runtime.txt 파일

     * python-3.9.1

       

6. 필요한 Dependency 설치

   * pip install gunicorn whitenoise dj-database-url psycopg2-binary

     

7. settings.py 수정

   * Whitenoise 설치

     1. MIDDLEWATE > SecurityMiddleware

        * 'whitenoise.middleware.WhiteNoiseMiddleware'.

          

     2.  ALLOWED_HOSTS 수정

        * ALLOWED_HOSTS = []를 ALLOWED_HOSTS[' * ']로 수정

          

     3. DB 관련 코드 수정

        * settings.py 

          ```python
          import dj_databse_url
          db_from_env = dj_datebase_url.config(conn_max_age=500)
          DATABASES['default'].update(db_from_env)
          ```



1) requirements.txt 생성

* pip freeze > requirements.txt

  

2) git에 수정된 파일 추가

* git add -A

* git commit -m "add files for deploying to Heroku"

  

3) Heroku 관련 명령어 실행

1. heroku login

2. heroku create

3. git push heroku main

4. heroku run python manage.py migarte

5. heroku run python manage.py createsuperuser

6. heroku open

   

4) Heroku에서 환경변수 설정

1. [https://dashboard.heroku.com](https://dashboard.heroku.com/) 에서 앱 선택
2. Settings
3. Config Vars > Reveal Config Vars
4. KEY VAULE 값에 입력 후 저장