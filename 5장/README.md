# 5장 : 형식 맞추기
> 프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야 한다. 코드 형식을 맞추기 위한 간단한 규칙을 정하고 그 규칙을 착실히 따라야 한다.
> 

## 📌 형식을 맞추는 목적

### 코드 형식의 중요성

- 코드 형식은 중요하다!

    - 너무나도 중요하므로 융통성 없이 맹목적으로 따르면 안 된다.


    - 코드 형식은 의사소통의 일환이다.

### 왜 중요한가?

- 오늘 구현한 기능이 다음 버전에서 바뀔 확률은 아주 높다.


    - 오늘 구현한 코드의 가독성은 앞으로 바뀔 코드의 품질에 지대한 영향을 미친다.


- 오랜 시간이 지나 원래 코드의 흔적이 사라지더라도


    - 맨 처음 잡아놓은 구현 스타일과 가독성 수준은 유지보수 용이성과 확장성에 영향

## 📌 적절한 행 길이를 유지하라

### 소스코드 길이는 얼마나 길어야 적당할까?

(자바에서 파일 크기는 클래스 크기와 밀접)

- 대다수 자바 소스 파일은 크기가 어느 정도일까?
    
    ![](https://blog.kakaocdn.net/dn/bUT1v1/btq2tmuFutm/eVKfKKIBnGoPMwhTRgKsK0/img.png)
    
    - 상자를 관통하는 선은 각 프로젝트에서 최대 파일 길이와 최소 길이 파일


    - 상자는 대략 파일의 1/3을 차지한다. 상자 중간이 평균이다.
    - 분석
        - 평균 파일 크기는 65줄


        - 전체 파일 중 대략 1/3이 40-100줄
        - 가장 긴 파일은 약 400줄, 가장 짧은 파일은 6줄
- 결론
    - 500줄을 넘지 않고 대부분 200줄 정도인 파일로도 커다란 시스템을 구축할 수 있다.


    - 일반적으로 큰 파일보다 작은 파일이 이해하기 쉬우므로 이걸 바람직한 규칙으로 삼으면 좋겠다.

### 신문 기사처럼 작성하라 🗞️

- 신문의 특징
    - 독자는 최상단 표제를 보고서 기사를 읽을지 말지 결정


    - 첫 문단은 전체 기사 내용을 요약
    - 쭉 읽으며 내려가면 세세한 사실이 조금씩 드러남.
    - 날짜, 이름, 발언, 주장, 기타 세부사항이 나온다.
- 소스파일에 적용
    - 이름은 간단하면서 설명이 가능하게


        - 이름을 보고도 올바른 모듈을 살펴보고 있는지 아닌지를 판단할 정도로 신경 써서 짓는다.
    - 소스코드 첫 부분은 고차원 개념과 알고리즘 설명
    - 아래로 내려갈수록 의도를 세세하게 묘사
    - **마지막에는 가장 저차원 함수와 세부 내역이 나옴.**

> 한 면을 채우는 기사는 거의 없다.

### 개념은 빈 행으로 분리해라

- 생각 사이에는 빈 행을 분리해야 마땅하다.
- 개념을 빈 행으로 구분한 좋은 예
    
    ```java
    package fitnesse.wikitext.widgets;
    
    import java.util.regex.*;
    
    public class BoldWidget extends ParentWidget {
    		public static final String REGEXP = "'''.+?'''";
    		private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
    			Pattern.MULTILINE + Pattern.DOTALL
    	);
    	
    	public BoldWidget(ParentWidget parent, String text) throws Exception {
    		super(parent);
    			Matcher match = pattern.matcher(text);
    			match.find();
    			addChildWidgets(match.group(1));
    	}
    	
    	public String render() throws Exception {
    		StringBuffer html = new StringBuffer("<b>");
    		html.append(childHtml()).append("<b>");
    		return html.toString();
    	}
    }
    ```
    
    - 패키지 선언부, import 문, 각 함수 사이에 빈 행이 들어감.
- 빈 행은 새로운 개념을 시작한다는 시각적 단서

### 세로 밀집도

- 줄바꿈이 개념을 분리한다면 세로 밀집도는 연관성을 의미함.
- 서로 밀집한 코드의 행은 세로로 가까이 놓여야 함.
- 나쁜 예
    
    ```java
    public class ReporterConfig {
    	/**
    	* 리포터 리스너의 클래스 이름
    	*/
    	private String m_className;
    	
    	/**
    	* 리포터 리스너의 속성
    	*/
    	private List<Property> m_properties = new ArrayList<Property>();
    	public void addProperty(Property property) {
    		m_properties.add(property);
    	}
    }
    ```
    
    - 의미 없는 주석으로 두 인스턴스 변수를 떨어뜨려 놓음.
  
- 개선한 예
    
    ```java
    public class ReporterConfig {
    	private String m_className;
    	private List<Property> m_properties = new ArrayList<Property>();
    
    	public void addProperty(Property property) {
    		m_properties.add(property);
    	}
    }
    ```
    
    - 코드가 **한 눈에** 들어옴.


    - 변수 두 개에 메서드 하나인 클래스라는 사실이 드러남.

### 수직 거리

> 함수 연관 관계와 동작 방식을 이해하려고 이 함수에서 저 함수로 오가며 소스 파일을 위아래로 뒤지는 등 뺑뺑이를 돌았으나 결국은 미로 같은 코드 때문에 혼란만 가증된 경험이 있는가?
> 
- 서로 밀접한 개념은 세로로 가까이 둬야 한다.
- 하지만 타당한 근거가 없다면 서로 밀접한 개념은 한 파일에 속해야 마땅하다.


    - **이게 바로 protected 변수를 피해야 하는 이유**


- 같은 파일에 속할 정도로 밀접한 두 개념은 세로 거리로 연관성 표현
    - 연관성 : 한 개념을 이해하는 데 다른 개념이 중요한 정도


    - 연관성이 깊은 두 개념이 멀리 떨어져 있으면 코드를 읽는 사람이 소스 파일과 클래스를 여기저기 뒤지게 됨.

**변수선언**

- 변수는 사용하는 위치에 최대한 가까이 선언한다. (?? 자바에서만인가?
- 우리가 만든 함수는 짧음 → 지역 변수는 각 함수 맨 처음에 선언
- 예시1
    
    ```java
    private static void readPreferences() {
    	InputStream is= null;
    	try {
    		is= new FileInputStream(getPreferencesFile());
    		setPreferences(new Properties(getPreferences()));
    		getPreferences().load(is);
    	} catch (IOException e) {
    		try {
    			if(is != null)
    			is.close();
    		} catch(IOException i1) {
    		}
    	}
    }
    ```
    
- 예시2
    - 목표를 제어하는 변수는 흔히 루프문 내부에 선언
    
    ```java
    public int countTestCases() {
    	int count= 0;
    	for(Test each : tests)
    		count += each.countTestCases();
    	return count;
    }
    ```
    
- 예시3
    - 블록 상단이나 루프 직전에 변수 선언