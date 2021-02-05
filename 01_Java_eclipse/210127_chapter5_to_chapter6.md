![](../md-img/%EC%A0%9C%EB%AA%A9%EC%9D%84%20%EC%9E%85%EB%A0%A5%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94._001-1612426103573.png)

# 💡 참조 타입 (이어서)
## 4. String 타입
### String 타입 사용 방법
👉 자바는 문자열을 String 변수에 저장하므로, String 변수를 먼저 선언하고 큰따옴표로 감싼 문자열 리터럴을 대입한다.
> #### String 타입 사용 예시
```java
// 예시1					// 예시2
String name;				String hobby = "programming";
name = "java";
```

### String 타입 특징
**📌 String 변수는 스택 영역, 문자열은 String 객체로서 힙 영역에 생성된다!**
- 문자열 리터럴이 동일하면 String 객체도 같다.
- new 연산자를 사용하면 문자열이 같더라도 객체는 각각 생성된다.
- String 변수도 참조 타입이므로 초기값으로 null을 대입할 수 있으나, JVM에서 자동으로 쓰레기 객체로 취급하여 제거해버린다.

## 5. 배열 타입
### 5-1. 배열
> #### 배열이란?
> 👉 같은 타입의 많은 데이터를 연속된 공간에 저장하는 자료구조로 인덱스를 사용한다!

- 배열은 객체로서 힙 영역에 생성되고, 배열 변수는 배열 객체를 참조한다.
- 배열 타입도 참조 타입에 속하므로 null로 초기화할 수 있다.
- **장점:** 변수 선언을 여러 번 할 필요가 없으며 반복문을 사용해서 많은 양의 데이터를 쉽게 처리할 수 있다.
- **단점:** 같은 타입의 데이터만 저장할 수 있고, 배열의 길이를 생성할 때 이미 결정하기 때문에 나중에 변경할 수 없다.

#### 배열 생성 방법
- 배열 변수를 선언한 다음에 값 목록이나 new 연산자로 배열을 생성할 수 있다.
- 값 목록은 배열에 넣을 값이 정해져 있을 때에 사용할 수 있고, new 연산자는 값은 정해져 있지 않지만 배열의 길이가 정해져 있을 때 사용할 수 있다.

> #### 배열 생성 예시
```java
// 값 목록으로 배열을 생성하는 예시1
int[] arry1 = {30, 10, 20};
>
// 값 목록으로 배열을 생성하는 예시2
int[] arry2;
arry2 = new int[] {30, 10, 20}
>
// new 연산자로 배열을 생성하는 예시1
int[] arry3 = new int[10];
>
// new 연산자로 배열을 생성하는 예시2
int[] arry4;
arry4 = new int[10];
>
// new 연산자로 배열을 생성하는 예시3
int[] arry5 = null;
arry5 = new int[20];
```

**📌 값 목록으로 배열 생성 시 주의할 점**
메소드의 매개값이 배열일 때와 배열 변수를 먼저 선언한 다음에 배열 변수에 값 목록으로 배열을 생성할 때에는 new 연산자를 반드시 써야 한다!
```java
int[] arry;
arry = {30, 10, 20};  // 컴파일 에러!!
```

### 5-2. 배열의 활용
#### 배열의 길이와 for문
- 배열의 길이: 배열에 저장할 수 있는 전체 항목 수
- 배열의 길이는 for문의 조건식에서 주로 활용하며, 이는 정말 많이 쓰인다.

> 배열의 길이를 구하는 방법?
> 👉 배열변수.length

#### 커맨드 라인 입력
- 자바 실행 시, JVM은 길이가 0인 String 배열을 먼저 생성해서 main() 메소드를 호출할 때 매개값으로 전달한다.
- 커맨드 라인 입력은 자바의 main() 메소드의 매개값인 String[] args를 활용한다.

