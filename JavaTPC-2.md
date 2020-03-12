# 2020-03-10

## JVM의 메모리 모델

### 	1. JVM Memory Model 1

![image](https://user-images.githubusercontent.com/52770718/76278087-0ed75700-62ce-11ea-9280-dbd149d1468b.png)

```java
package part_1;

public class TPC08 {
	//staic키워드는 프로그램을 실행하기전에 미리 메서드의 기계어 코드를 자동으로 메모리에 로딩하기 위해 사용
	public static void main(String[] args) { //main에 static이 없으면 메모리에 로딩이 안되기 때문에 main함수에는 무조건 static이 있어야한다.
		int a = 30;
		int b = 50;
		int v = add(a,b); // static method call
		System.out.println(v);
	}
	
	public static int add(int a,int b) { // main은 static zone에 있는데 add도 static zone에 있어야지 호출 할 수 있다.
		int sum = a+b;
		return sum;
		
	}
}

```

### 	2. JVM Memory Model 2

![image](https://user-images.githubusercontent.com/52770718/76279711-fb7aba80-62d2-11ea-9c2c-334d1c33bdec.png)

```java
package part_1;

public class TPC09 {
	public static void main(String[] args) {
		int a = 56;
		int b = 40;
		TPC09 tpc = new TPC09(); // 객체를 heap area에 생성하고 add를 호출
		int v = tpc.sum(a,b); // add는 static이 아니니까 바로  호출할 수 없음
		System.out.println(v);
	}
	public int sum(int a,int b) {
		int v = a+b;
		return v;
	}
}

```

## 기본자료형(PDT) vs 사용자정의자료형(UDDT)

![image](https://user-images.githubusercontent.com/52770718/76281511-9f1a9980-62d8-11ea-824e-d8ea5dd4b473.png)

```java
package part_1;

import kr.tpc.BookDTO;

public class TPC10 {
	public static void main(String[] args) {
		// int , float , char , boolean -> PDT
		int a;
		a = 10;
		
		
		// 책이라는 자료형을 만들자. --> class로 만들자
		// 객체생성
		BookDTO b = new BookDTO();
		
		//데이터를 집어넣고 출력 or 다른 곳으로 이동
		// b. : b가 가리키고 있는 곳
		b.title = "자바책";
		b.price = 17000;
		b.company = "영진";
		b.page = 670;
		
		System.out.print(b.title+"\t");
		System.out.print(b.price+"\t");
		System.out.print(b.company+"\t");
		System.out.println(b.page);
	}
}

```

```java
package kr.tpc;

public class BookDTO { // 설계
	public String title;
	public int price;
	public String company;
	public int page;
	
	//디포르 생성자 매서드 생략
	public BookDTO() {
		// 우리 눈에는 보이지 않지만 객체를 생성하는 작업을 한다.(기계어 코드) 
		// 객체 생성 뒤 this라는 객체도 만들어진다.
	}
}

```

## 객체가 메모리에 어떻게 만들어지나

![image](https://user-images.githubusercontent.com/52770718/76283572-45b56900-62de-11ea-8190-9b6c79530c94.png)

```java
package part_1;

import kr.tpc.BookVO;

public class TPC11 {
	public static void main(String[] args) {
		// 책 1권을 저장하기위해 [객체를 생성]하시오
		
		BookVO b = new BookVO(); // 객체 생성
		
		b.title = "파이썬";
		b.price = 15000;
		b.company = "에이콘";
		b.page = 700;
		
		System.out.print(b.title+"\t");
		System.out.print(b.price+"\t");
		System.out.print(b.company+"\t");
		System.out.println(b.page);
		
		//또 하나의 책
		BookVO b1 = new BookVO(); // 객체 생성
		
		b1.title = "오라클";
		b1.price = 25000;
		b1.company = "이지펍";
		b1.page = 500;
		
		System.out.print(b1.title+"\t");
		System.out.print(b1.price+"\t");
		System.out.print(b1.company+"\t");
		System.out.println(b1.page);
	}
}

```

```java
package kr.tpc;
//	책(Object) -> 제목, 가격, 출판사, 페이지수 .....
public class BookVO {
	public String title;
	public int price;
	public String company;
	public int page;
	//	디폴트생성자메서드(생략)
}

```

## 생성자 메서드

![image](https://user-images.githubusercontent.com/52770718/76285710-bad76d00-62e3-11ea-9a42-1d84e96397a2.png)

```java
package part_1;

import kr.tpc.BookVO;

public class TPC12 {
	public static void main(String[] args) {
		//생성자메서드 -> 생성 + 초기화 -> 중복정의 가능
		BookVO b1 = new BookVO(); // 생성o 초기화0 
		System.out.print(b1.title+"\t");
		System.out.print(b1.price+"\t");
		System.out.print(b1.company+"\t");
		System.out.println(b1.page);
		
		BookVO b2 = new BookVO(); // 생성o 초기화0 , 원하는 값으로 할 수가 없다. 따라서 중복정의를 해서 내가 원하는 초기값으로 설정할 수 있게 한다. 
		System.out.print(b2.title+"\t");
		System.out.print(b2.price+"\t");
		System.out.print(b2.company+"\t");
		System.out.println(b2.page);
		
		BookVO b3 = new BookVO("파이썬",18000,"대림",920);
		System.out.print(b3.title+"\t");
		System.out.print(b3.price+"\t");
		System.out.print(b3.company+"\t");
		System.out.println(b3.page);
	}
}

```

```java
package kr.tpc;
//	책(Object) -> 제목, 가격, 출판사, 페이지수 .....
public class BookVO {
	public String title;
	public int price;
	public String company;
	public int page;
	//	디폴트생성자메서드(생략)
	
	public BookVO( ) {
		//초기화 작업
		title = "java";
		price = 14000;
		company = "이지스";
		page = 780;
	}
	
	// 생성자 메서드의 중복정의(overloading), 매서드 이름이 같아도 되지만, 매개변수의 개수 or 데이터 타입이 달라야한다.
	// 생성자 메서드를 오버로딩하면 기본생성자는 생성되지 않는다.
	public BookVO(String title,int price,String company,int page) {
		this.title = title;
		this.price = price;
		this.company = company;
		this.page = page;
	}
}

```

## Private 생성자도 있어요?(Static과 관계)

![image](https://user-images.githubusercontent.com/52770718/76287393-bad96c00-62e7-11ea-9bdb-7c9e768f23d1.png)

```java
package part_1;

import kr.tpc.Inflearn;

public class TPC13 {
	public static void main(String[] args) {
		// Inflearn inf = new Inflearn(); // 객체생성
		// inf.tpc();
		// inf.java(); //접근방법을 굳이 변수로 접근할 필요가 없음
		Inflearn.java();
		Inflearn.tpc2();
		
		
		// Java API 생성자 private
		// System sys = new System(); 생성자 생성 불가능
		System.out.println("출력");
		// Math m = new Math(); 생성자 생성 불가능
		int v = Math.max(10, 20);
		System.out.println(v);
	}
}

```

```java
package kr.tpc;

public class Inflearn {
	private Inflearn() {
		//객체생성을 하지 못하게 함.
	}
	//인스턴스 메서드
	public void tpc() {
		System.out.println("TPC강의 너무 재미있다.");
	}
	
	
	//클래스 메서드
	public static void java() {
		System.out.println("java강의 너무 재미있다.");
	}
	
	public static void tpc2() {
		System.out.println("TPC강의 너무 재미있다.");
	}
}

```

