# 2020-03-09

### 이것만 알자! 프로그래밍의 3대요소(변수,자료형,할당)

```JAVA
package part_1;

public class TPC02 {
	public static void main(String[] args) {
		// 프로그래밍의 3대요소 :  변수 , 자료형 , 할당
		// 자료형 : 변수가 만들어 지기 위해서는 변수의 크기, 데이터의 종류를 결정짓기 위해서 사용
		// 정수 자료형 , 실수 , 문자 , 논리 자료형 등이 있다.
		int a,b,c;
		a = 1;
		b = 1;
		c = a+b; // 2
		System.out.println(c);
		
		float f;
		f=34.5f;
		System.out.println(f);
		
		char d;
		d = 'A';
		System.out.println(d);
		
		boolean g;
		g=true;
		System.out.println(g);
		
		// 자바에서 제공하지 않는 자료형을 쓰고싶으면 ex) 책, 영화 , 회원 등등
		// 직접 만들어서 써야한다 =>  사용자정의자료형을 만들어야한다.
		
		// Book bk; 
	}
}

```

### 관계를 이해하라 (V, D , A)  +  실습

```JAVA
package part_1;

import kr.bit.Book;
import kr.bit.PersonVO;

public class TPC03 {
	public static void main(String[] args) {
		// 관계를 이해하라(V,D,A - 변수,자료형,할당). PDT vs UDDT 을 비교
		// 정수 1개를 저장하기위한 변수를 선언하시오.
		int a; // 메모리공간에 a라는 변수의 메모리 공간이 생성
		a = 10;
		
		//******클래스는 사용자정의데이터타입을 만들기 위한 도구이다.*******
		// 책 1권을 저장하기위한 변수를 선언하시오.
		//? b; // 변수앞에는 자료형이 와야함.. 책이라는 자료형은 없기 때문에 우리가 만들어야한다.
		// 책이라는 자료형, 책은 제목, 가격, 출판사, 저자, 페이지 등등 여러 구성요소가 있음, 
		//기억공간을 붙여서 구성요소를 하나의 형태로 만듬 => 구조를 설계 = 클래스
		// 책을 b에 넣는다는 것은 b는 변수이기때문에 데이터를 하나만 저장할 수 있다.
		// 책은 여러가지 구성요소로 이루어진 덩어리기 때문에 b에는 책의 번지를 가리킨다.
		Book b; //책을 메모리에 실제로 만들어야함 -> 객체 생성(실체-Instance)
		b = new Book(); //  b에는 책 객체의 번지가 들어감 - 객체(인스턴스)변수
		b.title = "자바";
		b.price = 15000;
		b.company = "한빛미디어";
		b.page = 700;
		
		System.out.println(b.title);
		System.out.println(b.price);
		System.out.println(b.company);
		System.out.println(b.page);

		PersonVO p; 
		p = new PersonVO(); //객체생성
		p.name = "박매일";
		p.age = 40;
		p.weight = 68.4f;
		p.height = 175.7f;
		
		System.out.print(p.name+"\t");
		System.out.print(p.age+"\t");
		System.out.print(p.weight+"\t");
		System.out.print(p.height+"\t");
		
	}
}

```

```java
package kr.bit;

//책(객체=구조=덩어리(VO-Value Object)=기억공간 여러개를 하나의 구조로) -> 제목 , 가격 , 출판사 ,페이지수 ........(상태정보) + (행위정보: 동작 = 메서드)

public class Book { // 설계도를 작성 ----> 설계된 객체를 생성해야한다.(객체생성) , 사용자 정의 자료형이 되었다.
	public String title;
	public int price;
	public String company;
	public int page; 
}

```

```java
package kr.bit;

//회원 -> 이름, 나이, 몸무게, 키...
public class PersonVO {
	public String name;
	public int age;
	public float weight;
	public float height;
	// 배열은 아니다 -  동일한 데이터만 넣어야되는데 그런건 아니니까 
	// 새로운 구조다 - class로 설계해서 사용
}

```

### 데이터를 이동하라.(변수  vs 배열의 관계) + 실습

