# TIL_SELF_STUDY
recording what I learned for that day
# Day 09

keywords : `메소드 overload`  `접근제한자`  `상속`   `super(); `  `this();`  `static`  `싱글톤 패턴` `final`  `static final`

## 메소드 overload

* 조건 : 메소드명 동일, 매개변수의 타입 || 개수 || 순서를 바르게 선언

  * 리턴타입은 상관없다

* 오버로드하는 이유?

  * 가독성 ↑ : 메소드명만 보고 더해지는 부분을 바로 알 수 있음
  * 재사용 : 따로 메소드를 만들 필요없이 매개변수만 달리 줘서 재사용 가능

  ```java
  add( 10 , 20 );
  add( 1.1 , 2.2 );
  add( "자바" , "공부를" , "합니다");
  
  void add( int a, int b ){	//정수형 매개변수 2개
      System.out.println(a + b);
  }
  void add( double a, double b ){	//실수형 매개변수 2개
      System.out.println(a + b);
  }
  void add( String a, String b , String c){ //String 타입 매개변수 3개
      System.out.println( a + b + c );
  }
  
  ```

  

## 접근제한자

> public
>
> ​	다른 패키지에서도 사용가능, 접근 제한을 두지 않은 상태

>default
>
>​	같은 패키지에서 선언된 클래스는 사용가능하지만, 다른 패키지에서는 접근할 
>
>​	없는 상태

> protected
>
> ​	자식 (하위) 클래스가 아니라면 default과 마찬가지이지만, 
>
> ​	자식(하위) 클래스라면 다른 패키지라도 접근이 가능하다.

> private
>
> ​	선언된 클래스 내에서만 접근이 가능.



## 상속

> 상속의 특징

* 단일 상속만 가능 

  ```java
  class A extends class B, C {	//오류
  
  }
  ```

* 자식이 부모를 선정

  * 부모는 자식 선택 불가 (대신, final로 상속 못하게 막을 수 있음)
    * 자식클래스에서 개별화된 필드와 메소드가 있고
    * 부모 클래스에서는 자식 클래스들의 공통적인 필드와 메소드가 있기 때문

> 메모리 구조

```java
public class A extends B {
	public A() {	
		System.out.println("A 기본 생성자");
	}

	public static void main(String[] args) {	
		A a = new A();	//A 생성자를 호출하여 new 연산자로 객체 생성 
	}
}
```

```java
public class B {
	public B() {
		System.out.println("B 기본 생성자");
	}
}
```

>  출력 결과 

```java
B 기본 생성자
A 기본 생성자
```

* 명시적으로 생성자를 생성하지 않아도 자동으로 컴파일러가 기본생성자를 만들어준다
* 자식 클래스인 class A에서 생성자가 호출되면 A 가 부모보다 먼저 생성되나?
  * 호출은 자식 클래스를 먼저하더라도
  * 출력 결과에서 알 수 있듯이 완료는 부모 클래스인 B 생성자 호출이 먼저 끝난다.

> 🤖🤖Import vs. extends (상속) vs. new 객체 생성
>
> > 각각 언제 써야할까? 
> >
> > >  상속 : 부모와 자식 클래스의 관계 (is ~ a)
> >
> > `Sunflower(자식) is a plant(부모) ` 
> >
> > `Seoul tower(자식) is a building(부모)` 
> >
> > > import : 부분과 집합관계 ( has ~a)
> >
> > `plant(전체) has a root(부분)`
> >
> > `cake (전체) has flour(부분)`
> >
> > > new 객체 생성 : 사용/ 조립의 관계 (use)
> >
> > `Human (사용자) rides (uses) a Car(피사용자)`

## super 와 this.

* this. 와 this()
  * 자신의 인스턴스 클래스 멤버 자신을 가르킨다.
* super. 와 super()
  * 부모 (상위) 클래스의 멤버를 가르킨다.
  * 생성자: 직계 부모 클래스의 생성자는 호출할 수 있지만 조상 클래스의 생성자는 호출할 수 없다.
  * 필드와 메소드는 직계와 조상 모두 호출 가능하다.

