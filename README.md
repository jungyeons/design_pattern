# ⚙️ 디자인패턴 

## 📋 목차
> ### 📌 00. 디자인패턴을 학습하기 앞서
> ### 📌 01. 인스턴스를 효율적으로 생성(6) 
> ### 📌 02. 변경되는 부분을 분리(7)
> ### 📌 04. 동일시 취급(6) 
> ### 📌 05. 인스턴스를 관측과 단순화(4)


<br>
<br>

## 📌 전체 정리 그림 

<img src="https://github.com/user-attachments/assets/f9e9d68e-f339-48be-bd8a-3f537b56cf16" width="800" height="500"/>

<br>
<br>

## 📌 00. 디자인패턴을 학습하기 앞서 

### 1. UML에서 중요한 포인트 

> ### 👉 화살표 방향
- A -> B 
  - A가 B를 알고있다. 이는 B가 변경되면 A에도 영향이 미침
  - 의존하고 있다고도 표현함 

> ### 👉 클래스의 관계
- A -Uses-> B : A가 B를 사용한다
- A -Creates-> B : A가 B를 생성한다
- A -Notifies-> B : A가 B를 통지한다

> ### 👉 시퀀스 다이어그램은 "시간에 따라 변동하는 것(동적인 관계)"


### 2. 디자인패턴이란?

> ### 👉 OOP 설계를 추상화한 패턴 
- OOP를 설계하다 보면, 자주 사용되는 패턴이 있음. 이를 정리해서 패턴처럼 이용하는 것 

> ### 👉 설계도를 보는 태도
- 누가 누구를 알고 있는지
- 클래스, 인터페이스마다의 역할이 무엇인지
- 무엇을 위해 분리되어 있고 그룹핑되어 있는지
- OOP의 핵심인 "변경에 유리한 설계"를 어떻게 충족시키는지

<br>
<br>

##  📌 01. 인스턴스를 효율적으로 생성(5)

### 1. Singleton : 자원 절약, 인스턴스 1개 공유

<img src="https://github.com/jongheonleee/design_pattern_java/assets/87258372/d2563b0e-b38e-464b-9427-6d6f0b29496a" width="500" height="500"/>
<br>
💥 (그림 잘못됨, singleton 이라는 변수가 Method Area에 저장되는 거고 해당 변수가 Heap에 Singleton 객체를 참조하고 있는 형태가 맞음)

> ### 👉 인스턴스 하나만 생성하고 공유한다
  
- 자원을 절약하려는 의도, 무분별하게 new 연산자 하는 것을 막음 
- 인스턴스가 하나만 생성되는 것을 보증 해야한다 
- 공유하기 때문에 멀티쓰레드 환경에서 인스턴스 변수(iv) 값이 임의로 변경되는 것을 막아야한다
- iv가 없는게 좋음, 있어야하면 동기화 처리가 된 것을 사용하거나 상수를 사용해야함, 또한 iv에 계산된 값을 저장하기 보다는 메서드로 반환하는 게 좋음

> ### 👉 스프링에서 컨테이너에 빈 등록할 때, 기본적으로 Singleton으로 등록

- 빈으로 등록할 객체는 정보 공유가 가능한지, 멀티 쓰레드 환경에서 iv 오염이 발생하지 않는지 고려

<br>

### 2. Flyweight : 자원 절약, n개의 인스턴스를 한번만 생성하고 등록하여 공유

<img src="https://github.com/jongheonleee/design_pattern_java/assets/87258372/3dd289aa-297c-48d0-a635-67251501da59" width="500" height="500"/>

> ### 👉 n개의 인스턴스를 한번만 생성하고 맵에 등록하여 공유한다  

