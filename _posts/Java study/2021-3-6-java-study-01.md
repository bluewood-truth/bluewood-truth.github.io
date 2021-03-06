---
title: "Java: JVM의 개념과 구조"
description: ""
category: "Java Study"
tag: "Java"
---



## 1. JVM이란?

JVM이란 Java Virtual Machine의 줄임말로 직역하면 '자바를 실행하기 위한 가상 머신'이라 할 수 있다. 가상 기계란 소프트웨어화한 하드웨어를 넓게 지칭하는 말로, 비디오나 TV를 소프트웨어화한 미디어 플레이어가 그 예이다. JVM은 자바를 실행하기 위한 가상 컴퓨터라고 이해하면 된다.

자바로 작성된 애플리케이션은 모두 이 가상 컴퓨터에서만 실행되기 때문에, 자바 애플리케이션이 실행되기 위해서는 반드시 JVM이 필요하다.

![JVM](https://user-images.githubusercontent.com/55024033/110232356-6ac7d080-7f60-11eb-92d9-52b8c5a9d7a5.png)

일반 애플리케이션은 OS 위에서 바로 실행되기 때문에 OS에 종속적이다. 그래서 다른 OS에서 실행시키기 위해서는 애플리케이션을 그 OS에 맞게 변경해야 한다. 반면 자바 애플리케이션은 JVM하고만 상호작용을 하기 때문에 OS에 상관 없이 실행이 가능하다. (단 JVM은 OS에 종속적이기 때문에 해당 OS에서 실행 가능한 JVM이 필요하다)

<br>

## 2. JVM의 구조

![JVM_structure](https://user-images.githubusercontent.com/55024033/110232360-6c919400-7f60-11eb-8f98-78ccff974b4f.png)

### 2-1. Class Loader

자바 소스코드로 이루어진 자바 파일(.java)은 자바 컴파일러를 통해 바이트코드로 이루어진 클래스 파일(.class)로 컴파일된다. Class Loader는 런타임 시에 필요한 클래스 파일을 Runtime Data Area로 적재하는 역할을 하며, 이때 올바른 바이트코드로 작성되었는지 검사한다.

클래스 적재는 다음 두 가지 경우에 일어난다.

- 새로운 바이트코드가 실행될 때. (`MyClass mc = new MyClass()`)

- 바이트코드가 새로운 정적 참조(static reference)를 만들 때. (`System.out`)

### 2-2. Runtime Data Area

Runtime Data Area는 JVM이 프로그램을 수행하기 위해 OS로부터 할당받는 메모리 영역으로, 애플리케이션 실행 시 사용되는 데이터들이 적재되는 영역이다. 메소드 영역과 힙은 각각 하나씩만 생성되어 모든 쓰레드가 공유하고, JVM 스택, PC 레지스터, Native Method Stack은 각 쓰레드별로 생성된다.

#### ① Method Area 

런타임 상수 풀, 필드와 메서드 데이터, 메서드와 생성자의 코드, 클래스와 인스턴스 초기화와 인터페이스 초기화에 사용되는 특수 메서드 등, 각 클래스의 정보를 저장하는 공간이다. 

#### ② Heap

애플리케이션 실행 중 생성되는 모든 클래스 인스턴스 및 배열을 저장하는 공간이다. Garbage Collector에 의해 더 이상 참조되지 않는 메모리는 반환된다.

#### ③ JVM Stack

메서드가 호출되면 JVM Stack에 메서드 수행을 위한 메모리가 할당되며, 연산에 사용되는 지역변수, 연산의 중간 결과 등이 저장된다. 메서드의 작업을 마치면 할당된 메모리는 반환된다.

#### ④ PC Resister

현재 수행중인 JVM 명령(instruction)의 주소를 저장하는 공간이다.

#### ⑤ Native Method Stack

Java 이외의 언어로 작성된 메서드를 수행하기 위한 공간이다. 주로 JNI(Java Native Interface)에 의해 호출되는 C/C++ 코드를 수행할 때 사용된다.

### 2-3. Execution Engine

Execution Engine은 Class Loader에 의해 Runtime Data Area에 적재된 바이트코드를 실행하는 역할을 한다. 바이트코드를 JVM에서 실행하기 위한 방법은 다음 두 가지가 있다.

- Interpreter: 바이트코드 명령어를 하나씩 읽고 해석한다. 실행시간이 느리다.
- JIT(Just-In-Time) 컴파일러: 바이트코드를 기계어로 컴파일하여 실행한다. 컴파일에는 시간이 걸리지만 한번 컴파일한 기계어는 다시 실행할 때 해석할 필요가 없어 빠르게 수행된다.

기계어로 컴파일한 코드는 빠르게 실행이 가능하지만 컴파일에도 비용이 소모된다. 따라서 JVM은 처음에는 Interpreter로 해석하여 실행하다가 일정 기준이 넘으면 JIT 컴파일러로 컴파일한다.

### 2-4. Native Method Interface

JNI(Java Native Interface)라고도 한다. JVM 내에서 실행되는 Java 코드를 C, C++, 어셈블리와 같은 다른 언어로 작성된 애플리케이션 및 라이브러리와 상호 작동할 수 있도록 도와준다.

### 2-5. Native Method Libraries

C, C++ 어셈블리 등으로 작성된 라이브러리로 JNI을 통해 기능들을 제공한다.

---

##### 참고자료

- [\"Java의 정석" 3판](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=76083001&start=slayer)
- [The Java® Virtual Machine Specification (oracle.com)](https://docs.oracle.com/javase/specs/jvms/se11/html/index.html)
- [Java Native Interface Specification: 1 - Introduction (oracle.com)](https://docs.oracle.com/en/java/javase/11/docs/specs/jni/intro.html)
- [JVM Explained](https://javatutorial.net/jvm-explained)
- [JVM 구조와 자바 런타임 메모리 구조](https://jeong-pro.tistory.com/148)
- [JVM / 자바 프로그램의 실행흐름](https://sehun-kim.github.io/sehun/JVM/)

