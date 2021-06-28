<h1>Template 상속</h1>

* html 파일들에서 중복되는 코드를 base.html로 만들고 다른 html 파일들에서 상속

  ```html
  {% block content %}
  
  	* 상속할 내용
  
  {% endblock %}
  ```

  * settings.py의 TEMPLATES.'DIRS'에 base.html의 경로 추가

  * 중복되는 CSS/JS 코드도 상속 가능

