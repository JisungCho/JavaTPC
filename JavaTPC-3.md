# 2020-03-12

### 애매하다! class, object, instance 상호관계

![image](https://user-images.githubusercontent.com/52770718/76500865-aa122d00-6484-11ea-89ca-6e1593cfad01.png)

1. 클래스 - 설계도

   - 클래스 안에는 상태정보를 작성

   - 클래스는 잘 설계 해야한다.!

     - 잘 설계 ? 

       객체를 만든다음에 기억공간에 데이터를 넣고 뺄때  얼마나 쉽고 안전하게 할 수 있느냐

2. 객체 - 구체적인 구조, 덩어리가 담기지 않은 변수

3. 인스턴스 : 실체데이터 객체가 담겨져 있는 변수

   - 메모리에 객체가 생성되고 b1에 번지가 들어감 b1이 구체적인 뭔가(실체)를 가리키면서 b1은 인스턴스가 됨
   - 따라서 인스턴스(변수)라고 부르게됨
   - 따라서 인스턴스가 만들어져야지 데이터를 넣고 뺄수 있음

```java
package part_1;

import kr.tpc.BookDTO;

public class TPC13_2 {
	public static void main(String[] args) {
		// 책 -> class (BookDTO) -> 객체 -> 인스턴스
		String title = "스프링";
		int price =  25000;
		String company = "제이펍";
		int page = 890;
		// 책이라고 볼 수 없다, 왜냐하면 연속적인 기억공간 하나에 담아야하는데 그렇지않기 때문이다.
		// 묶으려면? 
		//1. 배열? 불가능 이질적인 데이터가 존재 
		//2.이 구조를 직접 설계 -> 클래스
		
		BookDTO dto; // dto(Object : 객체)
		dto = new BookDTO(title, price, company, page); // dto(Instance : 인스턴스)
		// dto는 이제 인스턴스 변수가 됨
		// 이제 다른 메서드나 다른 클래스로 이동시킬 수 있다.

		bookPrint(dto);
	}
	
	public static void bookPrint(BookDTO dto) { //한개의 구조로 된 BookDTO를 보냄
		System.out.print(dto.title+"\t");
		System.out.print(dto.price+"\t");
		System.out.print(dto.company+"\t");
		System.out.print(dto.page+"\t");
		
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

	public BookDTO(String title, int price, String company, int page) {
		this.title = title;
		this.price = price;
		this.company = company;
		this.page = page;
	}
	
	
	
}

```

### 잘 설계된 클래스(Model : DTO, DAO, Utility)

![image](https://user-images.githubusercontent.com/52770718/76502359-3aea0800-6487-11ea-8036-ffbf69a33cbc.png)



1. 정보은닉

   - 다른 객체로부터 접근을 막는 것

   - 외부에서 인스턴스 메모리 공간의 데이터를 마음대로 넣고 빼는 것을 막는다. 

   - private로 설정

   - 그럼 private로 설정되어 있는 데이터는 어떻게 넣고 뺄수 있는가?

     -getter, setter 메소드를 이용!

```java
package kr.tpc;

public class MemberVO {
	//public으로 설정시 잘못된 데이터가 들어올 수 있다.
	//private로 설정시 인스턴스 변수로 접근해서 값을 집어넣거나 뺄수 없다. -> getter,setter 사용
	private String name;
	private int age;
	private String tel;
	private String addr;

	public MemberVO() {//디폴트 생성자는 사용하든 사용하지않든 들어주는게 좋다.
	
	}
	public MemberVO(String name, int age, String tel, String addr) {
		this.name = name;
		this.age = age;
		this.tel = tel;
		this.addr = addr;
	}
	
	//setter, getter method
	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		// if문을 걸어서 조건을 설정해 줄 수 있음
		this.age = age;
	}

	public String getTel() {
		return tel;
	}

	public void setTel(String tel) {
		this.tel = tel;
	}

	public String getAddr() {
		return addr;
	}

	public void setAddr(String addr) {
		this.addr = addr;
	}

	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
	@Override
	public String toString() { //모든 값을 한꺼번에 꺼내기 위해 작성해 주는게 좋다.
		return "MemberVO [name=" + name + ", age=" + age + ", tel=" + tel + ", addr=" + addr + "]";
	}
	
}

```

```java
package part1;

import kr.tpc.MemberVO;

public class TPC14 {
	public static void main(String[] args) {
		MemberVO m = new MemberVO();
		/*
		//정상적인 경우
		m.name = "홍길동";
		m.age = 50;
		m.tel = "010-1111-1111";
		m.addr = "서울";
	
		//비정상적인경우
		m.age = 1000;
		
		System.out.print(m.name+"\t");
		System.out.print(m.age+"\t");
		System.out.print(m.tel+"\t");
		System.out.println(m.addr);
		*/
		
		m.setName("홍길동");
		m.setAge(50);
		m.setTel("010-1111-2222");
		m.setAddr("서울");
		
		System.out.print(m.getName()+"\t");
		System.out.print(m.getAge()+"\t");
		System.out.print(m.getTel()+"\t");
		System.out.println(m.getAddr());
	}
}

```

![image](https://user-images.githubusercontent.com/52770718/76525300-f2464500-64ae-11ea-8b3b-7402c7cce95a.png)

```java
package part1;

import kr.tpc.MemberVO;

public class TPC15 {
	public static void main(String[] args) {
		MemberVO memberVO = new MemberVO("홍길동", 40, "010-1111-1111", "서울");
		// setter method 필요 없음.
		
		System.out.println(memberVO);// toString이 정의가 안되어있으면 하나씩 호출해서 출력해봐여함
		
		MemberVO m1 = new MemberVO();
		m1.setName("나길동");
		m1.setAge(31);
		m1.setTel("010-2222-3333");
		m1.setAddr("충청");
		// System.out.println(m1.toString());
		System.out.println(m1); // m1을 출력하세요? -> m1이 가지고 있는 전체의 값을 출력해라 toString이 생략
		
		
	}
	
	
}

```

### 메서드의 오버로딩(Method Overloading)

![image](https://user-images.githubusercontent.com/52770718/76525461-36394a00-64af-11ea-9458-1177b9a7b4bb.png)

```java
package kr.tpc;

public class Overload {
	// DTO,VO가 아니니까 동작(Method)으로만 이루어진 객체
	
	public void hap(int a,int b) {
		System.out.println(a+b);
	}
	public void hap(float a,int b) {
		System.out.println(a+b);
	}
	public void hap(float a,float b) {
		System.out.println(a+b);
	}
}

```

```java
package part1;

import kr.tpc.Overload;

public class TPC16 {
	public static void main(String[] args) {
		// 1+1= ? ,  23.4 + 56 =? , 56.7+78.9=?
		Overload ov = new Overload();
		ov.hap(20, 50); //hap_int_int(20,50) 이라는 메소드가 내부적으로 실행
		ov.hap(23.4f, 56); //hap_float_int(23.4f,56)
		ov.hap(56.7f, 78.9f); //hap_float_float(56.7f, 78.9f)
		
		//내부적으로 컴파일시점에서 메소드의 이름이 정해져있으므로 프로그램 속도가 느려질 이유가 없다.
	}
}

```

### 동일한 구조 vs 이질적인 구조

![image](https://user-images.githubusercontent.com/52770718/76525643-94662d00-64af-11ea-8f72-c0a9eb246978.png)

```java
package kr.tpc;

public class MovieVO {
	private String title;
	private int price;
	private String author;
	private int level;
	private float time;
	
	public MovieVO() {

	}

	public MovieVO(String title, int price, String author, int level, float time) {
		super();
		this.title = title;
		this.price = price;
		this.author = author;
		this.level = level;
		this.time = time;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}

	public int getLevel() {
		return level;
	}

	public void setLevel(int level) {
		this.level = level;
	}

	public float getTime() {
		return time;
	}

	public void setTime(float time) {
		this.time = time;
	}

	@Override
	public String toString() {
		return "MovieVO [title=" + title + ", price=" + price + ", author=" + author + ", level=" + level + ", time="
				+ time + "]";
	}
	
	
}

```

```java
package part1;

import kr.tpc.MovieVO;

public class TPC17 {
	public static void main(String[] args) {
		int[] a = new int[5]; 
		a[0] = 10;
		a[1] = 20;
		a[2] = 30;
		a[3] = 40;
		a[4] = 50;
		
		int[] b = {1,2,3,4,5};
		
		int[] c = new int[] {10,20,30,40,50};
		
		for(int i=0 ;i<b.length;i++) {
			System.out.println(b[i]);
		}
		
		//영화(제목 , 가격, 주인공, 등급, 시간)
		MovieVO mv = new MovieVO("비트", 12000, "연기자", 12, 1.3f);
		System.out.println(mv);
		System.out.println("-----------------------------");
		
		// 영화 3편을 저장 => 객체 배열
		MovieVO[] marr = new MovieVO[3];
		marr[0] = new MovieVO("비트1", 12000, "연기자1", 12, 1.3f);
		marr[1] = new MovieVO("비트2", 13000, "연기자2", 13, 1.5f);
		marr[2] = new MovieVO("비트3", 11000, "연기자3", 14, 1.7f);
		
		System.out.println(marr[0]);
		System.out.println(marr[1]);
		System.out.println(marr[2]);
		System.out.println("-----------------------------");

		for(int i=0;i<marr.length;i++) {
			System.out.println(marr[i]);
		}
	}
}

```

