# 💡 9장. 확인문제 5번

## 📌 Vehicle 인터페이스의 익명 구현 객체 이용하기

```java
public interface Vehicle {
	public void run();
}
```

```java
public class Anonymous {
	Vehicle field = new Vehicle () {
		@Override
		public void run() {
			System.out.println("자전거가 달립니다.");
		}
	};
	void method1() {
		Vehicle localVar = new Vehicle() {
			@Override
			public void run() {
				System.out.println("승용차가 달립니다.");
			}		
		};
		localVar.run();
	}
	void method2(Vehicle v) {
		v.run();
	}
}
```

```java
public class AnonymousExample {
	public static void main(String[] args) {
		Anonymous anony = new Anonymous();
		anony.field.run();  // 자전거
		anony.method1();  // 승용차
		anony.method2(new Vehicle() {  // 트럭
			@Override
			public void run() {
				System.out.println("트럭이 달립니다.");
			}		
		});
	}
}
```

# 💡 예외 처리 (이어서)

## 2. 실행 예외

### NullPointerException

```java
public class ExceptionExample {

	public static void main(String[] args) {
		String str = "dudu-dudu-du";
		int a = 10, b = 0;
		try {
			System.out.println(str.toString());
			System.out.println(a/b);
		} catch(NullPointerException e) {
			System.out.println(e.getMessage());
		} catch(ArithmeticException e) {
			System.out.println(e.getMessage());
		} catch(Exception e) {
			System.out.println(e.getMessage());
		}
		System.out.println("프로그램 종료");
	}
}
```

* `null`값을 갖는 변수에 객체 접근 연산자(`.`)를 사용했을 때 발생하는 예외. 즉, 객체의 값이 없는데 사용하려 할 때 발생하는 예외이다.
* 예외가 발생하면 Console창에 어떤 예외가 어떤 소스의 몇 번째 코드에서 발생했는지 알려준다.
* 프로그램이 정상작동하지 않을 때 발생하는 오류와 맞는 걸 넣어줘야 예외처리가 제대로 된다.
* `catch`문은 위에서 아래로 순서대로 처리되므로 순서에 유의해야 한다.
* `Exception`은 모든 예외 상황에 대해 처리하는 것이기 때문에 가장 마지막에 처리하도록 해야 한다.

### ArrayIndexOutOfBoundsException

* 예시 1

```java
public class ExceptionExample2 {
	public static void main(String[] args) {
		int[] arr1 = {10, 20, 30, 40};
		int[] arr2 = {2, 0, 6};
		for (int i = 0; i < arr1.length; i++) {
			try {
				System.out.println(arr1[i]/arr2[i]);
			} catch (ArithmeticException e) {
				System.out.println(e.getMessage());
			} catch (ArrayIndexOutOfBoundsException e) {
				System.out.println(e.getMessage());
			}
		}		
	}
}
```

> **출력 결과**
>
> 5
> / by zero
> 5
> Index 3 out of bounds for length 3

* 예시 2

```java
public class ExceptionExample2 {
	public static void main(String[] args) {
		int[] arr1 = {10, 20, 30, 40};
		int[] arr2 = {2, 0, 6};		
		try {
			for (int i = 0; i < arr1.length; i++) {
			System.out.println(arr1[i]/arr2[i]);
			}
		} catch (ArithmeticException e) {
			System.out.println(e.getMessage());
		} catch (ArrayIndexOutOfBoundsException e) {
			System.out.println(e.getMessage());
		}		
	}
}
```

> **출력 결과**
>
> 5
> / by zero

* `try`의 위치에 따라 예외 처리의 결과가 달라진다.
  * 예시 1에서는 반복문이 끝날 때까지 예외를 하나씩 처리
  * 예시 2에서는 예외가 한 번 발생하면 처리 후 바로 반복문을 빠져나간다.

### NumberFormatException

```java
public class ExceptionExample3 {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int sel;
		while(true) {
			try {
				System.out.println("숫자 입력>> ");
				sel = Integer.parseInt(sc.nextLine());
				break;
			} catch (NumberFormatException e) {
				System.out.println("숫자만 입력이 가능합니다.");
			}
		}
		System.out.println("프로그램 종료");
	}
}
```

