# Java

- [Java의 특징](#feature)
- [Java의 철학](#philosophy)
- [IDE없이 컴파일하고, 실행하기](#run-compile-without-ide)
- [Wrapper Class](#wrapper-class)
- [Date](#java-date)
- [Javadoc](#javadoc)

<br>

## <a name="feature"></a>Java의 특징

<img src="http://www.mediafire.com/convkey/af35/opivqpoyrlo8n5gzg.jpg" width="500" />

- Java는 'Java의 아버지'로 불리우는 <a href="https://www.linkedin.com/in/jamesgosling/" target="_blank">James Gosling</a>가 1995년, Sun Microsystems.Inc,(이하 Sun)에서 다른 연구원들과 함께 개발한 프로그래밍 언어이다. 1995년에 Java 1.0을 발표했다. 
- 2009년 Oracle이 Sun을 56억불에 Oracle에 <a href="https://www.cbsnews.com/news/oracle-buys-sun-microsystems-for-74b/" target="_blank">인수</a>하면서 모든 권리가 Oracle로 이전되었다.
- Java는 [**객체지향언어**(Object Oriented Language)](#oop)<sup>`1`</sup>이다.
- Java가 등정하기 이전의 객체지향언어인 C++ 문법을 기본으로 개발되었다.
- Java가 다른 언어와 다른 가장 큰 특징은  [**JVM**](#jvm)<sup>`2`</sup> 위에서 작동되기 때문에 [**플랫폼 독립적**](#os-independent)<sup>`3`</sup> 이라는 점이다. HW에 맞게 완전하게 컴파일된 상태가 아니라 실행 시에 해석(Interpreter)되기 때문에 속도가 느리다는 단점을 가지고 있었다. 그러나 최근엔 JIT 컴파일러와 최적화된 기술로 인해 속도의 격차가 크게 줄었다.
- Java는 C/C++와 달리 **메모리를 자동으로 관리**해준다. **Garbage Collector**(이하 GC)가 사용하지 않는 메모리를 자동으로 정리해준다.

<br>

## <a name="philosophy"></a>Java의 철학

- [객체 지향 방법론](#oop)<sup>`1`</sup>을 사용해야 한다.
- 프로그램(바이트 코드)가 여러 운영체제에서 실행될 수 있어야 한다. ([플랫폼 독립성](#os-independent)<sup>`3`</sup>)
- 컴퓨터 네트워크 접근 기능이 기본으로 탑재되어 있어야 한다.
- 원격 코드를 안전하게 실행할 수 있어야 한다.
- 다른 객체 지향 언어들의 좋은 부분만 가지고 와서 사용하기 편해야 한다.

<br>

## <a name="run-compile-without-ide"></a>IDE없이 컴파일 하고, 실행하기

운영체제에 jdk를 설치했고, 환경변수 설정까지 마쳤다면 IDE없이도 자바 파일을 컴파일해서 클래스 파일을 생성할 수 있다. 물론 실행까지도 할 수 있다. 

**1. 자바 파일 생성(코딩)**

![](http://www.mediafire.com/convkey/b82c/qze1hsgyvs1863gzg.jpg)



**2. `javac` 이용해서 컴파일**

![](http://www.mediafire.com/convkey/b82c/qze1hsgyvs1863gzg.jpg)



**3. 실행**

![](http://www.mediafire.com/convkey/2006/qriceyec0qlbmtfzg.jpg)

실행할 땐 `java JavaYahac` 처럼 파일 포맷없이 클래스 파일명으로 명령을 한다. 그럼 현재 디렉토리에서 이름이 일치하는 클래스를 찾아서 실행한다. 

실행 순서는 `main()`를 먼저 수행하고, `main()`에서 호출하는 메서드들을 수행하는 순서로 수행된다.

<br>

## <a name="wrapper-class"></a>Wrapper Class

기본형 데이터타입의 객체화를 가능하게 도와주는 클래스. `java.lang` 패키지 안에 포함되어있다.

- 기본형 타입 : `int`
- 객체형 타입 : `Integer`



Wrapper Class는 다음과 같은 기능을 수행한다.

<img src="http://www.mediafire.com/convkey/e0bf/3kqk5j823suxigbzg.jpg" />

**오토 박싱 (Auto Boxing)**

기본 타입 데이터를 객체 타입의 데이터로 자동 형변환해주는 기능

**오토 언박싱 (Auto Unboxing)**

오토박싱과 반대로 객체 타입의 데이터를 기본형 타입 데이터로 자동 형변환



### References

- [프로그래머스 - [자바 중급] java.lang 패키지/오토박싱](https://youtu.be/Eofo8_xZbfk)

<br>

## <a name="java-date"></a>Date 객체

날짜를 출력하는 객체이다.

기본 객체 사용법은 다음과 같다.

```java
import java.util.Date;

class date_tutorial {
  public static void main(String[] args){
    Date date = new Date();
    System.out.println(date);
  }
}
```

이렇게 `Date` 객체를 그대로 사용할 경우 출력결과는 다음과 같다.

<img src="http://www.mediafire.com/convkey/ff9e/ltldv7wa674l88lzg.jpg" />

포맷을 내가 커스터마이징 하고자 할 경우 `SimpleDateFormat` 객체를 사용하면 된다.

```java
import java.util.Date;
import java.text.SimpleDateFormat;

class date_tutorial {
  public static void main(String[] args){
    Date day = new Date();
    SimpleDateFormat date = 
      new SimpleDateFormat("yyyy년 MM월 dd일");
    SimpleDateFormat clock = 
      new SimpleDateFormat("a hh시 mm분 ss초");
    
    System.out.println("서버 실행한 날짜는 "+date.format(day));
    System.out.println("서버 실행한 시각은 "+clock.format(day));
  }
}
```

![](http://www.mediafire.com/convkey/78db/kx7v4ezlus1w9xnzg.jpg)

<br>

## <a name="javadoc"></a>Javadoc

주석은 프로그램에 반영되는 코드가 아니다. 코드를 설명하는 문구이다.

내가 작성한 코드일지라도 며칠 지나서 코드를 다시보면, *'진정 내가 짠 코드란 말인가?'* 하는게 사실이다. 작성자 본인도 그런데, 함께 프로젝트를 할때 다른 동료들은 오죽할까. 

적절하게 코드를 설명하는 주석을 남기는 것이 좋다.

자바에서 주석을 남기는 방법은 두 가지가 있다.

**1. 한 줄 짜리 주석**

```java
// 이게 한 줄 짜리 주석이다.
```

**2. 두 줄 이상의 주석**

```java
/*
보통 IDE에서 /* 입력하고 엔터만 치면 자동으로 완성이 된다. 
위아래 기호 사이에 이렇게 작성하면 장문의 주석도 작성이 가능하다.
그러나 주석은 장황하지 않게 깔끔하게 쓰도록 하자.
\*
```

그리고 주석의 역할을 하면서 주석만 따로 뽑아서 문서로 만들어주는 **Javadoc**이라는게 있다.

**3. javadoc**

```java
/**
 * @author DevAndy
 * @return 매핑된 URI에 해당하는 문서를 반환
*/
@GetMapping("/login")
public String login(){
   return login;
}
```

Javadoc에서 자주 사용하는 구문은 다음과 같다고 한다.

- `@author`

  - 말 그대로 저자를 의미한다. 코드를 작성한 개발자의 이름을 작성하면 된다.

- `{@link}`

  - 다른 메서드/클래스를 연결할 때 사용하며, 다른 키워드와 사용할 때 유용하다고 한다.

- `@deprecated`

  - 오래되서 사용하지 않는 클래스와 인터페이스에 사용하며 `{@link}`와 사용하면 유용하다.

  - ```java
    /**
     * @deprecated 대신 {@link #new()} 사용한다.
     */
    @Deprecated
    public void old(){
      ..
    }
    ```

- `@see`

  - 클래스 이름을 연결할 때 사용한다.

    - ```java
      /**
       * @see com.lecturesearch.lecture.user.SocialType
       */
      ```

  - `@see #method`

    - ```java
      /**
       * @deprecated 대신 {@link #new()} 사용한다.
       * @see #new 에서 새로운 기능을 추가했다.
       */
      @Deprecated
      public void old(){
        ..
      }
      ```

  - `외부 URL링크`

    - ```java
      /**
       * @see <a href="https://github.com/youngjinmo/til">TIL</a>
       */
      ```

- `@since`

  - 클래스나 메서드의 작성 시점을 알려주는 키워드. 
  - API일 경우, API사용자가 어느 버전의 라이브러리에서 지원하는 기능인지를 알 수 있음.

- `@param`

  - Parameter의 이름과 용도를 알려주는 키워드.

- `@return`

  - 반환되는 데이터와 데이터 타입을 기술

- `@throws`

  - exception-class를 기술

- `{@code}`

  - 예제 코드를 첨부할 경우 사용하는 키워드.

  - HTML의 `<pre>` 키워드와 함께 사용해야 한다.

  - ```java
    /**
     * 클래스 설명
     * <pre>
     * {@code
     * Car car = new Car();
     * }
     * </pre>
     */
    ```

    

그래서 이걸 문서로 만드려면 터미널에서 클래스 파일이 디렉토리로 이동후 아래의 명령어를 입력해야한다.

```bash
javadoc -d docs [file.java]
```

그런데 아직 이 문서를 보는 방법을 모르겠다.. 궁금한데 내일 찾아보고 정리해야겠다..

<br>