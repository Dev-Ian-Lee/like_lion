<h1>CSS 기초</h1>

* CSS : Cascading Style Sheet(스타일 정의 명세서)



* CSS 기초 문법

  * p {											

    ​     font-family: '맑은 고딕';

    ​     font-size: 18px;

    ​     color: blue;						

    }

    * {} : 선언 블럭(Declaration Block)

    * p : 선택자(Selector) : HTML의 p태그를 가리킴

    * font-family, font-size, color : 속성(Property) : 지정할 스타일의 속성명

    * '맑은 고딕', 18px, blue : 값(Value) : 키워드나 특정 단위를 이용해 속성에 값 부여

    * 속성과 값은 짝을 이룸 → 이를 '선언(Declaration)'이라 칭함

      * 각 선언 뒤에는 세미콜론 필요

        

* HTML에 CSS를 적용하는 방법

  * Link style : HTML 외부에 있는 CSS 파일을 불러옴 → 가장 많이 사용

    ```html
    <head>
        <link rel="stylesheet' href='styles.css">
    </head>
    ```

    

  * Embedding style : HTML의 head 블럭 안에 style 태그를 사용해 CSS 작성

    ```html
    <head>
        <style>
            h1 {
                color: red;
            }
        </style>
    </head>
    ```

    

  * Inline style : HTML 요소에 직접 속성(Attribute)을 이용해 CSS 작성

    ```html
    <body>
        <h1 style="color: red;">
            hello
        </h1>
    </body>
    ```

    

