# RESTful한 URL(7원칙)
2022/07/18

## 1. 포스팅 이유
controller url 작성 시 RESTful 하게 작성해보자

## 2. REST

## 3. 7 Rules for REST API URI Design
1. 소문자를 사용한다

- 대문자는 때로 문제를 일으키는 경우가 있기 때문에 URI를 작성할 때는 소문자를 선호한다.
- RFC3986은 체계 및 호스트 구성요소를 제외하고 URI를 대소문자를 구분하여 정의한다.
- 주소에서 대소문자를 구분하므로, 카멜방식이 아닌 소문자를 사용하여 작성한다.

**BAD**
```
http://api.example.com/users/postComments
```

**GOOD**
```
http://api.example.com/users/post-comments
```

2. 언더바대신 하이픈을 사용한다.

- 가독성을 위해 긴 Path를 표현하는 단어는 하이픈으로 구분하는 것이 좋다.
- 프로그램의 글자 폰트에 따라서 언더바 문자는 문자가 부분적으로 가려지거나 숨겨질 수 있다. 
- 혼란을 야기할 수 있으므로 하이픈을 사용한다.
- 가급적 하이픈의 사용도 최소화하며, 정확한 의미나 표현을 위해 단어의 결합이 불가피한 경우에 사용한다.

**BAD**
```
http://restapi.example.com/users/post_comments
```

**GOOD**
```
http://restapi.example.com/users/post-comments
```

3. URI의 마지막에는 슬래시를 포함하지 않는다.
- 후행 슬래쉬는 의미가 전혀 없고 혼란을 야기할 수 있다.
- 슬래시는 계층을 구분하는 것으로, 마지막에는 사용하지 않는다.

많은 웹 구성 요소와 프레임워크는 다음 두 URI를 동등하게 취급한다.
```
http://api.canvas.com/shapes/
http://api.canvas.com/shapes
```
- 그러나 URI내의 모든 문자는 리소스의 고유 ID에 포함된다.
- 두 개의 다른 URI는 두개의 다른 리소스에 매핑된다. URI가 다르면 리소스도 다르고 그 반대도 마찬가지다. 그러므로 REST API는 명확한 URI를 생성해야한다.

**BAD**
```
http://restapi.example.com/users/
```

**GOOD**
```
http://restapi.example.com/users
```

4. 계층관계를 나타낼 때는 슬래시 구분자를 사용해야한다. + 행위는 포함하지 않는다.
- 슬래시 문자는 URI의 경로 부분에서 자원 간의 계층적 관계를 나타내기 위해 사용한다.
- 행위는 포함하지 않는다.
- 행위는 URL대신 Method(GET, POST, PUT, DELETE 등))를 사용하여 전달한다.

**BAD**
```
POST http://restapi.example.com/users/1/delete-post/1
```

**GOOD**
```
DELETE http://restapi.example.com/users/1/posts/1
```

5. 파일 확장자는 URI에 포함시키지 않는다.
- 파일 확장자는 URI에 포함하지 말아야한다. 
- 대신에 Content-Type 이라는 헤더를 통해 전달되는대로 미디어 타입을 사용하여 body의 콘텐츠를 처리하는 방법을 결정한다.
- Rest API클라이언트는 HTTP에서 제공하는 형식 선택 메커니즘인 Aceept 요청 헤더를 활용하도록 권장해야 한다.

**BAD**
```
http://restapi.example.com/users/photo.jpg
```

**GOOD**
```
GET http://restapi.example.com/users/photo

HTTP/1.1 Host: restapi.example.com Accept: image/jpg
```

6. 전달하고자 하는 자원의 명사를 사용하되, 컨트롤 자원을 의미하는 경우 예외적으로 동사를 허용한다.

**BAD**
```
http://restapi.example.com/posts/duplicating
```

**GOOD**
```
http://restapi.example.com/posts/duplicate
```


7. URI에 작성되는 영어를 복수형으로 작성한다.
- 하나의 인스턴스를 복수형으로 표시하는게 영어 문법적으로 맞지 않겟다고 생각할 수도 있지만 
- URI의 형식을 복수형으로 사용하는 것이 실무에서 많이 사용되고 있다.
- 관계가 다른 리소스 내에서만 존재할 경우 RESTFUL 원칙은 다음과 같은 지침을 제공한다.
  - http://api.college.com/students/3248234/courses
  - ID가 3248234인 학생이 학습한 모든 과정 목록 검색 
  - http://api.college.com/students/3248234/courses/physics
  - ID가 3248234인 학생을 위한 과정 물리학을 검색한다.

## 참고 사이트
https://dzone.com/articles/7-rules-for-rest-api-uri-design-1

https://velog.io/@pjh612/REST-API-URI-%EA%B7%9C%EC%B9%99

https://devuna.tistory.com/79?category=939368