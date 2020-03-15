# 2020-03-15

## 학습정리(우리가 사용하는 클래스의 종류들)

![image](https://user-images.githubusercontent.com/52770718/76622629-369c1880-6575-11ea-9819-51a0f3de5515.png)

```java
package part1;

import java.lang.*; // 디폴트 패키지(생략)

import com.google.gson.Gson;

import kr.tpc.MovieVO;
import kr.tpc.MyUtil;
// lang아래 모든 클래스를 가져옴, 
// 새로운 클래스를 발견하면 java.lang 패키지 아래에 클래스가 있다고 알려줌
// java.lang안에 자주쓰는 클래스를 넣어줌 디폴트 패키지기 때문에 import하지 않아도 쓸 수 있음

public class TPC18 {
	public static void main(String[] args) {
		// 1. 자바에서 제공해주는 class들.. API
		// 문자열(String) -> 상태정보 + 행위(동작)정보
		String str = new String("APPLE");
		System.out.println(str.toLowerCase()); // APPLE을 소문자로 바꿈
				
		// 2. 직접 만들어서 사용하는 class (DTO/VO, DAO , Utility... API)
		// Utility 클래스
		MyUtil my = new MyUtil();
		int sum = my.hap();
		System.out.println(sum);
		
		// 3. 다른 회사에서 만들어 놓은 class ... API
		// Gson -> json
		Gson gson = new Gson();
		
		MovieVO vo = new MovieVO("영화1", 12000, "연기자1", 15, 1.5f);
		gson.toJson(vo);
		String json = gson.toJson(vo);
		// {"title":"영화1","price":12000,"author":"연기자1","level":15,"time":1.5}
		System.out.println(json);
		//객체를 문자열의 포멧으로 만들어서 사용
	}
}

```

```java
package kr.tpc;

public class MyUtil {
	public int hap() {
		int sum = 0;
		for(int i=1;i<=10 ; i++) {
			sum += i;
		}
		return sum;
	}
}

```

##  수평적구조 vs 수직적구조

![image](https://user-images.githubusercontent.com/52770718/76696760-1170ec80-66d2-11ea-96ce-d97bd708cfba.png)

```java
package part1;

import kr.tpc.Animal;
import kr.tpc.Cat;
import kr.tpc.Dog;

public class TPC19 {
	public static void main(String[] args) {
		// Dog, Cat ---> Animal
		// 상속은 API를 이해하는데 큰 도움이 된다.
		// 상속은 현실세계와 같이 소유의 관계가 아니라 
		// 상속의 개념 - 자식이 부모가 가지고 있는 동작을 사용할 수 있다.
		
		
		Dog d = new Dog();
		d.eat();
		
		Cat c = new Cat();
		c.eat();
		// Dog.java(x) , Dog.class(o)
		// java파일이 없으면 Dog안에 어떤 기능이 있을지 알 수 없다.
		// 따라서 Dog의 클래스 파일로만 동작할 수 있도록 리모콘같은 것을 통해서 구동시킬 수 있게 해야한다.
		

		// Aniaml <----[Dog.class , Cat.class]
		// Animal을 통해 Dog와 Cat을 사용
		
		// Dog dd = new Dog();
		// dd.eat();
		/*
		Animal ani = new Dog();
		ani.eat();
		
		ani = new Cat();
		ani.eat();
		*/
		
	}
}

```

```java
package kr.tpc;

public class Dog extends Animal {
	// 이름, 나이 , 종  : 상태정보
	

}

```

```java
package kr.tpc;

public class Cat extends Animal {
	// 이름, 나이 , 종  : 상태정보
	
	public void night() { //고양이만의 기능
		System.out.println("밤에서 눈에서 빛이난다.");
	}
}

```

```java
package kr.tpc;

public class Animal {
	// Dog , cat --> eat() 공통 부분
	public void eat() {
		System.out.println("?"); // 포괄적, 추상적 
		
	}
}

```

## 재정의(Override)

![image](https://user-images.githubusercontent.com/52770718/76697194-66633180-66d7-11ea-8fe0-377b013c58c0.png)

```java
package part1;

import kr.tpc.Animal;
import kr.tpc.Cat;
import kr.tpc.Dog;

public class TPC20 {
	public static void main(String[] args) {		
		//Aniaml 부모클래스를 전혀 사용하지 않았음 -> 상속의 이점이 없음
		// Dog, Cat에 어떠한 동작이 있는지 알 경우 이렇게 사용
		Dog d = new Dog();
		d.eat(); // 재정의한 메소드
		
		Cat c = new Cat();
		c.eat(); // 재정의한 메소드
		c.night();
		
		//Animal ani = new Dog(); // 상속관계에서 [ 하위 = 자식  : 자동 형변환, upcasting]
		// 하위클래스의 동작방식을 몰라도 상위클래스를 통해 동작시킬 수 있다.
		Animal ani = new Dog(); //Dog의 동작방식을 모르기 때문에
		// upcasting(자동형변환)발생
		ani.eat();
		
		ani = new Cat();
		ani.eat();
		// ani.night();
		((Cat)ani).night(); //downcasting(강제형변환)
		
		

	}
}

```

```java
package kr.tpc;

public class Animal {
	// Dog , cat --> eat() 공통 부분
	public void eat() {
		System.out.println("?"); // 포괄적, 추상적 
		
	}
}

```

```java
package kr.tpc;

public class Dog extends Animal {
	// 이름, 나이 , 종  : 상태정보
	
	// 부모가 가지고 있는 eat()를 재정의(Override)
	@Override
	public void eat() {
		System.out.println("개처럼 먹는다.");
	}

}

```

```java
package kr.tpc;

public class Cat extends Animal {
	// 이름, 나이 , 종  : 상태정보
	
	public void night() { //고양이만의 기능
		System.out.println("밤에 눈에서 빛이난다.");
	}
	
	@Override
	public void eat() {
		System.out.println("고양이처럼 먹는다.");
	}
}

```

## 나보다 부모가 먼저야!

![image](https://user-images.githubusercontent.com/52770718/76697699-a88f7180-66dd-11ea-989c-c5c5b30a8ea9.png)

```java
package part1;

import kr.tpc.Animal;
import kr.tpc.Cat;
import kr.tpc.Dog;

public class TPC21 {

	public static void main(String[] args) {
		Dog d = new Dog();
		d.eat();
		
		Cat c = new Cat();
		c.eat();
		c.night();
		
		// 하위클래스에 어떤 동작이 있는지 알지 못할떄
		Animal ani = new Dog(); //upcasting
		// Dog생성자안에 super()라는 메소드가 있는데 결국 Animal 객체 생성 후 Dog객체 생성
		ani.eat(); // 부모의 eat지만 Dog에 재정의 되어있기 때문에 Dog의 eat가 실행
	
		// 다형성 : 상위클래스가 하위클래스들에게 동일한 메세지를 보내면 하위 클래스가 서로 다르게 동작하는 원리
		ani = new Cat();
		ani.eat();
		//ani.night();;
		/*
		Cat cc = (Cat)ani;
		cc.night();
		*/
		((Cat)ani).night();//downCasting
		
	
		
	}

}

```

```java
package kr.tpc;

public class Animal {
	// Dog , cat --> eat() 공통 부분
	public void eat() {
		System.out.println("?"); // 포괄적, 추상적 
		
	}
	public Animal() {
		super(); // new Object();
	}
}

```

```java
package kr.tpc;

public class Dog extends Animal {
	// 이름, 나이 , 종  : 상태정보
	
	// 부모가 가지고 있는 eat()를 재정의(Override)
	@Override
	public void eat() {
		System.out.println("개처럼 먹는다.");
	}
	
	public Dog() {
		super(); // new Animal();
	}

}

```

```java
package kr.tpc;

public class Cat extends Animal {
	// 이름, 나이 , 종  : 상태정보
	
	public void night() { //고양이만의 기능
		System.out.println("밤에 눈에서 빛이난다.");
	}
	
	@Override
	public void eat() {
		System.out.println("고양이처럼 먹는다.");
	}
}

```

## 부모 자식간 형변환이 된다!

![image](https://user-images.githubusercontent.com/52770718/76697947-821f0580-66e0-11ea-9b3c-f4a973788659.png)

```java
package part1;

import kr.tpc.Animal;
import kr.tpc.Cat;
import kr.tpc.Dog;

public class TPC22 {
	public static void main(String[] args) {
		
		// upCasting
		//Cat cat = new Cat(); //직접
		Animal ani = new Cat(); //간접
		//Object obj = new Cat(); //간접
		
		ani.eat(); // 컴파일 시점 - Animal의 eat(), 실행시점 - Cat의 eat() , upCasting!
		
		// ani.night(); 
		((Cat)ani).night(); // downCasting 
		
		ani = new Dog();
		ani.eat();
		// 다형성
		// 상위클래스가 하위클래스에게 동일한 메세지로 서로 다르게 동작시키는 원리
		
		Object o = new Dog();
		// o.eat(); 
		((Dog)o).eat();
		
	}
}
```

