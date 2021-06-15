<h1>Django 실습</h1>

* 에러의 이유
  * 오타
  * 저장 안 함



* App : Django 프로젝트를 이루는 작은 단위
  * 유지보수 용이



* manage.py아닌 프로젝트 파일 내에서 실행하는 것이 더 좋음
  * python manage.py startapp '앱 이름'



* '프로젝트 이름'/settings.py 에서 INSTALLED_APPS 블럭에 앱 이름 추가
  * ''앱 이름''.apps.'앱 이름(첫 문자 대문자)'Config
  * 줄 마지막에 콤마 추가하는 것 권장



* 웹사이트 구동 순서
  1. 사용자가 서버에 요청
  2. 서버의 view는 model에게 요청에 필요한 데이터를 받음
  3. view는 받은 데이터를 적절하게 처리해서 template으로 넘김
  4. template은 받은 정보를 사용자에게 보여줌



* template은 별도의 파일 만들어서 보관
* view는 views.py에 작성
  * .py 파일이므로 파이썬 문법으로 작성
  * request받아서 render 반환



* url과 view 연결
  * urls.py에 앱 import
  * urlpatterns에 path함수 사용해 연결
    * 세 번째 인자로 이름 부여하는 것 권장



* python manage.py runserver 명령어로 서버 실행
  * url 클릭해 내가 생성한 웹사이트로 이동
  * url에 다른 path 함수의 첫 번째 인자 추가하면 해당 url로 이동 가능(/admin) 



* request.GET['input태그 이름']을 통해 html 파일의 input태그에 사용자가 한 입력 가져올 수 있음
  * 이를 변수로 저장하고 render함수로 반환하면, 다른 파일에서도 사용자의 입력값 사용 가능
    * render(request, 'html 파일 이름', {'변수명' : 변수명'})



* 정리
  1. app 생성
  2. template 제작
  3. view 제작
  4. url 변경