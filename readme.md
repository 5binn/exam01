## 1차 요구사항 구현
- [x] 유저가 루트 url로 접속시에 게시글 리스트 페이지(http://주소:포트/article/list)가 나온다.
- [x] 리스트 페이지에서는 등록 버튼이 있고 버튼을 누르면 http://주소:포트/article/create 경로로 이동하고 등록 폼이 나온다.
- [x] 게시글 등록을 하면 http://주소:포트/article/create로 POST 요청을 보내어 DB에 해당 내용을 저장한다.
- [x] 게시글 등록이 되면 해당 게시글 리스트 페이지로 리다이렉트 된다. 페이지 URL 은 http://주소:포트/article/list 이다.
- [x] 리스트 페이지에서 해당 게시글을 클릭하면 상세페이지로 이동한다. 해당 경로는 http://주소:포트/article/detail/{id} 가 된다.
- [x] 게시글 상세 페이지에는 id에 맞는 게시글 데이터와 목록 버튼이 있다. 목록 버튼을 누르면 게시글 리스트 페이지로 이동하게 된다.

**추가 기능**
- 루트 함수로 http://주소:포트 또는 http://주소:포트/article 경로 이동 시 리스트 페이지로 리다이렉트 된다.
- 벨리데이션 주입하여 게시글 등록 시 여백을 허용하지 않게 된다.

## 미비사항 or 막힌 부분

- 부트스트랩 활용<br>
상세 페이지에서 날짜를 우측 정렬해보고 싶었으나 하지 못해 후에 해당 기능을 알아볼 예정입니다.
- 타임리프 문법 미숙<br>
HTML 도 아직 숙련되지 않아 해당 문법 작성 시 교재 참고하여 작성하였습니다.


## MVC 패턴
- model, view, controller 세 파트로 구분되어집니다. 각 요소들은 다른 구성요소의 동작요소를 알면 안되고 변경이 있을 시 변경을 통지할 수 있어야 합니다.
- 뷰(view)는 사용자 인터페이스 요소를 나타내고 단순히 화면에 표시하는 기능만 가지고 모델의 정보를 따로 저장하면 안됩니다.
- 모델(model)은 정보, 데이터를 나타내고 사용자가 원하는 모든 데이터를 가지고 있어야 하고 그 외에 뷰/컨트롤러의 어떤 정보도 알면 안됩니다.
- 컨트롤러(controller)는 두 요소를 이어주는 역할이고 두 요소를 중재하기 위해서는 두 요소를 알고 있어야 합니다.
- 유저가 뷰에서 화면을 보고 입력을 하면 컨트롤러에서 모델에 있는 데이터를 일련의 과정을 거쳐 가져와 다시 뷰에 나타나는 패턴입니다.
- 해당 패턴을 사용하여 유저가 직접적으로 데이터에 접근하지 못하게 하여 보안성을 높이고 해당 역할만 할당하기에 나중에 유지보수가 용이하다는 장점이 있습니다.


## 스프링에서 의존성 주입(DI) 방법 3가지 방법
**의존성 주입이란?<br>**
객체를 직접 만들어 사용하는 것이 아닌 주입받아 사용하여 의존성을 줄여주게 됩니다.<br>

**- 필드 주입<br>**
객체 자체에 @Autowired 를 적용하여 주입<br>
코드가 간결해지지만, 의존관계를 명확히 파악할 수 없고, 필드 주입 시에는 final선언이 안되어 객체가 변할 수 있습니다.
```java
@Controller
public class Controller{
    @Autowired
    private Service service;
}
```
**- 생성자 주입<br>**
생성자 함수를 만들어 해당 함수에 @Autowired 를 적용<br>
현재 가장 권장되는 방식으로 final선언으로 객체의 불변성을 확보할 수 있어 유지보수가 용이합니다.
```java
@Controller
public class Controller {
    private final Service service;

    @Autowired //생성자가 하나일 때는 생략가능
    public Controller(Service service) {
        this.service = service;
    }
}
```
Lombok의 @RequiredArgsConstructor 를 이용하여 간결하게 만들 수 있습니다.
```java
@Controller
@RequiredArgsConstructor
public class Controller {
    private final Service service;
}
```
**- setter 수정자 주입<br>**
setter 메서드에 @Autowired 를 적용<br>
변경 가능성이 있는 의존 관계에서 사용하고, final선언할 수 없습니다.
```java
@Controller
public class Controller {
    private Service service;
    
    @Autowired
    public setService(Service service){
        this.service = service; 
    }
}
```

## JPA의 장점과 단점
**JPA란?<br>** 자바 ORM기술의 표준으로 SQL을 작성하지 않고 객체를 DB에 저장하게 도와주는 기술입니다.<br>

**장점**
- 통일된 SQL문을 작성해주므로 SQL문의 오류를 줄일 수 있습니다.
- 데이터베이스의 종류가 달라져도 일관된 코드를 사용하므로 유지보수가 용이합니다. <br>

**단점**
- 내부에서 SQL문을 작성하므로 직접SQL문을 작성하는 것 보다 성능이 떨어집니다.
- JPA를 사용하기 위해 학습해야 할 것들이 많습니다.
## HTTP GET 요청과 POST 요청의 차이
**HTTP란?<br>** 웹 상에서 클라이언트와 서버 사이에 데이터를 주고 받을 수 있는 프로토콜입니다.

**GET**
- get은 서버로부터 데이터를 조회하기 위해 설계된 메소드로, 데이터를 body에 담지 않고 URL끝에 ?로 이름과 값을 가지는 쿼리스트링을 통해 전송합니다.
- get요청은 오로지 데이터를 읽을 때만 사용하기에 같은 요청을 하면 항상 같은 응답을 받을 수 있기에, 데이터를 변경하는 연산에는 적합하지 않습니다.
- 만약 get요청이 성공하면 200(Ok) HTTP 응답코드를 반환하게 됩니다.

**POST**
- post는 리소스를 생성/변경하기 위해 설계된 메소드로, 데이터를 HTTP 메세지의 body에 담아서 전송합니다.
- post요청은 서버에 동일한 요청을 여러 번 하더라도 응답은 항상 다를 수 있습니다.
- 만약 성공적으로 요청이 완료되면 201(Created) HTTP 응답코드를 반환합니다.