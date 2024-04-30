---
title: 자바에서의 다중 상속
date: 2024-03-09 22:33:00 +09:00
categories: [Java]
---

## **다중 상속**
다중 상속이란 2개 이상의 클래스로부터 동시에 상속받는 것을 말한다. 자바에서는 다중 상속을 허용하지 않는다. C++에서는 다중 상속을 허용했었던 기억이 있는데, 왜 자바에서는 다중 상속을 허용하지 않을까?

---

## **자바는 왜 다중 상속을 허용하지 않을까?**
> One reason why the Java programming language does not permit you to extend more than one class is to avoid the issues of multiple inheritance of state, which is the ability to inherit fields from multiple classes. For example, suppose that you are able to define a new class that extends multiple classes. When you create an object by instantiating that class, that object will inherit fields from all of the class's superclasses. What if methods or constructors from different superclasses instantiate the same field? Which method or constructor will take precedence? Because interfaces do not contain fields, you do not have to worry about problems that result from multiple inheritance of state.
>

공식문서에서 자바는 다중 상속으로 인한 문제를 피하기 위해서 허용하지 않는다고 한다. 예시로는 다중 상속을 했을 때 생길 수 있는 모호함에 대해 설명해 놓았다.

---

## **다이아몬드 문제**

![](/assets/img/java/multiple inheritance/diamond problem.png)

다중 상속으로 인한 모호성을 설명해 주는 대표적인 예시로는 다이아몬드 문제가 있다.
위와 같은 관계의 클래스가 있을 때, D 타입 객체에서 method()를 호출한다고 생각해 보자. B와  C 모두 A에서 상속받은 method()를 오버라이딩 하고 있기 때문에, D에서 method()를 호출하면 컴파일러는 B와 C 중 어떤 클래스의 method()를 호출했는지 알 수 없다. 이것이 다이아몬드 문제이다.

---

## **디폴트 메소드 규칙**
자바에서 다이아몬드 문제와 비슷하게 모호한 경우들을 몇 개 생각해 봤는데, 그 코드들의 공통점이 있었다. 인터페이스의 디폴트 메소드가 사용된다는 것이다. Modern Java in action에는 충돌(conflicts)을 컴파일러가 어떻게 해결하는지 나와 있다.

### **세 가지 규칙**
> 
1. Classes always win. A method declaration in the class or a superclass takes priority over any default method declaration.
2. Otherwise, subinterfaces win: the method with the same signature in the most
specific default-providing interface is selected. (If B extends A, B is more specific
than A.)
3. Finally, if the choice is still ambiguous, the class inheriting from multiple interfaces has to explicitly select which default method implementation to use by overriding it and calling the desired method explicitly.
>

>
1. 클래스의 메소드는 인터페이스의 메소드보다 실행 시 우선순위가 높다.
2. 자식 인터페이스는 부모 인터페이스보다 우선순위가 높다.
3. 여전히 모호한 경우, 직접 사용할 메소드를 구현하거나 선택해야 한다.
>

요약하면 이 정도로 생각할 수 있다. 세 가지 규칙에 대해 자세히 알아보자.

---

#### **1. 클래스 vs 인터페이스**

![](/assets/img/java/multiple inheritance/1.drawio.png)

```java
public class Main {
	public static void main(String[] args) throws IOException {
		C c = new C();
		c.method();
	}
}


interface A {
	public default void method() {
		System.out.println("A");
	}
}


class B {
	public void method() {
		System.out.println("B");
	}
}


class C extends B implements A {
	
}
```
클래스의 메소드는 인터페이스의 디폴트 메소드보다 우선순위가 더 높다. 따라서 c.method()를 실행하면 B의 method()가 실행되어 B가 출력된다.

---

#### **2-1. 부모 인터페이스 vs 자식 인터페이스**

![](/assets/img/java/multiple inheritance/2-1.drawio.png)

```java
public class Main {
	public static void main(String[] args) throws IOException {
		C c = new C();
		c.method();
	}
}


interface A {
	public default void method() {
		System.out.println("A");
	}
}


interface B extends A {
	public default void method() {
		System.out.println("B");
	}
}


class C implements A, B {
	
}
```
자식 인터페이스의 디폴트 메소드는 부모 인터페이스의 디폴트 메소드보다 우선순위가 더 높다. 따라서 c.method()를 실행하면 B의 method()가 실행되어 B가 출력된다.

---

#### **2-2. 부모 인터페이스 vs 자식 인터페이스**

![](/assets/img/java/multiple inheritance/2-2.drawio.png)

```java
public class Main {
	public static void main(String[] args) throws IOException {
		C c = new C();
		c.method();
	}
}


interface A {
	public default void method() {
		System.out.println("A");
	}
}


interface B extends A {
	public default void method() {
		System.out.println("B");
	}
}


class D implements A {
	
}


class C extends D implements A, B {
	
}
```

D는 A를 상속받았고, B는 A로부터 상속받은 method()를 오버라이딩 했다. 이때 c.method()를 실행하면 B의 method()와 D의 method() 중 어떤 게 실행될까? 정답은 B의 method()가 실행된다. 
1번 규칙에 따르면 클래스의 메소드는 인터페이스의 메소드보다 우선순위가 높은데 왜 D가 아닌 B의 method()가 실행될까?
D가 A를 상속받긴 했어도, 클래스 내부에서 따로 오버라이딩을 하지 않았기 때문에 D의 method()는 "D의 method()"가 아니라 "A의 method()" 취급을 받는다. 이때 2번 규칙에 따라서 B가 A보다 우선순위가 높기 때문에 B의 method()가 실행된다.

---

#### **3. 여전히 모호할 경우**

![](/assets/img/java/multiple inheritance/3.drawio.png)

```java
public class Main {
	public static void main(String[] args) throws IOException {
		C c = new C();
		c.method();
	}
}


interface A {
	public default void method() {
		System.out.println("A");
	}
}


interface B {
	public default void method() {
		System.out.println("B");
	}
}


class C implements A, B {
	
}
```

C가 인터페이스 A, B를 동시에 상속받는 경우다. 부모 클래스, 부모 인터페이스, 자식 인터페이스가 없기 때문에 기존의 1, 2번 규칙으로는 해결할 수 없다. 실행 시 **"Duplicate default methods named method with the parameters () and () are inherited from the types B and A"** 라는 내용의 오류가 발생한다. 이 경우 해당 메소드를 오버라이딩 해야 한다.

#### **메소드 직접 오버라이딩 하기**
```java
public class Main {
	public static void main(String[] args) throws IOException {
		C c = new C();
		c.method();
	}
}


interface A {
	public default void method() {
		System.out.println("A");
	}
}


interface B {
	public default void method() {
		System.out.println("B");
	}
}


class C implements A, B {
	@Override // 1
	public void method() {
		System.out.println("C");
	}
    
    @Override // 2
    public void method() {
		B.super.method();
	}
    // 자바 8부터 지원하는 문법으로 interface.super.method() 형식이다.
    // method()는 호출하고자 하는 메소드를, interface는 method()가 포함된 인터페이스를 의미한다.
}
```