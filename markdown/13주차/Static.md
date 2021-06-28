<h1>Static</h1>

* Django에서 다루는 파일

  * 정적 파일 : 서버에 미리 저장되어 있는 파일(있는 그대로)

    * Static : 개발자가 서버를 개발할 때 미리 넣어놓은 정적 파일(Img, JS, CSS)

    * Media : 사용자가 업로드할 수 있는 파일

      

  * 동적 파일 : 서버의 데이터들이 가공된 다음 보여지는 파일



* settings.py에 static 파일의 경로 추가

  * os.path.join 사용해 경로 생성

    

* python manage.py collectstatic

  * static 파일들을 모아 폴더 생성