``` java
public class A extends B {
	int a;
	
	public A() {
        super();
		System.out.println("A 기본 생성자");
	}

	void aMethod() {
    //	super();  -----------(X)
        System.out.println("A 메소드 호출");
        super.bMethod();
		super.cMethod();
    }
	public static void main(String[] args) {
		A a = new A();
		a.aMethod();
	}
```

```java
public class B extends C {
	int b;

	public B() {
		System.out.println("B 기본 생성자");
	}

	void bMethod() {
		System.out.println("B 메소드 호출");
	}
```

```java
public class C {
	int c;
	public C() {
		System.out.println("C 기본 생성자");
	}
	void cMethod() {
		System.out.println("C 메소드 호출");
	}
```

> 출력결과

```java
C 기본 생성자
B 기본 생성자
A 기본 생성자
B 메소드 호출
C 메소드 호출
```

##  static : 정적 필드 & 정적 메소드 

```java
public class A extends B {
	int a;
	
	public A() {
		super();
		System.out.println("A 기본 생성자");
	}

	void aMethod() {
		System.out.println("A 메소드 호출");
		B.bMethod();	//static이므로 클래스로 접근가능
		super.cMethod();
	}
	public static void main(String[] args) {
		A a = new A();		
		a.aMethod();
		a.cMethod();
	}
```

```java
public class B extends C {
	int b;

	public B() {
		System.out.println("B 기본 생성자");
	}

	static void bMethod() {
		System.out.println("B 메소드 호출");
	}

}
```

```java
public class C {
	static int c=100;
	
	static {
		System.out.println("static 블록 영역입니다.");
	}
	static {
		
		c=200;
	}
	public C() {
		System.out.println("C 기본 생성자");
	}
	void cMethod() {
		System.out.println("cMethod 호출");
		System.out.println(c);
	}
}

```

> 출력 결과

```java
static 블록 영역입니다.	//static{} 블록 먼저 메모리에 올라감
C 기본 생성자			
B 기본 생성자
A 기본 생성자
A 메소드 호출
B 메소드 호출
cMethod 호출
200					
cMethod 호출
200
```

* 메모리에 올라가는 순서
  * static {   } →   static 변수,  메소드→ {    } → 부모 생성자 → 자식생성자 → main() 메소드 수행
* 주의 사항 🤖
  * static은 선언하는 순간부터 메모리에 올라가서 실행할 때까지도 계속있는다
  * 즉, 불필요하게 static으로 선언해서 메모리를 낭비하는 것은 지양해야한다

## 싱글톤 형식

* 싱글톤 : 전체 프로그램에서 단 하나의 객체만 생성되는 객체

```java
public class Singleton {

	private static Singleton singleton= new Singleton();
	
	private Singleton () {}
	
	static Singleton getInstance() {
		return singleton;
	}
	
}
```

* 방식
  1. `private`으로 외부에서 객체 생성을 하지 못하도록  막아버림
  2. `private`으로 Singleton 객체에 접근할 수 없도록 막고, static으로 공용으로 클래스로 접근하도록 하기
  3. `private` 으로 접근할 수 없는 생성자와 필드를 `static getInstance`메소드로 싱글톤 객체 리턴하기

## final

* `final` 키워드가 붙으면 마지막이므로 수정이 불가능

  * 따라서 `final class A`가 되면 A는 부모 클래스로 삼을 수 없다
  * `final int a1  `를 선언한다면 
    	1. 생성과 동시에 초기값을 줘야하거나
    	1.  생성자에서 초기화를 함

  ```java
  public class A {
  	final int a1 = 10;
  	public A() {
  	//	a1 = 10;  -----------(X) 오류
  	}
  ```

  ```java
  public class A {
  	final int a1;
  	public A() {
  		a1 = 10;
  	}
  ```

## 상수 static final

* 고정 불변의 값 , 공용성을 띠고 객체마다 저장할 필요가 없는 값
  * 지구의 둘레, 원주율 등등..

​				`static final PI = 3.141592`

​				`static final EARTH_RADIUS`