* 변수가 `int`로 선언되어 숫자 형태가 되어야 하지만 문자를 입력하면 숫자 형태로 변환이 불가능하여 `NumberFormatException`이 발생
* `NumberFormatException`처리를 할 때 `try`-`catch`를 `while`문 안에 넣어서 숫자만 입력 가능하도록 처리할 수 있다.

### ClassCastException

```java
class A {}
class B extends A {}
class C extends A {}
public class ExceptionExample4 {

	public static void main(String[] args) {
		A a = new B();
		try {
			C c = (C)a;
		} catch (ClassCastException e) {
			System.out.println("Cast failed");
		}
	}
}
```

* 인터페이스 또는 상위 클래스를 상속받은 관계가 아닌데 Casting할 때 `ClassCastException` 발생
* 타입 변환 시에는 `instanceof`연산자를 활용해서 타입 변환이 가능한지 알아보는 것이 좋다.

## 3. 예외 처리 코드

### 예외 처리 하는 법

* **예외처리코드:** 예외 발생 시 프로그램 종료를 막고 정상 실행을 유지할 수 있도록 처리하는 코드
* `try`-`catch`-`finally` 블록을 이용하여 예외 처리 코드 작성
* `try` 블록에는 예외가 발생 가능 코드 입력
* `catch`블록에는 예외 발생 시 처리 코드를 입력하며 여러 개의 `catch`블록이 올 수 있다.
* `finally`블록은 생략 가능하며 예외 발생 여부에 상관없이 항상 반드시 실행해야 하는 코드 입력

```java
// try-catch-finally를 모두 사용한 예외 처리 코드 예시
public class ExceptionExample5 {
    
	public static void main(String[] args) {
		try {
			String str = null;
			System.out.println(str.toString());
			System.out.println("try");
			return;
		} catch (Exception e) {
			System.out.println("예외 처리");
			return;
		} finally {  // 예외 발생할 때나 발생하지 않을 때나 반드시 처리. try나 catch문의 return 적용X
			System.out.println("마무리 처리");
		}
	}
}
```

## 4. 예외 떠넘기기

### throw와 throws