- Singleton 의 확장 버전, Singleton은 인스턴스 1개 생성을 절약하려는 의도. Flyweight는 인스턴스 n개 생성을 절약하려는 의도
- FlyweightFactory에 Singleton 적용, Map으로 생성한 인스턴스를 등록하고 관리
  - Map은 대용량 데이터 저장할 때 유용함. 내부적으로 해시 함수를 통해 데이터 조회함(충돌 발생을 낮추려는 의도)
  - 따라서, 소용량의 경우 Flyweight패턴과는 어울리지 않음. 예를들어 enum의 경우 여러개의 상수로 묶어도 맵으로 등록하지 않음 
  
- 공유하기 때문에 멀티 쓰레드 환경에 주의해야함. 즉 Flyweight를 적용할 인스턴스가 무거운지, 공유 가능한 인스턴스인지 따져 봐야함
- getFlyweight()메서드가 동기화 처리되야함. 예를 들어, 서로 다른 쓰레드가 동시에 같은 인스턴스를 사용하길 원하고 해당 인스턴스가 없는 경우 여러번 생성될 수 있음 

> ### 👉 스프링에서 컨테이너 구조와 매우 유사

- 스프링은 제어의 역전, 즉 사용과 생성을 분리함. 따라서, 스프링 컨테이너에서 빈을 생성 및 등록 그리고 관리해줌(생성). 개발자는 해당 빈을 사용하기만 하면됨
- 물론, 빈은 크게 두 부류로 등록됨, Singleton과 Prototype으로 등록됨
- Flyweight는 Singleton 사용함 

<br>

### 3. Prototype : 원형 등록, 필요시 복제품 사용 

<img src="https://github.com/jongheonleee/design_pattern_java/assets/87258372/a498a8d1-ee5a-4082-9c08-a72648692c87" width="500" height="500"/>

> ### 👉 생성이 어렵거나 복잡한 인스턴스를 생성하고 맵에 등록하여 추후에는 복제해서 사용 

- 사용 시기
    - (1) 종류가 많아 클래스로 정리하기 어려움 : 다형성 적용된 배열, 타입이 매우 많음(배열 생성하는 과정이 다른 인스턴스와 다른 이유임)
    - (2) 인스턴스 생성이 복잡함 : 한번만 생성하고 맵에 등록함, 추후에는 등록된 인스턴스를 복제해서 사용
    - (3) 프레임워크와 인스턴스를 분리 : 프레임워크를 특정 클래스에 의존하지 않게하려는 의도

- 얕은 복사(값 그대로 복사), 깊은 복사(참조하는 인스턴스도 복사)인지 고려
- Flyweight 는 참조값을 줌, Prototype 은 복사한 인스턴스를 줌(새로운 인스턴스)
- 선언부와 구현부가 분리되어 있음, 선언부는 설계도로 변경 주기가 낮음, 하지만 구현부는 변경 주기가 높음

> ### 👉 스프링에서 컨테이너 구조와 매우 유사

- 스프링은 제어의 역전, 즉 사용과 생성을 분리함. 따라서, 스프링 컨테이너에서 빈을 생성 및 등록 그리고 관리해줌(생성). 개발자는 해당 빈을 사용하기만 하면됨
- 물론, 빈은 크게 두 부류로 등록됨, Singleton과 Prototype으로 등록됨


<br>

### 4. Builder : 인스턴스 생성이 복잡한 경우, 인스턴스 생성을 해주는 클래스 활용

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/00ca4cee-cd82-40bd-afe4-fe65c216f3e8" width="500" height="500"/>

> ### 👉 복잡한 구조를 가진 인스턴스인 경우 빌더 패턴 활용
  - 이때는, 생성을 관장하는 객체(Director)와 생성 처리 객체(Builder)를 활용
  - 내부적으로 Bridge와 Template Method 적용
  - 이를 통해 복잡한 구조를 가진 인스턴스를 쉽게 생성함 

> ### 👉 생성자에 매개변수가 많은 경우(4~5 이상)
  - 문제점, 생성자 오버로딩이 많이 일어남, 생성자 매개변수 순서 정보가 명확하지 않음 
  - 내부적으로 Builder 클래스를 선언해서 사용해서 해결

