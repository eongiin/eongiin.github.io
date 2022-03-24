---
layout: post
title: abstract class and interface
# subtitle: 부제목
# description: >
#   설명
date: '2022-03-23 21:20:0'
categories:
  - language
  - java
tags: [java]
related_posts:
  - 
sitemap: true
published: true
---
# [Java] 추상 클래스와 인터페이스

* toc 
{:toc}

## 추상 클래스
구체적이지 않은 클래스

- 클래스 앞에 `abstract` 키워드를 이용해서 정의한다
- 추상 메소드를 무조건 포함해야 하는 것은 아니며 생성자와 필드, 일반 메소드 포함 가능하다
- 추상 메소드가 하나라도 포함된다면 추상 클래스가 되어야 한다
- 추상 메소드는 구현 부분이 없다 → 자식 클래스에서 반드시 구현해야 한다
  - 상속받은 클래스가 추상 클래스라면 구현하지 않아도 된다
- 메소드 접근 제한자로 public, private, protected, (default) 모두 가능하지만 추상 메소드에는 private을 쓸 수 없다  
(자식이 접근해서 오버라이딩 해야하기 때문)
- 추상 클래스 자체로는 객체를 생성할 수 없지만, 추상클래스를 상속받은 클래스를 통하면 인스턴스화 가능하다
- 하위 클래스의 생성자에서 super()변수를 사용해서 추상 클래스의 생성자를 부르고 초기화 시키므로 생성자가 필요하다  
- 다중 상속 불가

## 인터페이스
서로 관계가 없는 물체들이 상호 작용을 하기 위해서 사용하는 장치나 시스템

- 상수 (public static final), abstract method, default method, static method를 포함할 수 있다
  - 변수를 선언하면 컴파일시 자동으로 public static final로 바뀐다 
- 원래는 모든 메소드가 추상 메소드여야 했지만, Java 8부터 default 메소드와 static 메소드를 포함할 수 있게 됐다
  - default 메소드와 static 메소드를 사용하려면 해당 키워드로 명시해줘야 한다 → 생략시 abstract method
  - 인터페이스의 모든 메서드의 접근 제한자는 자동으로 public이다
- 생성자를 가질 수 없다
- 클래스는 이러한 인터페이스를 여러개 구현할 수 있다
- 어떠한 A 클래스가 여러 인터페이스를 구현 했을 때, 인터페이스가 가진 메소드의 이름이 겹치는 경우가 발생할 수 있다  
  - 그 메소드가 abstract 메소드라면? 어차피 구현된 것이 없으니 A 클래스가 알아서 구현을 하면 된다 (문제 x)
  - default 메소드라면? 반드시 오버라이딩 해야 한다  
  (원래 default 메소드는 따로 오버라이딩 해주지 않아도 됨)
  - static 메소드라면? static 메소드는 어차피 오버라이딩을 하지 못한다  
  인터페이스의 static 메소드는 `인터페이스이름.메소드이름`으로 접근하기 때문에 상관없다 (문제 x)

<br>
<b>default method</b>  
- 인터페이스가 변경이 되면, 인터페이스를 구현하는 모든 클래스들이 해당 메소드를 구현해야 하는 문제가 있어 이를 해결하기 위해 인터페이스에 메소드를 구현할 수 있도록 했다
- default 메소드로 공통 기능을 작성함으로써 반복되는 코드 작성을 줄일 수 있다
- 오버라이딩 가능 (필수 아님)

<b>static method</b>
- static method를 선언함으로써, 인터페이스를 이용하여 간단한 기능을 가지는 유틸리티성 인터페이스를 만들 수 있다
- 오버라이딩 불가


## 공통점

- 추상화를 위해 사용됨  
선언부(어떤 것이 동작하는지)는 보여주고, 내부 구현부(어떻게 동작하는지)는 숨기는 형태

## 차이점

### 사용 목적

- 추상 클래스
  -  is-a : ~는 ~이다  
- 인터페이스 
  - has-a : ~는 ~를 할 수 있다

ex) Bird는 Animal 이고 날 수 있다(Flyable)  

```
class Bird extends Animal implements Flyable
```

### 다중 상속

- 추상 클래스는 다중 상속할 수 없다 (참고 -[자바는 왜 다중상속을 지원하지 않을까?](https://siyoon210.tistory.com/125){:target="_blank"})  
- 인터페이스는 여러 개 구현될 수 있다  
- 만약 각각 다른 추상 클래스로부터 상속 받은 두 클래스 A,B에게 공통된 기능을 추가하고 싶다면?  
→ 인터페이스로 작성해서 구현하면 된다

### 구성 요소

| | 추상 클래스 | 인터페이스 |
| :------: | :--------: | :----------: |
| 접근 제한자| public<br>protected<br>(default)<br>private<br>(단, 추상 메소드는 private 불가) | public|
| method | abstract<br>일반<br>static| abstract<br>default<br>static|
| 멤버 변수 | 클래스 변수<br>인스턴스 변수|only public static final|
| 생성자 | O| X|


## Reference

- [추상클래스 VS 인터페이스 왜 사용할까? 차이점, 예제로 확인](https://myjamong.tistory.com/150){:target="_blank"}  
- [추상 클래스와 인터페이스의 차이](https://velog.io/@new_wisdom/Java-%EC%B6%94%EC%83%81-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4){:target="_blank"}  
- [추상 클래스 vs 인터페이스](https://da-nyee.github.io/posts/java-abstract-class-vs-interface/){:target="_blank"}  
- [default method, static method 강의](https://programmers.co.kr/learn/courses/5/lessons/241){:target="_blank"}  
- [인터페이스](https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6){:target="_blank"}  
- [인터페이스와 추상클래스](https://medium.com/webeveloper/%EC%9E%90%EB%B0%94-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%99%80-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-6eecbe5d6350){:target="_blank"}  