```java
public class ExceptionExample6 {

	public static void main(String[] args) {
		try {
			throw new Exception("내가 만든 예외");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

* `throw`를 사용하여 예외를 직접 생성해서 떠넘길 수 있다.
* 예외는 `throw`를 해야 하지만 `java`에 원래 있는 함수에는 `throw`가 포함되어 있으므로 따로 `throw`를 써주지 않아도 예외 처리가 된다.
* 메소드 내에서는 `throw`를 사용한다.
* 메소드 선언부 끝에는 `throws`로 작성해서 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘긴다.
* `throws` 뒤에는 콤마(`,`)로 구분하여 여러 개의 예외를 나열할 수 있다.

```java
public class ExceptionExample7 {
	public static void method1() throws Exception {
		method2();
	}
	public static void method2() throws Exception {
		String str = null;
		try {
			 System.out.println(str.toString());
		} catch (NullPointerException e) {
			System.out.println(e.getMessage());
			throw new Exception("다시 생성한 예외");
		}
	}
	public static void main(String[] args) {
		try {
			method1();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

## 5. 사용자 정의 예외와 예외 발생

### 사용자 정의 예외 클래스 선언

- 은행 업무 처리 프로그램에서 잔고 부족 예외 같은 것은 자바 표준 API에서 제공하지 않는다.
- 잔고 부족 예외, 계좌이체 실패 예외 등 애플리케이션 서비스와 관련된 예외를 애플리케이션 예외라고 한다.
- 애플리케이션 예외는 개발자가 직접 정의해서 만들어야 하므로 **사용자 정의 예외**라고 한다.
- 사용자 정의 예외 클래스는 일반 예외와 실행 예외 모두 선언 가능!

```java
enum BankExpCode {NOT_ACC, EXIST_ACC, LACK_BLNC, NOT_DEPOSIT, MAIN_MENU, ACC_MENU};
class AccountException extends Exception {
	// 계좌번호가 틀린 경우
	// 계좌번호 중복 생성되는 경우
	// 잔액이 부족한 경우
	// 입금액이 잘못된 경우
	// 메뉴: 0-5까지만 선택 가능
	// 서브메뉴: 1,2만 선택 가능
	BankExpCode errCode;
	public AccountException() {
		super("계좌 오류");
	}
	public AccountException(String message) {
		super(message);
	}
	public AccountException(String message, BankExpCode errCode) {
		super(message);
		this.errCode = errCode;
	}
	public AccountException(BankExpCode errCode) {
		super("계좌 오류");
		this.errCode = errCode;
	}
	@Override
	public String getMessage() {
		String msg = super.getMessage();
		switch(errCode) {
		case NOT_ACC: msg += ": 계좌번호가 틀립니다."; break;
		case EXIST_ACC: msg += ": 이미 존재하는 계좌번호입니다."; break;
		case LACK_BLNC: msg += ": 계좌의 잔액이 부족합니다."; break;
		case NOT_DEPOSIT: msg += ": 입금액이 잘못되었습니다."; break;
		case MAIN_MENU: msg += ": 0 - 5만 선택 가능합니다."; break;
		case ACC_MENU: msg += ": 1, 2만 선택 가능합니다."; break;
		}
		return msg;
	}	
}
```

### 예외 발생시키기

* 메소드를 호출한 곳에서 예외를 처리하도록 메소드 선언부에 `throw`키워드로 예외를 떠넘긴다.
* 메소드를 호출한 곳에서는 `try`-`catch`를 통해 예외 처리를 해준다.
* `catch`문에서 예외 메시지를 출력하려면 예외 메시지를 갖는 생성자를 이용해야 한다.

```java
public class AccountExceptionTest {
	public static void errNotAcc() throws AccountException {
		throw new AccountException(BankExpCode.NOT_ACC);
	}
	public static void errExistAcc() throws AccountException {
		throw new AccountException(BankExpCode.EXIST_ACC);
	}
	public static void main(String[] args) {
		try {
			errExistAcc();
		} catch (AccountException e) {
			System.out.println(e.getMessage());
		}
	}
}
```

## 6. 예외 정보 얻기

* 모든 예외 객체들은 `Exception`클래스를 상속받으므로 `Exception`이 갖고 있는 메소드를 호출할 수 있따.
* `getMessage()`
  * 예외 메시지를 얻어올 때 가장 많이 사용하는 메소드
  * 예외 메시지에는 왜 예외가 발생했는지 간단한 설명이나 예외 코드가 포함된다.

# 💡 멀티 스레드

> 웹에서보다 안드로이드 개발 시에 멀티 스레드가 특히 더 필요하다.

## 1. 멀티 스레드 개념

- 프로세스: 실행 중인 하나의 프로그램(애플리케이션)
  -  하나의 프로그램이 다중 프로세스를 만들기도 한다. (예: 2개의 크롬 브라우저 실행 시)
- 멀티 태스킹: 두 가지 이상의 작업을 동시에 처리하는 것
- 멀티 프로세스: 두 개 이상의 독립적인 프로그램들을 동시에 실행하고 여러 가지 작업 처리
- 멀티 스레드: 한 개의 프로그램을 실행하고 내부적으로 여러가지 작업 처리
- 메인 스레드: 모든 자바 프로그램은 메인 스레드가 `main()` 메소드를 실행하면서 시작!
  - 실행 종료 조건: 마지막 코드 실행 또는 `return`문을 만났을 때
  - 필요에 따라 작업 스레드를 만들어서 병렬적으로 코드를 실행할 수 있다!

## 2. 작업 스레드 생성과 실행

- run()을 오버라이드 하거나 runnable 인터페이스로 상속받아서 스레드가 실행할 코드를 작성
- '`스레드.start()`'로 작업 스레드의 `run()` 메소드 실행

### Thread 클래스에서 직접 생성한 경우(Runnble 구현 클래스 생성)

```java
import java.awt.Toolkit;

public class BeepTask implements Runnable {  // 다른 클래스의 기능을 상속받을 수 있다

	@Override
	public void run() {
		Toolkit toolkit = Toolkit.getDefaultToolkit();
		for(int i=0; i<5; i++) {
			toolkit.beep();
			try { Thread.sleep(500); } catch(Exception e) {}
		}
	}
}
```

### Thread의 하위 클래스로 생성한 경우

```java
import java.awt.Toolkit;

public class BeepThread extends Thread {  // 다른 클래스의 기능을 상속받을 수 없음(단일상속만 가능하기 때문)

	@Override
	public void run() {
		Toolkit toolkit = Toolkit.getDefaultToolkit();
		for(int i=0; i<5; i++) {
			toolkit.beep();
			try { Thread.sleep(500); } catch(Exception e) {}
		}
	}
}
```

### 스레드의 이름

* 스레드의 이름은 디버깅할 때 어떤 스레드가 어떤 작업을 하는지 조사할 목적으로 사용된다.
* 사용자가 직접 생성한 스레드의 이름은 `Thread-n`으로 설정
* 스레드의 이름을 따로 명명할 수도 있다.

```java
public class ThreadA extends Thread {
	public ThreadA() {
		setName("ThreadA");  // 스레드 이름 명명하기
	}

	@Override
	public void run() {
		for(int i=0; i<2; i++) {
			System.out.println(getName() + "가 출력한 내용");  // 스레드 이름 가져오기
		}
	}
}
```

## 3. 스레드 우선 순위

### 동시성과 병렬성

- 동시성: 멀티 작업을 위해 하나의 코어에서 멀티 스레드가 번갈아가며 실행하는 성질
- 병렬성: 멀티 작업을 위해 멀티 코어에서 개별 스레드를 동시에 실행하는 성질

### 스레드 스케줄링

> 👉 스레드의 갯수가 코어의 수보다 많을 경우, 스레드를 어떤 순서로 동시에 실행할지 결정하는 것

- 스레드 스케줄링에 의해 스레드들이 번갈아서 `run()` 메소드를 조금씩 실행한다.
- 자바의 스레드 스케줄링
  - 우선순위: 1 - 10 중 숫자가 높을 수록 우선순위가 높으며, 기본 우선순위는 5. *코드로 제어 가능!(`thread.setPriority()` 이용)*
  - 순환 할당: 시간 할당량을 정해서 하나의 스레드를 정해진 시간만큼 실행하고 다른 스레드를 실행하는 방식. JVM에서 자동으로 정해지므로 코드로 제어 불가.

## 4. 동기화 메소드와 동기화 블록

- 공유 객체를 사용할 때 주의할 점

  - 멀티 스레드가 하나의 객체를 공유해서 생기는 오류

    예) 한 사람이 계좌에서 돈을 빼내는데 동시에 다른 사람이 같은 계좌에 돈을 넣으려고 할 때

  - 한 스레드가 설정한 값을 다른 스레드가 그 사이 바꾸면 오류가 날 수도 있다.

  👉 **그래서 동기화 메소드와 동기화 블록을 사용한다!**

### 동기화 메소드 및 동기화 블록

> 단 하나의 스레드만 실행할 수 있는 메소드 또는 블록

* 다른 스레드는 이미 실행 중인 메소드나 블록이 끝날 때까지 대기한다. (동기화)
* `synchronized`: 메소드 선언부 또는 메소드 내부에서 블록 선언 시 사용하는 키워드

#### 동기화 메소드

```java
public synchronized void method() {
    // 단 하나의 스레드만 실행
}
```

#### 동기화 블록

```java
public void method() {
    // 여러 스레드 실행 가능 영역
    synchronized(this) {
        // 단 하나의 스레드만 실행
    }
    // 여러 스레드 실행 가능 영역
}
```

## 5. 스레드 상태

- 일반적인 상태
  - 스레드 객체 생성 ➡ `start()` ➡ 실행 대기 ⬅(반복)➡ 실행 ➡ 종료
- 일시정지 상태를 도입한 경우
  - 실행 대기 ➡ 실행 ➡ 일시 정지 ➡ 실행 대기 ➡ 실행 ➡ 일시 정지 (반복)