<br>

### 5. Factory Method : '팩터리' 추상화, 상위에 '팩터리'를 추상화 하위에서 구체화

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/64fb1aad-569d-44d5-b9b2-31a96cd270e0" width="500" height="500"/>

> ### 👉 '인스턴스 생성 흐름(팩터리)'과 '인스턴스 생성 방법(구체 팩터리)'을 패키지 단위로 분리
- 인스턴스 생성 흐름, 방법을 변경 관점에서 분리
- 이를 패키지 단위로 분리함
- 상위 패키지는 1개, 하위 패키지는 n개 -> OCP 충족

> ### 👉 템플릿 메서드의 확장 버전
- 템플릿 메서드는 클래스 단위에서 분리, 상위 클래스 - 하위 클래스
- Factory Method는 패키지 단위에서 분리, 상위 패키지 - 하위 패키지

> ### 👉 n개의 팩터리와 인스턴스를 묶어서 관리 

<br>

### 6. Abstract Factory : '팩터리'와 '인스턴스의 부품' 추상화

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/ccb63493-b18a-49ba-99b8-9122010a1772" width="500" height="500"/>

> ### 👉 Factory Method의 확장버전
- 두 패턴 모두 '팩터리'를 추상화 했다는 점에서 유사함
- 하지만, Abstract Factory는 복잡한 인스턴스 생성에 적합한 패턴, 인스턴스를 부품으로 쪼갬, 그리고 부품을 추상화해서 관리
- 추상화된 부품을 결합해서 추상 인스턴스를 생성 -> 다형성 

> ### 👉 추상적인 것에 주목
- 구체적인 것에 주목하지 않고 추상적인 것에 주목
- OCP 충족하기 위함 


<br>
<br>

##  📌 02. 변경되는 부분을 분리(8)

### 7. Iterator : 반복해서 처리를 분리함(사용과 구현), 구체적인 반복 처리 과정을 분리(조건식, 반복 처리)

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/9459b35d-8b18-48f6-afd3-b4f508a4bf4c" width="500" height="500"/>

> ### 👉 for 문에서 조건식 부분을 외부로 분리함
- 조건식에서 특히 에러가 많이 발생, 조건식은 변경 주기가 잦음, 그 외에는 불변
- iterator를 사용하는 인스턴스는 단순히 반복 처리해서 사용만함. 즉, 반복 처리 과정 및 해당 집합의 내부 사정을 몰라도됨
- 대표적으로 자바에는 Collection의 하위 클래스인 List, array, set이 있음
  - set의 경우 순서가 없음, 하지만 treeSet을 탐색하는 순서가 있기 때문에 그 방법에 따라 반복 처리가 달라짐
    - (1) 전위
    - (2) 중위
    - (3) 후위
    - (4) 레벨(bfs)  

<br>

### 8. Template Method Pattern : 세부 내용(변경이 자주 일어나는) 부분을 분리, 상속을 통해 코드 완성, 상위의 전체 틀을 구성하고 하위에서 세부 내용을 결정함 


<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/de37b59b-91d9-4b0d-a89b-30a3fb1c16c3" width="500" height="500"/>


> ### 👉 변경되는 것과 변경되지 않는 것의 분리, 즉, 불변과 가변의 분리
- 기존에 서로 관련도 높은 것들 끼리 묶음. 하지만, 그 내부에서 변경 시점이 서로 다른 경우 분리함
- 변경되는 것과 변경되지 않는 것의 분리
- 상위 클래스에서 뼈대를 구성하고 하위 클래스에서 구체적인 내용 결정



> ### 👉 전략 패턴과 같음. 하지만, 코드를 완성하는 방식에는 차이가 있음 
- 코드를 상속을 통해 완성시킴(추상 클래스를 상속 받아서 완성시켜서 사용)
- 전략 패턴의 경우 알고리즘을 주입 받아서 코드 완성
  - 생성자 주입
  - 매개변수 전달

