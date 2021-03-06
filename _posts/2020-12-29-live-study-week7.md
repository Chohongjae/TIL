---
title: "라이브 스터디 7주차"
categories: 
  - LiveStudy
last_modified_at: 2020-12-29T23:00:00+09:00
---

# 목표
## 패키지
- 자바의 패키지에 대해 학습하세요.

## 학습할 것
- [package 키워드](#package-키워드)
- [import 키워드](#import-키워드)
- [클래스패스](#클래스패스)
- [CLASSPATH 환경변수](#CLASSPATH-환경변수)
- [-classpath 옵션](#-classpath-옵션)
- [접근지시자](#접근지시자)

### package 키워드
    자바 프로그램에서는 다수의 클래스를 정의하기 떄문에 애플리케이션이나 애플리케이션 내부의 기능 등에서
    클래스를 정리할 필요가 있다. 그러한 경우에 클래스를 계층적으로 정리하기 위해 사용하는 것이 패키지다.
    패키지의 선언에는 package 키워드를 사용한다.
    패키지의 선언은 소스 코드의 가장 선두 부분에 기술한다.
    
```java
package com.chongjae.javastudy;
```

    예를 들어 Messenger 애플리케이션을 개발할 때 패지키 구성을 고려해 보자.
    1. 사용자 관리 기능
    2. 메시지 기능
    3. 그룹 기능
    이러한 기능들이 있다고 가정했을 때
    
    com
     |
     | - - chohongjae
                |
                | - - - - - messenger
                                |
                                | - - - - - account
                                |             |
                                |             | - - - - - AccountController.java 
                                |             | - - - - - AccountService.java
                                | - - - - - message
                                |             |
                                |             | - - - - - MessageController.java 
                                |             | - - - - - MessageService.java
                                | - - - - - group
                                              |
                                              | - - - - - GroupController.java 
                                              | - - - - - GroupService.java
                                                   
    위와 같이 패키지를 이용하면 애플리케이션의 구성이 명확하게 되어 클래스가 갖는 역할을 보다 명확히 나타낼 수 있다.
    
    단, 패키지의 명명에 대해서는 주의가 필요한데 관례로 제 3자에게 제공하는 라이브러리나 프레임워크 등과
    패키지명이 중복되지 않도록 자신이 소유한 도메인명을 "역순"으로 해서 패키지명을 시작하도록 하고 있다.
    
    또한 패키지명과 클래스명을 조합한 이름(FQCN)이 중복된 클래스가 있다면 프로그램이 의도하지 않은 동작을 하는 경우가 있다.
    그렇기때문에 이 FQCN이 중복되지 않도록 기능이나 모듈마다 패키지명을 나누도록 한다.
    
### import 키워드
    위와 같이 선언한 패키지에 속한 클래스를 다른 파일에서 사용하기 위해서는 클래스 이름 앞에 패키지의 경로까지 포함한 풀 네임을 명시해 사용해야 한다.
    하지만 클래스를 사용할 때마다 매번 이렇게 긴 이름을 사용하는 것은 비효율적이므로, 자바에서는 import 키워드를 별도로 제공한다.

    import 문은 자바 컴파일러에 코드에서 사용할 클래스의 패키지에 대한 정보를 미리 제공하는 역할을 한다.
    따라서 import 문을 사용하면 다른 패키지에 속한 클래스를 패키지 이름을 제외한 클래스 이름만으로 사용할 수 있게 된다.
    
```java
package com.chongjae.javastudy;

import com.chohonjae.messenger.account.AccountController; // 해당 클래스만 사용
import com.chohonjae.messenger.account.*; // 해당 패키지의 모든 클래스를 사용
```
    
    만약 import com.chohonjae.* 이렇게한다고해서 chohongjae 패키지 아래에 이쓴 모든 하위 패키지의 클래스까지 포함하는 것은 아니다.
    또한 자바에서 가장 많이 사용하는 java.lang 패키지에 대해서는 import 문을 사용하지 않아도 클래스 이름만으로 사용할 수 있도록 해주고 있다.
    
```java
package com.chongjae.javastudy;

import static com.chohonjae.AccountController.staticMethod; // AccountController의 static 메소드인 staticMethod import 
import static com.chohonjae.AccountController.*; // AccountController의 static 메소드 전부 import
```

    JDK 1.5부터는 정적(static) 메소드를 더욱 쉽게 사용하기 위해서 static import 를 지원한다.
    그래서 위에 import 문과 같이 import 뒤에 static을 붙이면 해당 클래스의 static 메소드를 import 할 수 있다.    
    주의해야 할 것은 같은 클래스 내에 동일한 이름의 메소드가 있으면 클래스 자신의 메소드가 우선된다.

    
### 클래스패스
    클래스패스란 말 그대로 클래스를 찾기위한 경로이다. 

    java runtime(java 또는 jre)으로 이 컴파일된 .class 파일에 포함된 명령을 실행하려면,
    먼저 이 파일을 찾을 수 있어야 한다.
    class가 있는 위치에서 class를 실행하는 명령어를 입력하였을 경우에는 별도로 경로를 알려주지 않아도 된다.
    하지만 다른 class가 있는 위치와 다른 경로에서 실행한다면 실행할 클래스의 위치, 즉 클래스 패스를 명시해주어야
    해당 위치에서 클래스를 찾아 실행할 수 있다.
    또한 만약 클래스 패스를 설정하지 않았다면 디폴트는 현재 경로에서 class 찾게 된다.
    우리가 많이 사용하는 intellij IDE도 클래스 패스를 설정하지 않으면 기본적으로 현재 경로에서 파일을 찾는다.
    
    즉 JVM이 클래스를 찾아 사용해야 하는데 클래스들이 어디 있는지, 클래스 파일을 찾는데 기준이 되는 파일경로인 것이다.
    
    classpath를 지정할 수 있는 두 가지 방법이 있다.
    하나는 환경 변수 CLASSPATH를 사용하는 방법이고, 또 하나는 java runtime에 -classpath 플래그를 사용하는 방법이다.

### CLASSPATH 환경변수
    운영체제는 시스템 운영상 필요한 경로를 시스템에 지정해두고서 참조하는 것이 가능하다.
    이를 환경 변수라고 부르는데, 자바도 이곳에 필요한 경로를 설정해두고서 이용하는 것이 가능하다.
    
    JVM이 시작될 때 JVM의 클래스 로더는 시스템에 등록된 CLASSPATH라는 환경 변수를 호출한다.
    이 CLASSPATH에 지정된 경로를 모두 검색해서 특정 클래스에 대한 코드가 포함된 .class 파일을 찾고 JVM에 로드한다.
    그러므로 CLASSPATH 환경 변수에는 필수 클래스들이 위치한 디렉토리를 등록하도록 한다.
    
    클래스 패스는 "echo $CLASSPATH"로 확인하고 실제로 클래스 패스가 설정이 되어있지 않으면 빈칸이 나온다.

```java
export CLASSPATH=.:/Users/hongjae/develop:/Users/hongjae/java:...:경로...
```

    이렇게 CLASSPATH를 설정할 수 있고 이것은 JVM에게 프로그램을 실행할 때 ":" 로 구분 지어진
    "." 와 "/Users/hongjae/develop" 그리고 "/Users/hongjae/java"에 있는 클래스 파일도 찾아달라고 말하는 것과 같다.
    
    또한, 상기에서 보면 맨앞에 현재 경로를 의미하는 . 를 추가했는데,
    이는 자바 컴파일러가 명시적으로 CLASSPATH를 지정할 경우 JVM은 해당 경로만 탐색하기 때문에
    클래스 패스를 추가하는 것이 아니라 설정, 고정하게 되는 것이고 현재 경로를 보지 않게 된다.
    그렇기때문에 명령 프롬프트 상에서 위치한 디렉토리도 클래스 패스로 지정하고 싶다면 . 을 추가해주면 되는 것이다.
    
### -classpath 옵션
    환경 변수를 시스템에 설정하지 않고도 클래스 패스를 지정하는 것이 가능한데,
    이는 javac, java를 이용해서 실행할 때 –classpath 라는 옵션을 사용하는 것이다.

```java      
java –classpath .:bin HelloWorld
java –classpath .:/Users/hongjae/develop HelloWorld
```
    
    위와 같이 클래스들이 현재 디렉토리와 다른 경로에 존재한다면
    현재경로(".") 에 없다면 현재 디렉터리의 하위 디렉터리 중 bin 이라는 디렉토리에서 클래스를 찾아 혹은,
    현재경로(".") 에 없다면 /Users/hongjae/develop 이라는 디렉토리에서 클래스를 찾아라는 의미로 
    -classpath 옵션을 사용할 수 있다.
    
### 접근지시자(접근제한자)
    프로그램 개발에서는 예상 외의 필드 참조나 메서드의 호출에 따라 버그가 발생하는 일이 있다.
    그런 일을 막기 위해서 클래스나 변수, 메서드를 사용할 수 있는 범위(가시성)을 접근 제한자를 지정함으로써 제한할 수 있다.
    
### 클래스에 지정할 수 있는 접근 제한자
- public : 다른 모든 클래스로부터 참조 가능
- default(지정 없음) : 동일 패키지 내의 클래스로부터 참조 가능(패키지 프라이빗)

### 필드 및 메서드에 지정할 수 있는 수식자
- public : 모든 클래스로부터 참조 가능 
- protected : 자식 클래스 및 동일 패키지내의 클래스로부터 참조 가능
- default(지정 없음) : 동일 패키지 내의 클래스로부터만 참조 가능(패키지 프라이빗)
- private : 자기 자신의 클래스 안에서만 액세스 가능(자식 클래스로부터 참조 불가능)

{% raw %} <img src="https://chohongjae.github.io/assets/img/20201229livestudyweek7/접근제한자.png" alt=""> {% endraw %}
- [이미지 출처](https://studymake.tistory.com/424)


{% raw %} <img src="https://chohongjae.github.io/assets/img/20201229livestudyweek7/PublicClassTest.png" alt=""> {% endraw %}
- public으로 선언된 클래스와 메소드들
- 클래스내에서 모든 메소드의 접근이 가능하다.

{% raw %} <img src="https://chohongjae.github.io/assets/img/20201229livestudyweek7/DefaultClassTest.png" alt=""> {% endraw %}
- default로 선언된 클래스와 메소드들
- 물론 클래스내에서 모든 메소드의 접근이 가능하다.

{% raw %} <img src="https://chohongjae.github.io/assets/img/20201229livestudyweek7/AccessModifierSamePackage.png" alt=""> {% endraw %}
- public으로 선언된 클래스와 default으로 선언된 클래스와 같은 패키지이기 때문에 private 메소드를 제외하고 모두 접근이 가능하다.

{% raw %} <img src="https://chohongjae.github.io/assets/img/20201229livestudyweek7/AccessModifierOuterPackage.png" alt=""> {% endraw %}
- AccessModifierTest 클래스는 위에서 선언된 다른 클래스들과 패키지가 다르기 때문에 public 메소드만 접근이 가능하다.
- AccessModifierTest2 클래스는 패키지가 달라도 위에서 선언된 public 클래스의 자식 클래스이기 때문에 public, protected 메소드 접근만 가능하다.
- 위에서 default로 선언된 클래스는 패키지가 다르기 때문에 클래스를 import 할 수 없기 때문에 메소드나 필드에 접근할 수 없다.

***새로운 패키지의 클래스가 패키지 프라이빗인 경우 그 클래스가 갖고 있는 메소드나 필드가 public 이라고 해도<br>
부모 클래스를 import 할 수 없기 때문에 해당 클래스의 메소드나 필드를 사용할 수 없다.***
    