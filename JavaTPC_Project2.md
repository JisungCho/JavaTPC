# JavaTPC_Project2

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;

import org.json.JSONArray;
import org.json.JSONObject;
import org.json.JSONTokener;

import com.google.gson.JsonArray;



public class Project01_D {
	public static void main(String[] args) {
		String apiURL = "https://naveropenapi.apigw.ntruss.com/map-geocode/v2/geocode?query="; // API URL , 홈페이지에서 긁어옴
		String client_id="ptwnh29tsk"; // 클라이언트 아이디, 사용자마다 다름
		String client_secret="sW6HLJP77LOeFGEKze7Jq8vA656pNn9kOxyylo9P";// 클라이언트 비밀번호, 사용자마다 다름
		
		// BufferedReader는 한줄을 통째로 입력받는다, 입력스트림을 받는 생성자가 없다.
		// 따라서 InsutStreamReader를 통해서 입력스트림을 받는다.
		// System.in는 키보드로 입력받는다.
		BufferedReader io = new BufferedReader(new InputStreamReader(System.in)); 
		
		try {
			System.out.print("주소를 입력하세요 : ");
			String address = io.readLine(); //입력한 한줄을 읽는다.
			String addr = URLEncoder.encode(address, "UTF-8"); // address를 UTF-8형태로 인코딩, 입력공백도 문자처리 해줘야한다.
			String reqUrl = apiURL+addr; // https://naveropenapi.apigw.ntruss.com/map-geocode/v2/geocode?query=입력주소
			
			URL url = new URL(reqUrl); //잘못된 url이면 에러발생
			//url과 연결
			HttpURLConnection con = (HttpURLConnection) url.openConnection();
			//부가적으로 넘겨줄 정보
			con.setRequestMethod("GET");
			con.setRequestProperty("X-NCP-APIGW-API-KEY-ID", client_id);
			con.setRequestProperty("X-NCP-APIGW-API-KEY", client_secret);
			
			BufferedReader br;
			int responseCode = con.getResponseCode(); //정상적인 연결이면 200이 반환
			if(responseCode == 200) { //정상연결
				br = new BufferedReader(new InputStreamReader(con.getInputStream(),"UTF-8")); //연결된 쪽에서 "UTF-8"형식으로 읽어온다.
			}else { //비정상연결
				br = new BufferedReader(new InputStreamReader(con.getErrorStream())); //에러메세지를 읽어온다.
			}
			
			String inputLine;
			StringBuffer response = new StringBuffer();
			//읽어온것이 빈게 아니라면 response에 추가함
			while((inputLine = br.readLine())!=null) {
				response.append(inputLine);
			}
			br.close(); //다 추가되면 저장
			
			//response에 json이 모두 저장된 상태
			
			//response를 tokener객체로 만듬
			JSONTokener tokener = new JSONTokener(response.toString());
			//결국 reponse(json) -> JSONObject로 생성
			JSONObject object = new JSONObject(tokener);
			System.out.println(object.toString(2)); //들여쓰기
			
			//
			JSONArray arr = object.getJSONArray("addresses");
			for(int i=0;i<arr.length();i++) {
				JSONObject temp = (JSONObject) arr.get(i);
				System.out.println("address : "+temp.get("roadAddress"));
				System.out.println("jibunAddress : "+temp.get("jibunAddress"));
				System.out.println("위도 : "+temp.get("x"));
				System.out.println("경도 : "+temp.get("y"));
			}
			
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
/*
주소를 입력하세요 : 서울시 관악구 관악로1
{
  "addresses": [{
    "distance": 0,
    "roadAddress": "서울특별시 관악구 관악로 1 서울대학교",
    "x": "126.9522394",
    "jibunAddress": "서울특별시 관악구 신림동 산56-1 서울대학교",
    "y": "37.4640070",
    "addressElements": [서
      {
        "types": ["SIDO"],
        "code": "",
        "shortName": "서울특별시",
        "longName": "서울특별시"
      },
      {
        "types": ["SIGUGUN"],
        "code": "",
        "shortName": "관악구",
        "longName": "관악구"
      },
      {
        "types": ["DONGMYUN"],
        "code": "",
        "shortName": "신림동",
        "longName": "신림동"
      },
      {
        "types": ["RI"],
        "code": "",
        "shortName": "",
        "longName": ""
      },
      {
        "types": ["ROAD_NAME"],
        "code": "",
        "shortName": "관악로",
        "longName": "관악로"
      },
      {
        "types": ["BUILDING_NUMBER"],
        "code": "",
        "shortName": "1",
        "longName": "1"
      },
      {
        "types": ["BUILDING_NAME"],
        "code": "",
        "shortName": "서울대학교",
        "longName": "서울대학교"
      },
      {
        "types": ["LAND_NUMBER"],
        "code": "",
        "shortName": "산56-1",
        "longName": "산56-1"
      },
      {
        "types": ["POSTAL_CODE"],
        "code": "",
        "shortName": "08826",
        "longName": "08826"
      }
    ],
    "englishAddress": "1, Gwanak-ro, Gwanak-gu, Seoul, Republic of Korea"
  }],
  "meta": {
    "count": 1,
    "page": 1,
    "totalCount": 1
  },
  "errorMessage": "",
  "status": "OK"
}
address : 서울특별시 관악구 관악로 1 서울대학교
jibunAddress : 서울특별시 관악구 신림동 산56-1 서울대학교
위도 : 126.9522394
경도 : 37.4640070
*/

```