> ### 👉 자바에서의 적용 사례 : Object, Enum, [ ] (배열), Thread(상속 구현)

> ### 👉 브릿지 패턴과의 차이, 추상 메서드 개수
- 템플릿 메서드 패턴의 경우 하나의 추상 메서드에 대한 내용
- 브릿지 패턴은 n개의 추상 메서드에 대한 내용
  - n개의 추상 메서드를 인터페이스로 추출해서 선언(기능)과 구현을 분리한 형태 

<br>

### 9. Strategy : 알고리즘을 분리해서 주입 받아서 사용(특정 문제를 다양한 방식으로 처리가능)

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/0f20b692-fcff-4e2f-88e0-40092bd68a80" width="500" height="500"/>


> ### 👉 변경되는 것과 변경되지 않는 것의 분리, 즉, 불변과 가변의 분리
- 기존에 서로 관련도 높은 것들 끼리 묶음. 하지만, 그 내부에서 변경 시점이 서로 다른 경우 분리함
- 변경되는 것과 변경되지 않는 것의 분리
- 변경이 자주 일어나는 알고리즘을 외부로 부터 주입받아서 구체적인 알고리즘 내용 완성 시킴 


> ### 👉 템플릿 패턴과의 차이점
- 코드를 완성하는 방식이 다름
- 전략 패턴은 코드를 주입받아서 완성시킴
  - 생성자를 통해 주입 
  - 매개변수로 전달 

> ### 👉 자바에서의 적용 사례 : sort("비교 대상", "비교 기준"), Comparator 전달, Thread 생성시 Runnable 매개변수로 전달 

> ### 👉 브릿지 패턴과의 차이, 추상 메서드 개수
- 전략 패턴의 경우 하나의 추상 메서드에 대한 내용
- 브릿지 패턴은 n개의 추상 메서드에 대한 내용
  - n개의 추상 메서드를 인터페이스로 추출해서 선언(기능)과 구현을 분리한 형태 

<br>

### 10. Bridge : 기능(선언)과 구현을 타입 단위로 분리

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/cdb6e844-e33c-4054-838f-2e52836ed288" width="500" height="500"/>


> ### 👉 변경되는 것과 변경되지 않는 것의 분리, 즉, 불변과 가변의 분리
- 위의 Template Method, Strategy는 1개의 추상 메서드를 다뤘다면, Bridge는 n개의 추상 메서드를 다룸
  - 인터페이스 선언, 클래스 단위로 주입 받음 
  - 결론적으로는 기능과 구현을 분리 
  - 즉, Template Method, Strategy는 메서드나 특정 알고리즘 단위로 분리했다면 Bridge는 클래스 단위로 분리
    - 그 결과 기능의 클래스 계층과 구현의 클래스 계층으로 나뉨

- 더욱 면밀히 얘기하면, 기능의 클래스 계층과 구현의 클래스 계층을 분리함(두 개의 독립된 클래스 계층으로 나눔)
  - 새로운 기능을 추가할 때는 각 계층에 맞는 곳에 상속으로 추가하면됨 
  - 상위에서는 기본 기능 구상, 하위에서는 새로운 기능 추가 

<br>

### 11. State : 상태(지속적으로 변경)를 타입 단위로 분리 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/de4170c3-a887-46f2-9002-4ba69801175d" width="500" height="500"/>

> ### 👉 if-else 문은 변경이 자주일어남
- 상태는 지속적으로 변경되고 추가됨
- 이를 if-else문으로 매번 확인하는 것은 OOP적 설계가 아님
- 따라서, 상태를 타입 단위로 정의해서 if-else문을 쪼개서 해결
- OOP적 설계 충족, 상태가 추가되어도 해당 상태를 새로 정의해서 외부에서 주입해주면됨 

