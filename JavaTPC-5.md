# JavaTPC

## 추상클래스(일부 다형성 보장)

```JAVA
package part1;

import kr.poly.Animal;
import kr.poly.Cat;
import kr.poly.Dog;

public class TPC25 { //추상클래스(실습)
	public static void main(String[] args) {
		// Animal의 eat()를 Dog가 재정의를 했다는 전제가 있어야한다.
		
		//Animal ani2 = new Animal(); 추상클래스 객체생성 안됨
		//ani2.eat();
		
		// 추상클래스는 upcatsing으로 사용, 부모의 역활을 충실하게 이행하면된다.
		Animal ani = new Dog();
		ani.eat();
		ani.move();
		
		ani = new Cat();
		ani.eat();
		((Cat)ani).night(); //downcasting
		ani.move();
	}
}
/*
개처럼먹는다.
무리를 지어서 이동한다.
고양이처럼 먹는다.
밤에 눈에서 빛이난다.
무리를 지어서 이동한다.
*/
```

```JAVA
package kr.poly;

public abstract class Animal { // 추상클래스(불완전한객체,장애객체), 객체생성 불가능 -> Animal ani = new Animal(); (X)
	//추상클래스는 서로 기능이 비슷한 클래스들을 묶을때 사용한다.
	public abstract void eat(); //추상메서드 -> 불완전한 메서드 , 장애메서드 , 반드시 자식에서 구현해야함
	
	public void move() { //구현메서드가 들어갈 수 있다., 재정의를 해도되고 안해도된다.
		System.out.println("무리를 지어서 이동한다.");
	}
}

```

```JAVA
package kr.poly;

public class Cat extends Animal { // 구현클래스
	
	public void night() { 
		System.out.println("밤에 눈에서 빛이난다.");
	}

	@Override
	public void eat() { // 무조건 재정의 해야함
		System.out.println("고양이처럼 먹는다.");
	}
	
}

```

```java
package kr.poly; 

public class Dog extends Animal { // 재정의를 안해면 error가 발생 , 반드시 구현해야한다.
	// 이름, 나이 , 종  : 상태정보
	// 부모가 가지고 있는 eat()를 재정의(Override)
	@Override
	public void eat() {
		System.out.println("개처럼먹는다.");
	}
	public Dog() {
		super(); // new Animal();
	}

}

```

## 21.인터페이스(100% 다형성 보장)

```java
package part1;

import kr.poly.Radio;
import kr.poly.RemoCon;
import kr.poly.TV;

public class TPC26 { //인터페이스 실습
	public static void main(String[] args) {
		//RemoCon r = new RemoCon(); 객체구현 불가능
		
		RemoCon r = new TV();
		r.chUp();
		r.chDown();
		r.Internet();
		
		r = new Radio();
		r.chUp();
		r.chDown();
		r.Internet();
	}
}
/*
TV의 채널이 올라간다.
TV의 채널이 내려간다.
인터넷이 된다.
Radio 채널이 올라간다.
Radio 채널이 내려간다.
Radio는 인터넷이 지원되지 않는다.
*/
```

```java
package kr.poly;

public interface RemoCon { // 구현된 메서드는 가질수없다 ,장애객체, 객체생성x -> RemoCon re = new RemoCon() (x)
	// static final 상수를 사용할 수 있다.
	// 인터페이스는 객체를 생성할 수없기 때문에 static
	public static final int MAXCH = 100;
	int MINCH = 1;
	
	// 추상메서드
	public abstract void chUp(); //abstract가 생략되어 있음 
	public void chDown();
	public void Internet();
}

```

```java
package kr.poly;

public class TV implements RemoCon {//구현한다!
	int currCh = 70;
	@Override
	public void chUp() {
		if(currCh < RemoCon.MAXCH) {
			currCh++;
			System.out.println("TV의 채널이 올라간다. : "+currCh);
		}else {
			currCh = 1;
			System.out.println("TV의 채널이 올라간다. : "+currCh);
		}
		
	}

	@Override
	public void chDown() {
		if(currCh>RemoCon.MINCH) {
			currCh--;
			System.out.println("TV의 채널이 내려간다. : "+currCh);
		}else {
			currCh = 100;
			System.out.println("TV의 채널이 내려간다. : "+currCh);
		}
	}

	@Override
	public void Internet() {
		System.out.println("인터넷이 된다.");
	} 
	
	//TV의 독자적인 기능도 구현할 수 있다.
}

```

```java
package kr.poly;

public class Radio implements RemoCon{

	@Override
	public void chUp() {
		System.out.println("Radio 채널이 올라간다.");
	}

	@Override
	public void chDown() {
		System.out.println("Radio 채널이 내려간다.");
	}

	@Override
	public void Internet() {
		System.out.println("Radio는 인터넷이 지원되지 않는다.");
	}

	// 추가적으로 Radio의 기능을 넣을 수 있다.
}

```

## 22.부모가 있어서 너무 좋아!

```java
package part1;

import kr.poly.RemoCon;
import kr.poly.TV;

public class TPC27 { // 22.부모가 있어서 너무좋아
	public static void main(String[] args) {
		//RemoCon으로 TV클래스를 구동하시오
		
		RemoCon r = new TV();
		for(int i = 0;i<40;i++) {
			r.chUp();
		}
		for(int i = 0;i<40;i++) {
			r.chDown();
		}
		r.Internet();
	}
}
/*
TV의 채널이 올라간다. : 71
TV의 채널이 올라간다. : 72

.
.
.
.
TV의 채널이 올라간다. : 97
TV의 채널이 올라간다. : 98
TV의 채널이 올라간다. : 99
TV의 채널이 올라간다. : 100
TV의 채널이 올라간다. : 1
TV의 채널이 올라간다. : 2
.
.
TV의 채널이 올라간다. : 8
TV의 채널이 올라간다. : 9
TV의 채널이 올라간다. : 10
TV의 채널이 내려간다. : 9
TV의 채널이 내려간다. : 8
.
.
TV의 채널이 내려간다. : 3
TV의 채널이 내려간다. : 2
TV의 채널이 내려간다. : 1
TV의 채널이 내려간다. : 100
TV의 채널이 내려간다. : 99
TV의 채널이 내려간다. : 98
.
.
TV의 채널이 내려간다. : 72
TV의 채널이 내려간다. : 71
TV의 채널이 내려간다. : 70
인터넷이 된다.

*/
```