> #### 커맨드 라인 입력 활용 예시
1. main(String[] args)로 사칙연산을 할 수 있는 코드 짜기
```java
public class MainArgsExample {
	public static void main(String[] args) {
		int val1 = Integer.parseInt(args[0]);
		int val2 = Integer.parseInt(args[2]);
		String op = args[1];
		switch(op) {
		case "+": System.out.println(args[0] + args[1] + args[2] + "=" + (val1+val2)); break;
		case "-": System.out.println(args[0] + args[1] + args[2] + "=" + (val1-val2)); break;
		case "x": System.out.println(args[0] + args[1] + args[2] + "=" + (val1*val2)); break;
		case "/": System.out.println(args[0] + args[1] + args[2] + "=" + (val1/val2)); break;
		}
	}
}
```
2. Run Configuration 창 열기
![](C:%5CUsers%5CUser%5CDesktop%5CTIL%5Cmd-img%5Cimage-1612425853811.png)
3. Arguments탭의 Program arguments에 연산식 입력하고 Run 버튼 누르기
![](C:%5CUsers%5CUser%5CDesktop%5CTIL%5Cmd-img%5Cimage%20(1)-1612425871235.png)
4. 콘솔창으로 결과 확인하기
![](C:%5CUsers%5CUser%5CDesktop%5CTIL%5Cmd-img%5Cimage%20(2).png)

### 5-3. 다차원 배열
- 자바는 1차원 배열을 이용하여 수학의 행렬과 같은 방식으로 2차원 배열을 구현할 수 있다.
- 가로 인덱스가 행, 세로 인덱스가 열이 된다.

> #### 다차원 배열 생성 예시
```java
int[][] array = new int[2][3];
```

- 다차원 배열은 각 배열의 길이를 구할 때 주의해야 한다!

> #### 다차원 배열의 길이 예시
```java
int[][] array2 = new int[2][3];
array2.length = 2  // array2 = new int[][] {array2[0], array2[1]}임을 기억하자.
array2[0].length = 3  // array2[0] = new int[3];과 같다!
array2[1].length = 3  // array2[1] = new int[3];과 같다!
```

### 5-4. 객체를 참조하는 배열과 배열 복사
#### 객체를 참조하는 배열
각 항목에 직접 값을 갖고 있는 기본 타입과 달리, 참조 타입(클래스와 인터페이스) 배열은 각 항목에 값을 참조할 번지를 갖고 있다.
예) String 배열
> #### 객체를 참조하는 배열의 예시
```java
public class RefArrayExample {
	public static void main(String[] args) {
		String[] strarry = new String[3];
		strarry[0] = "Hello";
		strarry[1] = "Java";
		strarry[2] = "!";		
	}
}
```

#### 배열 복사
배열은 한 번 생성하면 길이를 변경할 수 없어서 기존 배열보다 더 많은 데이터를 배열에 저장하려면 새 배열을 만들어서 복사할 수밖에 없다.

**배열 복사의 방법**
- for문 사용
- System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length); 메소드 이용

**배열 복사의 종류**
- 얕은 복사: 배열1의 참조값을 배열2에 복사하는 것
- 깊은 복사: 배열1의 참조값과 객체 모두를 새로 배열2에 복사하여 생성하는 것

> #### 배열 복사의 예시
```java
public class CopyArrayExample {
	public static void main(String[] args) {
		int[] arry = new int[] {10, 20, 30};
		int[] copy = new int[3];
		int[] rpy = arry;  // 참조 변수 복사(얕은 복사)
		copy = arry;  // copy가 arry의 참조값을 갖게 되는 참조 변수 복사(얕은 복사)
		System.arraycopy(arry,0,copy,0,arry.length);  // 메모리 분리 복사(깊은 복사)
		arry[0] = 40;
		System.out.println("원본 데이터");
		for (int i=0; i<arry.length; i++) {
			System.out.println(arry[i]);
		}
		System.out.println("\n깊은 복사");
		for (int i=0; i<copy.length; i++) {
			System.out.println(copy[i]);
		}
		System.out.println("\n얕은 복사");
		for (int i=0; i<rpy.length; i++) {
			System.out.println(rpy[i]);
		}
	}
}
```

### 5-5. 배열을 이용한 향상된 for문
- 향상된 for문을 이용하면 속도가 빠르다
- Python에서 많이 쓰인다(많이 써봤따)
#### 📌 향상된 for문의 형식
```java
for(타입 변수 : 배열) {
	실행문;
}
```

> **향상된 for문을 이용한 예제**
```java
public class AdvancedForExample {
	public static void main(String[] args) {
		int[] arr = {10, 20, 30, 40, 50};
		int sum = 0;
		for (int data : arr) {
			sum += data;
		}
		System.out.println(sum);
	}
}
```