> ### 👉 Strategy, Bridge와 매우 유사함, 결국에는 분리가 핵심 
- 셋 다 공통점은 변경이 많이 발생하는 것을 외부에서 주입하는 형태를 구상함
- Strategy는 한 기능(알고리즘,,,), Bridge는 n개를 타입으로 묶어서 분리(기능과 구현 분리), State는 상태를 타입으로 정의해서 분리

> ### 👉 Singleton 사용, 상태는 하나여야함

> ### 👉 타입으로 정의함의 의미 컴파일러를 최대한 활용해야한다.
- Cloneable 을 구현한 객체만 clone()을 호출할 수 있음
  - 이를 컴파일러가 체크해줌. 이런 것이 좋은 코드
- 마찬가지로 해당 패턴도 상태를 타입으로 정의해서 if-else문을 쓰는 것을 지양하고 컴파일러가 타입 체크하는 것을 최대한 활용함
- 이는 Command 패턴도 적용됨 

<br>

### 12. Visitor : 데이터 구조와 특정 처리를 분리


<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/55519de7-2b6f-4211-adcc-7ca2a3594f3b" width="500" height="500"/>


> ### 👉 '데이터 집합'과 '작업'을 분리
- 해당 패턴의 핵심, 처리를 데이터 집합과 분리하는 것
- 이를 통해 데이터 집합을 부품으로서의 독립성을 높임, 즉 부품처럼 여러군데에서 사용할 수 있게 만듦
- 처리와 같이 결합하면, 부품으로서 사용하기 어려움
- 또한, 처리를 담당하는 클래스는 해당 역할에만 집중할 수 있어서 좋음

> ### 👉 각 역할에 집중하는 구조
- '데이터 집합'으로 분리된 클래스와 '처리'로 분리된 클래스는 각자의 역할에 집중 할 수 있음
- SRP 충족

> ### 👉 Composite, Template Method
- '처리' 역할을 담당하는 클래스를 추상 클래스로 선언 -> Template Method
- '데이터 집합' 역할을 담당하는 클래스를 재귀적 구조가 용이하게 만듦 -> Composite

> ### 👉 Element의 메서드 accept(Visitor v), Visitor의 메서드 visit(Element e)

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/3683201f-7248-4f18-9344-9299ac814cf1" width="500" height="500"/>
<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/662b41dd-91fa-4669-9b44-d8538481e357" width="500" height="500"/>

- 해당 패턴은 코드가 재미있음. 이를 더블 디스패치라고도 함

<br> 

### 13. Adapter : 특정 클래스를 부품으로서 연결하다 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/e1bf5d8d-15e7-43a4-801c-1b91f29d542f" width="500" height="500"/>

> ### 👉 '제공된 것' 과 '제공할 것'을 연결
- 특정 클래스를 부품으로써 재사용하고 싶을 때 주로 사용하는 패턴
- 핵심은 둘 사이를 연결함, 즉 코드를 동적으로 완성
  - 코드를 완성하는 방법
    - 상속
    - 구현


<br>
<br>

##  📌 03. 동일시 취급한다(6)

<br>

### 14. Composite : 내용과 그릇을 동일시 취급한다, 묶어서 동일시 취급한다 


<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/9a3cb768-7edc-4adc-922c-2f44629b9f79" width="500" height="500"/>

> ### 👉 동일시 취급하여 재귀적 구조를 형성함 
- 내용과 그릇의 공통점을 추출해서 상위 추상 클래스 정의(Template Method)
- 재귀적으로 해당 타입을 추가할 수 있음, 예를 들어서 파일과 폴더 구조가 있음(트리 형성)
- 재귀구조의 장점은 유연함, 동적으로 추가하거나 삭제하기 편리함 




<br>

### 15. Decorator : 장식과 내용물을 동일시 한다, 상속과 포함을 동시에 구현


<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/1bf3bf68-7313-4a1c-a45e-75dbf473fe5d" width="500" height="500"/>