```java
package part_1;

public class TPC04 {
	public static void main(String[] args) {
		// 4. 데이터를 이동하라(변수 vs 배열)
		int a,b,c; //변수의 개수가 증가하면 메소드를 수정해야하고 불편함, 데이터 이동이 어렵다.
		a=10;
		b=20;
		c=30;
		//  a+b+c = ? 특정 메서드에서 처리 -? hap()
		hap(a, b, c);
		
		int[] arr; // int여러개를 담을 수 있는 기억공간을 만듬
		arr = new int[3]; // arr을 번지를 가리킴
		arr[0] = a;
		arr[1] = b;
		arr[2] = c;
		
		hap1(arr);
	}
	
	public static void hap(int a, int b, int c) {
		int sum = a+b+c;
		System.out.println(sum);
	}

	public static void hap1(int[] x) { //배열을 받아서 덧셈을 진행 , 매개변수 부분이 간결
		//int sum = x[0]+x[1]+x[2];
		//System.out.println(sum);
		
		//  반복문을 활용 - for, while
		int sum = 0;
		for(int i=0;i<x.length;i++) {
			sum += x[i];
		}
		System.out.println(sum);
	}
}

```

```java
package part_1;

public class TPC05 {
	public static void main(String[] args) {
		// 배열의 목적 : 여러개의 같은 데이터를 하나의 구조로 만들어서 이동하기 쉽게 하기위해 필요
		int[] a = new int[3]; 
		
		a[0] = 10;
		a[1] = 20;
		a[2] = 30;
		int sum = 0;
		for(int i=0;i<a.length;i++) {
			sum += a[i];
		}
		System.out.println(sum);

		
		//9개의 정수형 변수를 2차원 구조로 만들어라(3x3)
		int[][] b = new int[3][3]; // 3x3행렬
		
		b[0][0] = 1;
		b[0][1] = 2;
		b[0][2] = 3;
		
		b[1][0] = 1;
		b[1][1] = 2;
		b[1][2] = 3;
		
		b[2][0] = 1;
		b[2][1] = 2;
		b[2][2] = 3;
		
		for(int i=0;i<b.length;i++) { //행에 접근
			for(int j=0;j<b[i].length;j++) { //행에 접근
				System.out.print(b[i][j]+"\t");
			}
			System.out.println();
		}
		
		//가변길이 배열
		//직각 삼각형 배열 만들기
		int[][] star = new int[5][];
		star[0] = new int[1];
		star[1] = new int[2];
		star[2] = new int[3];
		star[3] = new int[4];
		star[4] = new int[5];
		
		for(int i=0;i<star.length;i++) { //행에 접근
			for(int j=0;j<star[i].length;j++) { //행에 접근
				star[i][j] = '*';
				System.out.print((char)star[i][j]);
			}
			System.out.println();
		}
	}
}

```

### 메서드는 변수다

```java
package part_1;

public class TCP06 {
	//메서드 :  기능, 동작 ->  데이터를 만들어 낸다( 동작을 한 후에 데이터를 한 개만 만들어낸다!)
	//메서드에서 리턴 하는 값을 *****메서드 이름에 저장한다.****(메서드 이름이 변수 역활을 한다.)
    //메서드가 호출이 되려면 : 1. 실인수와 가인수의 개수가 같아야한다. 2. 실인수와 가인수의 데이터타입이 같아야한다.
	public static void main(String[] args) {
		// 5. 메서드 -> 동작, 기능
		// 정수 2개를 매개변수로 받아서 총합을 구하여 리턴하는 메서드를 정의하시오
		int a = 67;
		int b = 98;
		// a+b = ? 
		int result = sum(a, b);
		System.out.println(result); //165
		
		int[] arr = makeArr(); //10,20,30의 형태로 넘어옴
		int hap = 0 ;
		for(int i=0;i<arr.length;i++) {
			hap += arr[i];
		}
		System.out.println(hap);
	}
	
	public static int sum(int a, int b) { //static메서드에서 다른 메서드를 호출할 때 같은 static 메서드를 호출할 수 있다고 생각해라
		int v = a+b;
		return v;
	}
	
	// 10, 20, 30을 리턴하고 싶다.
	public static int[] makeArr() {
		int x = 10;
		int y = 20;
		int z = 30;
		int[] arr = new int[3];
		
		arr[0] = x;
		arr[1] = y;
		arr[2] = z;
		
		return arr; // 하나의 형태로 만들어서 리턴
	}
}

```

```java
package part_1;

public class TPC07 {
	public static void main(String[] args) {
		
		int a = 20;
		float b= 56.7f;
		// a+b = ?
		float v = sum(a, b); // call by value
		System.out.println(v); //76.7
		
		int[] arr = {1,2,3,4,5};
		//배열의 총합이 얼마인지 구하여라
		int vv = arrSum(arr); // call by reference
		System.out.println(vv); //15
		
	}
	public static float sum(int a,float b) {
		float v= a+b;
		return v;
	}
	public static int arrSum(int[] x) {
		int hap = 0;
		for(int i=0;i<x.length;i++) {
			hap += x[i];
		}
		return hap;
	}
}

```

