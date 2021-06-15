<h1>Model 실습</h1>

* 테이블(릴레이션)과 같은 이름의 클래스 정의

  * models의 Model 클래스 매개변수로 사용

    

* 테이블의 각 칼럼 변수(필드)로 정의

  * charField : 문자열 자료형

    * max_length : 문자열 자료형의 최대 길이 제한

      

  * DateTimeField : 날짜 자료형

  * TextField : 글자 수 제한 없는 문자열 자료형

  * 부모 클래스로부터 상속받은 칼럼은 정의 불필요



* python manage.py makemigrations
  *  models.py의 변경 사항 앱 내의 migrations 폴더에 저장



* python manage.py migrate
  * migration 폴더를 실행시켜 데이터베이스에 적용



* superuser 등록
  * python manage.py createsuperuser

    * superuser 생성

      

  * models.py에 admin.site.register('models 이름')으로 superuser 등록

