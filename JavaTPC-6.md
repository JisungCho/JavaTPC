# JavaTPC-6

## 26.인터페이스와 JDBC의 관계

- 벤더 별로 데이터베이스 접속방법, CRUD 동작방법이 다르다 -> 메소드가 다르다
- 그렇기 때문에 개발자가 각 데이터베이스의 동작을 알아야지 JDBC프로그래밍이 가능하다
- 인터페이스를 통해서 각 데이터베이스가 수행하는 동작의 이름을 통일시키고 (구현방법은 각자 다르지만) 
- 그 인터페이스를 구현하게한다.
- 그러면 개발자는 어떤 데이터베이스던지 인터페이스를 통해서 사용한다

```java
package kr.tpc;

public interface DbConnect { // 어떤 데이터베이스던지 DbConnect를 구현해야함
	public void getConnection(String url,String user,String password);
}

```

```java
package kr.tpc;

public class JavaMySQL implements DbConnect {

	@Override
	public void getConnection(String url, String user, String password) { //추상메소드 구현
		System.out.println("JavaMySQL DB가 접속됩니다.");
	}

}

```

```java
package kr.tpc;

public class JavaOracle implements DbConnect {
	@Override
	public void getConnection(String url, String user, String password) {
		System.out.println("Oracle DB가 접속됩니다.");
	}

}

```

```java
package kr.tpc;

public class TPC32 {
	public static void main(String[] args) {
		//  Oracle, Mysql -> 벤더에서 제공하는 Drive class필요
		DbConnect conn = new JavaOracle();
		conn.getConnection("url", "bit", "12345");
		
		conn = new JavaMySQL();
		conn.getConnection("url", "root", "a1234");
	}
}
/*
Oracle DB가 접속됩니다.
JavaMySQL DB가 접속됩니다.
*/
```

## 27.인터페이스의 상속관계

- 인터페이스끼리의 상속은 가능
- 상속받은 인터페이스를 구현할 경우 상위인터페이스와 하위인터페이스의 추상메서드를 모두 구현해야함

## 28.Object 클래스는 신이야!

- Object클래스 : 최상위 클래스
- toString() : 번지의 주소를 문자열로 출력

```java
package kr.tpc;

 import java.lang.*; //생략가능
public class A extends Object { //extedns뒤에 생략가능
	public A() { //생략가능
		super();
	}
	public void display() {
		System.out.println("나는 A이다.");
	}
	
	//재정의
	@Override
	public String toString() {
		return "재정의 메서드 입니다.";
	}
	
}

```

```java
package kr.tpc;

public class TPC33 {
	public static void main(String[] args) {
		A a = new A(); // 다형성x
		a.display(); // 자신의 display()
		System.out.println(a.toString()); // 재정의된 메소드
		System.out.println(a); // a만 써도 toString호출됨
		// A에 toString이 재정의 안되어있으면 번지값이 문자열로 나옴
		
		Object obj = new A(); // upcasting
		((A)obj).display(); // downcasting
		System.out.println(obj.toString()); // 다형성 원리에 의해 자녀의 toString호출
	}
}
/*
나는 A이다.
재정의 메서드 입니다.
재정의 메서드 입니다.
나는 A이다.
재정의 메서드 입니다.
*/
```

## 29.Object class활용

```java
package kr.tpc;

public class A2 {
	public void go() {
		System.out.println("나는 A의 go 메서드 입니다.");
	}
}

```

```java
package kr.tpc;

public class B {
	public void go() {
		System.out.println("나는 B의 go 메서드 입니다.");
	}
}

```

```java
package kr.tpc;

public class TPC35 {
	public static void main(String[] args) {
		// A2와 B 클래스를 저장할 배열
		Object[] o = new Object[2]; //다형성 배열
		o[0] = new A2();
		o[1] = new B();
		
		for(int i=0;i<o.length;i++) {
			if(o[i] instanceof A2) { // o객체를 A2타입으로 참조할 수 있으면
				((A2)o[i]).go();
			}else {
				((B)o[i]).go();
			}
		}
		printGo(o);
	}

	private static void printGo(Object[] o) {
		for(int i=0;i<o.length;i++) {
			if(o[i] instanceof A2) {
				((A2)o[i]).go();
			}else {
				((B)o[i]).go();
			}
		}
	}
	
}
/*
나는 A의 go 메서드 입니다.
나는 B의 go 메서드 입니다.
나는 A의 go 메서드 입니다.
나는 B의 go 메서드 입니다.
*/
```