## 6. 열거 타입
> 열거 타입이란?
> 👉 한정된 값만 갖는 데이터 타입으로, 여기에서 한정된 값이란 열거 상수를 말한다. (ex. 요일이름)

- 열거 타입 변수는 참조 타입이므로 null값 저장 가능
- 모든 열거 타입은 컴파일 시 java.lang.Enum 클래스를 자동으로 상속한다.
- 열거 객체의 메소드: 열거 상수의 문자열을 내부 데이터로 갖고 있다.

#### 열거 타입을 생성하는 방법

1. 열거 타입 선언: **파일 이름과 열거타입이름이 동일**해야 하며 관례상 **첫 글자는 대문자**이다.
  ```java
  public enum 열거타입이름 {...}
  ```
2. 열거 상수 정의: 열거 상수 이름은 관례상 **모두 대문자**로 적고, 다른 단어가 결합되는 경우에는 **언더바(_)로 단어 사이를 연결**한다.

> #### 열거 타입을 이용한 예제
```java
import java.util.Calendar;
public class EnumWeekExample {
	public static void main(String[] args) {
		Week today = null;		
		Calendar cal = Calendar.getInstance();
		int week = cal.get(Calendar.DAY_OF_WEEK);		
		switch(week) {
		case 1: today = Week.SUNDAY; break;
		case 2: today = Week.MONDAY; break;
		case 3: today = Week.TUESDAY; break;
		case 4: today = Week.WEDNESDAY; break;
		case 5: today = Week.THURSDAY; break;
		case 6: today = Week.FRIDAY; break;
		case 7: today = Week.SATURDAY; break;
		}		
		System.out.println("오늘 요일: " + today);
	}
}
```

---

# 💡 클래스

## 1. 객체 지향 프로그래밍 (Object Oriented Programming)

> 👉 부품에 해당하는 객체들을 먼저 만들어서 이들을 조립하여 하나의 프로그램을 완성시키는 것

### 1-1. 객체

물리적으로 존재하는 것과 추상적으로 생각할 수 있는 것 중에서 **속성과 동작을 가지는 모든 것**이 객체!

#### 객체의 특징

* 필드(속성)와 메소드(동작)로 구성된 자바 객체로 모델링이 가능

* 객체끼리 서로의 기능(동작)을 이용하고 데이터를 주고 받음 👉 **객체의 상호 작용**

* 객체는 상호 작용 수단으로 메소드를 호출한다.

  ```java
  // 메소드 호출 형태
  리턴값 = 객체명.메소드명(매개값1, 매개값2, ...);
  ```

#### 객체간의 관계

- 집합 관계: 완성품 - 부품
- 사용 관계: 객체 - 객체, 한 객체가 다른 객체의 메소드를 호출하여 사용
- 상속 관계: 종류 객체(상위 객체) - 구체적인 사물 객체(하위 객체)

#### 📌 객체 지향 프로그래밍의 특징

- 캡슐화

  객체의 필드와 메소드를 하나로 묶어서 구현 내용을 감추는 것. 외부 객체는 객체 내부의 구조를 알 수 없으며 해당 객체의 필드와 메소드만 이용 가능하다. 캡슐화하여 보호하는 이유는 외부의 잘못된 사용으로 객체가 손상되는 것을 방지하기 위함! 캡슐화를 위해 자바는 **접근 제한자(Access Modifier)**를 사용한다.

- 상속

  상위 객체의 필드와 메소드를 하위 객체에 물려주는 것. 하위 객체는 상위 객체를 확장해서 추가적인 필드와 메소드를 가질 수 있다.

  장점: 설계 시간 단축, 중복 코드 감소, 유지 보수 시간 최소화!

- 다형성

  같은 타입이지만 실행 결과가 다양한 객체를 대입할 수 있는 성질. 자바는 인터페이스나 부모 클래스의 타입 변환이 가능하다.

## 2. 객체와 클래스

- 자바에서는 클래스가 설계도이다.
- 클래스로부터 만들어진 객체를 해당 클래스의 인스턴스라고 한다.
- 클래스에서 객체를 만드는 과정을 인스턴스화라고 한다.
- 객체의 구성요소: 속성과 기능
- 속성은 변수, 기능은 메소드로 정의