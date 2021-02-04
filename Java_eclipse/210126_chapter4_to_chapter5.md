![](C:%5CUsers%5CUser%5CDesktop%5CTIL%5Cmd-img%5C%EC%A0%9C%EB%AA%A9%EC%9D%84%20%EC%9E%85%EB%A0%A5%ED%95%B4%EC%A3%BC%EC%84%B8%EC%9A%94._001.png)

# 💡 자바의 제어문
자바의 제어문은 조건문과 반복문으로, 2종류이다.

## 1. 조건문
### 1-1. if문

- if문은 조건식의 결과에 따라 블록 실행 여부가 결정된다.
- 조건식에는 true/false 값을 산출하는 연산식 또는 boolean 변수가 올 수 있다.
- 조건식이 true일 때 블록을 실행하고, false이면 블록을 실행하지 않는다.

#### 📌 if문의 형식
```java
if ( 조건식 ) {
	실행문;
	실행문;
	.
	.
	.
}
```

### 1-2. if-else문
- if문의 조건식이 true이면 if 블록을 실행하고, false이면 else 블록이 실행되는 조건문

#### 📌 if-else문의 형식
```java
if ( 조건식 ) {
	실행문;
} else {
	실행문;
}
```

### 1-3. if-else if-else문
- 처음 if문의 조건식이 false일 경우 다른 조건식을 더 추가할 때 쓰는 조건문
- else if는 제한 없이 추가할 수 있으며, 마지막에는 else를 쓴다.

#### 📌 if-else if-else문의 형식
```java
if ( 조건식 ) {
	실행문;
} else if ( 조건식 ) {
	실행문;
} else if ( 조건식 ) {
	실행문;
} else {
	실행문;
}
```

> if-else if-else문을 이용하면 **_랜덤으로 주사위 번호 하나를 뽑거나 로또 번호를 뽑는(!)_** 조건식을 만들어볼 수 있다.
```java
public class IfDice {
	public static void main(String[] args) {
		int num = (int)(Math.random()*6) + 1;  // 0 <= Math.random() < 1
		if(num==1) {
			System.out.println("1번이 나왔습니다");
		} else if(num==2) {
			System.out.println("2번이 나왔습니다");
		} else if(num==3) {
			System.out.println("3번이 나왔습니다");
		} else if(num==4) {
			System.out.println("4번이 나왔습니다");
		} else if(num==5) {
			System.out.println("5번이 나왔습니다");
		} else {
			System.out.println("6번이 나왔습니다");
		}
	}
}
```
정수 숫자 랜덤으로 1개 뽑기 메소드
👉 **(int) Math.random()*숫자 + 1**

### 1-4. 중첩 if문
- if문 내부에는 if문을 또 사용한 것을 중첩 if문이라고 한다.

#### 📌 중첩 if문의 형식
```java
if ( 조건식 ) {
	if ( 조건식 ) {
    	실행문;
    } else {
    	실행문;
    }
} else {
	실행문;
}
```

### 1-5. switch문
- switch문에는 조건식을 사용하지 않는다.
- 그 대신 변수의 값에 따른 실행문을 선택해서 실행문이 결정되므로 같은 식을 if문으로 작성한 것보다 간결하게 코드를 쓸 수 있다.
- switch()에서 괄호 안에는 정수 타입과 string타입의 변수만 들어갈 수 있다.
- ❗ 각 case마다 실행문 뒤에는 break;가 반드시 필요하다!
- 만약 case가 없으면 마지막의 default를 실행하는데 default는 생략 가능하다.

#### 📌 switch문의 형식
```java
switch(변수) {
case 값1: 실행문1; break;
case 값2: 실행문2; break;
case 값3: 실행문3; break;
default: 실행문4; break;  // default는 없어도 상관없음
}
```

## 2. 반복문
### 2-1. for문
- 지정한 횟수만큼 실행문을 반복해서 실행하는 반복문으로, 반복시킬 횟수를 알고 있을 때 사용하는 것이 일반적이다.
- 초기화식에서 루프 카운트 변수를 선언할 떄에는 부동소수점 타입을 사용하면 안 된다. (ex. float는 사용X)

#### 📌 for문의 형식
```java
for( 초기화식; 조건식; 증감식) {
	실행문;
}
```

> **for문도 중첩**하여 사용할 수 있다!
```java
public class ForNestedExample {
	public static void main(String[] args) {
		for (int i=1; i<=5; i++) {
			for (int j=1; j<=5; j++) {
				System.out.println("i="+i+", j="+j+": nested for loops");
			}
		}
	}
}
```

### 2-2. while문
- while문은 조건식이 true인 한 계속해서 반복하는 반복문
- 조건식이 false가 되면 반복을 멈춘다.
- 조건식에는 true/false 값을 내는 연산식이나 boolean 변수가 올 수 있다.
- 무한 루프를 도는 while문을 빠져나가기 위해서 break문을 사용할 수도 있다.

#### 📌 while문의 형식
```java
while(조건식) {
	실행문;
}
```

> **while문도 중첩**하여 사용할 수 있다!
```java
public class WhileNestedExample {
	public static void main(String[] args) {
		int i = 1;
		while (i<=5) {
			int j = 1;
			while (j<=5) {
				System.out.println("i="+i+", j="+j+": nested while");
				j++;
			}
			i++;
		}
	}
}
```

### 2-3. do-while문
- 실행문부터 먼저 실행하고, 실행문의 결과에 따라 루프 여부를 결정할 때 쓰는 반복문

