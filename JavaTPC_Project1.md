# JavaTPC_Project

```java
package kr.inflearn;

public class BookDTO { 
	private String title;
	private int price;
	private String company;
	private int page;
	public BookDTO() {
		//디폴트 생성자는 필수로 써주는게 좋다
	}
	public BookDTO(String title, int price, String company, int page) {
		super();
		this.title = title;
		this.price = price;
		this.company = company;
		this.page = page;
	}
	public String getTitle() {
		return title;
	}
	public int getPrice() {
		return price;
	}
	public String getCompany() {
		return company;
	}
	public int getPage() {
		return page;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public void setPrice(int price) {
		this.price = price;
	}
	public void setCompany(String company) {
		this.company = company;
	}
	public void setPage(int page) {
		this.page = page;
	}
	@Override
	public String toString() {
		return "BookDTO [title="+title+", price="+price+", company="+company+", page="+page;
	}
	
	
	
}

```

```java
import java.util.ArrayList;
import java.util.List;

import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import kr.inflearn.BookDTO;

public class Project01_A {
	public static void main(String[] args) {
		// Obect(BookDTO) -> Json(String)
		BookDTO dto = new BookDTO("자바", 21000, "에이콘", 670);
		//Gson api
		Gson g = new Gson();
		String json = g.toJson(dto); //dto안에 있는 멤버변수가 key가 되서 들어감 
		System.out.println(json);
		
		// Json(String) -> Object(BookDTO)
		BookDTO dto1 = g.fromJson(json, BookDTO.class);//정확하게 class까지 작성
		System.out.println(dto1);
		
		// Object(List<Boo kDTO>) -> Json(String) : [{ },{ },...]
		List<BookDTO> list = new ArrayList<BookDTO>();
		list.add(new BookDTO("자바1", 21000, "에이콘1", 570));
		list.add(new BookDTO("자바2", 31000, "에이콘2", 670));
		list.add(new BookDTO("자바3", 11000, "에이콘3", 770));
		String lstJson = g.toJson(list);
		System.out.println(lstJson);
		
		// Json(String) -> Object(List<BookDTO>)
		//g.fromJson(lstJson, List<BookDTO>.class); //타입정보가 2개니까 저렇게 쓸 수 없음
		List<BookDTO> lst1 = g.fromJson(lstJson, new TypeToken<List<BookDTO>>() {}.getType());
		for(BookDTO vo : lst1) {
			System.out.println(vo);
		}
	}
}
/*
{"title":"자바","price":21000,"company":"에이콘","page":670}
BookDTO [title=자바, price=21000, company=에이콘, page=670
[{"title":"자바1","price":21000,"company":"에이콘1","page":570},{"title":"자바2","price":31000,"company":"에이콘2","page":670},{"title":"자바3","price":11000,"company":"에이콘3","page":770}]
BookDTO [title=자바1, price=21000, company=에이콘1, page=570
BookDTO [title=자바2, price=31000, company=에이콘2, page=670
BookDTO [title=자바3, price=11000, company=에이콘3, page=770
*/

```

------

```java
import org.json.JSONArray;
import org.json.JSONObject;

public class Project01_B {
	public static void main(String[] args) {
		// JSON-Java(org.json)
		JSONArray students = new JSONArray();
		JSONObject student = new JSONObject();
		student.put("name", "홍길동");
		student.put("phone", "010-1111-1111");
		student.put("address", "서울");
		System.out.println(student);
		//{"address":"서울","phone":"010-1111-1111","name":"홍길동"}

		students.put(student);
		
		student = new JSONObject();
		student.put("name", "나길동");
		student.put("phone", "010-2222-2222");
		student.put("address", "광주");
		
		students.put(student);
		
		JSONObject object = new JSONObject();
		object.put("students", students);
		System.out.println(object.toString(2)); //들여쓰기
		/*
		{"students": [
		  {
		    "address": "서울",
		    "phone": "010-1111-1111",
		    "name": "홍길동"
		  },
		  {
		    "address": "광주",
		    "phone": "010-2222-2222",
		    "name": "나길동"
		  }
		]}
		*/
	}
}	

```

-----

```json
{"students": [
  {
    "address": "서울",
    "phone": "010-1111-1111",
    "name": "홍길동"
  },
  {
    "address": "광주",
    "phone": "010-2222-2222",
    "name": "나길동"
  }
]}
```

```java
import java.io.InputStream;

import org.json.JSONArray;
import org.json.JSONObject;
import org.json.JSONTokener;

public class Project01_C {
	public static void main(String[] args) {
		String src = "info.json"; //이클래스가 있는 경로에서 파일을 찾음
		// IO->Stream(스트림)
		InputStream is = Project01_C.class.getResourceAsStream(src);
		//이 클래스가 만들어진 곳에서 괄호안에있는 리소스와 연결해서 객체(stream)을 가져와라
		if(is == null) {
			throw new NullPointerException("Cannot find Resource file");
		}
		
		//문자열을 json객체로 변환되서 메모리에 올라감
		JSONTokener tokener = new JSONTokener(is);
		//tokener에 저장되어있는 json데이터를 JSONObject형식으로 바꾸어줌
		JSONObject object = new JSONObject(tokener);
		JSONArray students = object.getJSONArray("students");
		for(int i=0;i<students.length();i++) {
			JSONObject student = (JSONObject) students.get(i);//학생 한명
			System.out.println(student);
		}
		
		
	}
}
/*
{"address":"서울","phone":"010-1111-1111","name":"홍길동"}
{"address":"광주","phone":"010-2222-2222","name":"나길동"}
*/


```