> ### 👉 상속과 포함을 동시에 구현 
- 군인에게 총, 칼, 대포, ,,, 여러 무기를 장착해도 군인임은 변함없음
- 즉, 장식과 내용을 분리하고 내용에 장식을 지속적으로 추가해도 내용이 바뀌지 않음(다형성)
- 장식과 내용을 분리해두었다가 맥락에 맞게 합쳐서 사용함
  - 여러기능을 동적으로 추가할 수 있음

> ### 👉 Template Method, Bridge 적용 
- 장식과 내용물의 공통 부분을 상위 추상 클래스로 정의(Template Method)
- 생성자의 매개변수로 내용을 주입(Bridge), 겉에는 껍데기 클래스로 부가 기능을 가지고 있음
- Display d = new Border(new StringDisplay); - Border : 껍데기, StringDisplay : 알맹이 
  - 외부에서 아무리 껍데기를 추가해도 알맹이는 변하지 않음 
  - Display d = new BorderN(...new Border2(new Border(new StringDisplay))...);

> ### 👉 자바에서 활용
- IO 관련 클래스에서 많이 활용됨
  - Scanner sc = new Scanner(new InputStream()) ... 

<br>

### 16. Proxy : '대리인'과 '본인'을 동일시함, 대리인(매니저)을 통해 특정 객체를 사용한다 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/6e9d975c-e003-4d66-ace6-1af98276f4d6" width="500" height="500"/>

> ### 👉 '대리인(매니저)' 와 '본인'을 분리하여 동일시 취급
- 근본적인 이유는 클라이언트가 '대리인(매니저)'를 통해서 '본인'을 사용
- 따라서, 인터페이스로 '대리인'과 '본인'을 동일시 취급
  - 이는 Decorator, Composite과 유사함 
- 또한, '본인'을 사용하기 어려운 상황에서 '대리인'을 사용하기 위함도 있음
  - 가령, '본인'이 매우 바쁜 경우


> ### 👉 Decorator 패턴과 매우 유사함 
- Decorator는 포함과 상속으로 이루어짐. 이때, 상속은 '장식'을 '내용' 취급하기 위함(다형성 활용)
- 이와 같이 Proxy에서도 다형성을 통해 '대리인'과 '본인'을 동일시 취급함

> ### 👉 스프링 AOP 에서 활용
- 스프링 AOP의 대표적인 기술로는 Tx가 있음
- @Transactional을 사용하면 런타임 시점에 try-catch 문이 붙음 

<br>

### 17. Command : n개 명령어를 묶어서 하나의 타입으로 표현, 명령어를 집합에 담기 위함  

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/89800a75-d049-4ba8-a4d9-ae7e330c96f7" width="500" height="500"/>

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/a6c093ea-7bed-4daa-bf48-3d38fb38ec34" width="500" height="500"/>


> ### 👉 명령을 하나의 타입으로 표현해서 집합으로 사용 
- 명령어의 경우, 스택에 쌓임.
- 이때, 스택에 쌓이기 위해선 n개의 명령어가 서로 같은 타입이여야함
- 따라서, n개의 명령어를 특정 타입으로 동일시 취급함(다형성활용)

> ### 👉 Composite 패턴과 유사함 
- Composite 패턴은 '내용'과 '그릇'을 동일시 취급하여 재귀적 구조를 형성함. 예를 들어 폴더와 파일 구조
- 이와 마찬가지로 Command 패턴은 n개의 명령어를 동일시 취급하여 Composite 패턴의 효과를 볼 수 있음

### 18. Chain Of Responsibility : n개의 해결책을 하나의 해결책으로 묶음, 동적으로 원하는 해결책으로 처리하기 위함 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/11cdb904-97c0-4c7d-8e12-54d216656efe" width="500" height="500"/>
<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/0452798d-1d2c-46da-a9c2-600a04381444" width="500" height="500"/>