#### 📌 do-while문의 형식
```java
do {
	실행문;
} while (조건식);
```

> #### do-while문을 이용한 예제
```java
import java.util.Scanner;
public class DoWhileExample {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int sum = 0;
		int n;
		do {
			String input = sc.nextLine();
			n = Integer.parseInt(input);
			sum += n;
		} while (n != 0);
		System.out.println(sum);
	}
}
```
>
>키보드 입력값의 키 코드를 받아주는 메소드
>👉 System.in.read()
>
>import 등을 자동으로 처리해 주는 단축키
>👉 shift + ctrl + o

### 2-4. break문
- switch문이나 반복문을 종료할 때 쓰인다.

> #### 반복문과 break문을 이용한 예제
```java
public class BreakExample {
	public static void main(String[] args) {
		int sum = 0;
		int i;
		for (i=1;;i++) {  // while (true) = for (;;)
			sum += i;
			if (sum>=100) break;
		}
		System.out.println("Sum: " + sum + ", i: " + i);
	}
}
```

- 반복문이 중첩되어 있는 경우, 가장 가까운 반복문을 종료하며 바깥쪽 반복문은 종료하지 않는다.
- 바깥쪽 반복문까지 종료시키려면 바깥쪽 반복문에 이름(라벨)을 붙이고 break 이름;을 사용한다.

> #### 중첩된 반복문과 break문을 이용한 예제
```java
public class BreakExample2 {
	public static void main(String[] args) {		
		Outter: for (int i=0; i<10; i++) {
			for (int j=0; j<10; j++) {
				System.out.println("i: " + i + ", j: " + j);
				if (j==3) break Outter;
			}
		}
	}
}
```

### 2-5. continue문
- continue문은 반복문(for, while, do-while문)에서만 사용된다!
- continue문은 break문과 반대로 반복문의 조건식으로 이동하여 계속 반복되게 만든다.

> #### for문과 continue문을 이용한 예제
```java
public class ContinueExample {
	public static void main(String[] args) {
		// 1 - 100 중 3의 배수가 아닌 수의 합
		int sum = 0;
		for (int i=1; i<=100; i++) {
			if (i%3==0) continue;  // 증감식으로 바로 이동, 3의 배수 건너뛰기
			sum += i;
		}
		System.out.println(sum);
	}
}
```

# 💡 참조 타입
자바의 데이터 타입은 기본 타입(primitive type)과 참조 타입(reference type)으로 나누어진다. 참조 타입 중에서는 대표적으로 string 타입이 많이 쓰이는 타입 중 하나이다.
> 그래서 참조 타입이란?
> 👉 객체의 번지를 참조하는 타입(= C언어의 pointer)으로 **배열, 열거, 클래스, 인터페이스 타입!**

## 1. JVM의 메모리 사용 영역
### 메소드 영역 (Method Area)
- .class 같은 실행할 명령어들이 있는 영역
- runtime 상수풀, 필드, 데이터, 메소드 데이터, 메소드 코드, 생성자 코드 등을 분류해서 저장
- 메소드 영역은 JVM이 시작할 때 생성되고 모든 스레드가 공유하는 영역

### 힙 영역 (Heap Area)
- 객체와 배열이 생성되는 영역
- 객체와 배열은 JVM 스택 영역의 변수나 다른 객체의 필드에서 참조
- 사용되지 않는 객체는 Garbage Collector가 자동 제거하기 때문에 개발자가 객체 제거를 위한 코드를 작성할 필요가 없음(~~그런데 어차피 자바는 코드로 객체를 직접 제거할 수 없다~~)

### JVM 스택 영역 (JVM Stack Area)
- 각 스레드마다 하나씩 있으며, 스레드가 시작될 때 할당된다.
- 추가로 스레드를 생성하지 않았다면 main 스레드만 존재하므로 JVM 스택도 하나이다.
- JVM 스택은 메소드를 호출할 때마다 프레임을 추가하고 메소드가 종료되면 해당 프레임을 제거한다.
- 기본 타입 변수는 스택 영역에 직접 값을 갖고 있으나, 참조 타입 변수는 힙 영역이나 메소드 영역의 객체 주소를 가진다.

## 2. 참조 변수의 ==, != 연산
### 📌 기본 타입과 참조 타입의 ==, != 연산 차이점
- 기본 타입: 변수의 값이 같은지 다른지 비교
- **참조 타입: 동일 객체를 참조하는지 아닌지 조사하므로 연산값은 true/false로 나타난다!**

## 3. null과 NullPointerException
### null
👉 참조 타입 변수가 힙 영역의 객체를 참조하지 않는다는 뜻
- null값도 초기값으로 사용 가능하므로 null로 초기화된 참조 변수는 스택 영역에 생성된다.
- 참조 타입 변수가 null값을 갖는지 여부를 알아보려면 ==, != 연산을 하면 된다!

### NullPointerException
- Exception(예외): 사용자의 잘못된 조작이나 잘못된 코딩으로 발생하는 프로그램 오류
- NullPointerException은 참조 타입 변수가 null값을 갖고 있을 때 해당 참조 타입 변수를 사용하려고 하면(= 객체를 사용하려고 하면) 발생한다.

> #### NullPointerException이 발생하는 예시
```java
public class NullExample {
	public static void main(String[] args) {
		String str = null;
		System.out.println(str.toString());
	}
}
```