> ### 👉 책임을 떠넘기다?
- '해결책'을 하나로 묶음. 이때 링크드 리스트 구조로 매 순회하면서 문제를 해결할 수 있는것을 찾음
- 특정 해결책이 해결을 못하면 자신과 연결된 해결책으로 넘김 

> ### 👉 Composite, Command 유사함
- 모두 여러 객체를 동일시 취급하는 맥락이 같음

> ### 👉 재미있는 코드 부분

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/961dbc2d-268e-4623-b238-a63f52e5fe22" width="500" height="500"/>
<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/5e51f4c5-9a1a-4a33-9223-cec5a259727d" width="500" height="500"/>

<br>

### 💥 19. Interpreter : '토큰'을 동일시 하여 파싱 트리 형성 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/94a098f8-e303-40d4-ab5f-31051b487252" width="500" height="500"/>


> ### 👉 컴퓨터 내의 '미니 언어' 정의 
- 프로그램의 문제를 해결하기 위해 '미니 언어' 정의
- 이를 통해 문제를 '미니 언어'로 해결하여 하나의 '미니 프로그램' 생성 



<br>
<br>

##  📌 03. 인스턴스를 관측과 단순화(4)
<br>

### 💥 20. Facade : 복잡한 시스템을 가리고 외부에 인터페이스로 제공, 복잡도를 낮춤 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/16f25005-857e-46f2-9a09-e5ec2c8da0f7" width="500" height="500"/>

> ### 👉 내부와 외부를 분리하여 전체 복잡도를 낮춤
- Facade가 내부 시스템을 알고있음(관측), 이를 외부에 인터페이스로 제공
- 외부에서는 내부 시스템 사정을 알필요없이 특정 작업 수행 가능
  - 사용과 내부 상황을 분리하여 단순화함 


> ### 👉 Iterator 와 유사
- Iterator의 경우, 반복 처리 작업과 사용을 분리함. 따라서, 사용하는 쪽에서는 내부 사정을 알필요없음
- 즉, 복잡도를 낮춤. 단순화함 

<br>

### 💥 21. Mediator : 객체 간의 복잡한 소통을 외부에서 관리, '중재자'가 통신을 담당  

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/30d720f9-cde7-4352-8533-80c15eb03c80" width="500" height="500"/>

> ### 👉 복잡도를 낮추기 위해 객체의 역할을 나눔
- 문제점은 n개의 객체가 소통하는 과정이 복잡함
- 이를 서로 직접적으로 소통하는 것이 아닌 '중재자'를 둬서 간접적으로 소통
- 따라서 객체 간의 소통의 복잡함을 해결 

> ### 👉 확장에 용이함 -> OCP
- 여러 중재자 배치 가능, 또한 여러 객체가 생겨도 복잡도는 상대적으로 적게 증가
- 책임과 역할을 분리했기 때문 -> '중재자', '소통자'

<br>

### 💥 22. Memento : 특정 시점의 객체를 저장과 복원을 함  

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/04e3672f-ac4d-4381-aec1-b7a118969dfa" width="500" height="500"/>

> ### 👉 인스턴스를 저장과 복원함 
- 특정 시점의 인스턴스를 저장해 둠
- 추후에 해당 인스턴스를 복원하기 위함 

> ### 👉 확장에 용이함 -> OCP
- 여러 중재자 배치 가능, 또한 여러 객체가 생겨도 복잡도는 상대적으로 적게 증가
- 책임과 역할을 분리했기 때문 -> '중재자', '소통자'

<br>

### 💥 23. Observer : 상태 변화를 외부로 분리, '관찰자'가 상태 변화가 관측되면 특정 작업 처리 

<img src="https://github.com/jongheonleee/design_pattern/assets/87258372/b1bd9c4f-b3ec-4002-b316-ca9180468474" width="500" height="500"/>

> ### 👉 객체의 상태 변화를 외부에서 관측하고 작업을 처리함 -> '관찰자'
- 특정 상태 변화에 따른 작업 처리를 n개로 확장하기 용이함 
- OCP 충족


<br>



