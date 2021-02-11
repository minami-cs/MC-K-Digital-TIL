# 💡 클래스 (이어서)

## 8. 접근 제한자(Access Modifier)

* 클래스와 클래스의 구성 멤버에 대한 접근을 제한하는 역할을 한다.
* 클래스 앞, 클래스 내 변수 앞, 메소드 앞에 붙는다.

### 클래스의 접근 제한자

> **📌 클래스에 사용 가능한 접근 제한자는 `public`과 `default`뿐이다!!**

* 클래스의 접근 제한자 종류
  * `public`: 모든 패키지에서 자유롭게 접근 가능
  * `default`: 클래스 선언 시 public을 생략하면 자동으로 적용되며 **같은 패키지 내에서만 접근 가능**

```java
// default 접근 제한자 사용 예시
class 클래스이름 {...}

// public 접근 제한자 사용 예시
public class 클래스이름 {...}
```

### 생성자의 접근 제한자

* new 연산자로 객체 생성 시 접근 제한자에 따라 호출 가능 여부를 결정한다.
* 생성자의 접근 제한자 종류
  * public ⊃ protected ⊃ default ⊃ private
  * `public`: 모든 패키지에서 자유롭게 접근 가능
  * `protected`: 같은 패키지 내에서 자유롭게 접근 가능하며 다른 패키지에서 사용 시 상속만 가능
  * `default`: 생성자 선언 시 public을 생략한 경우 자동으로 적용되고 같은 패키지 내에서만 접근 가능
  * `private`: 같은 클래스 내부에서만 생성자 호출 및 객체 생성 가능

```java
public class ClassName {
    // public
    public ClassName (...) {...}
    // protected
    protected ClassName (...) {...}
    // default
    ClassName (...) {...}
    // private
    private ClassName (...) {...}
}
```

## 9. Getter와 Setter

* 외부에서 함부로 객체의 데이터 전체에 접근하지 못하게 할 때 사용하는 메소드
* 클래스 선언할 때 필드를 `private`으로 하고 해당 필드에 대해 Getter와 Setter 메소드를 만든다.

> **Getter와 Setter 메소드 생성 방법**
>
> 👉 [Source] ➡ [Generate Getters and Setters]

```java
// Getter와 Setter 사용 예제
public class Account {
	public static final int MIN_BALANCE = 0;
	public static final int MAX_BALANCE = 1000000;
	private int balance;  // balance 필드를 private으로 선언
	public int getBalance() {
		return balance;
	}
	public void setBalance(int balance) {
		if (balance>=MIN_BALANCE && balance<=MAX_BALANCE) {
			this.balance = balance;
		}
	}
}
```

```java
class Account {
	String account;
	String name;
	int balance;
	public Account() {  // 기본 생성자 값
		account = "100";
		name = "n/a";
		balance = 0;
	}
	public String info() {
		return "계좌번호: " + account + ", 이름: " + name + ", 잔액: " + balance;
	}
}

public class AccountTest {
	public static void main(String[] args) {
		// Account 객체 2개 만들기
		Account acc1 = new Account();
		Account acc2 = new Account();
		// 101, 홍길동, 100000
		acc1.account = "101";
		acc1.name = "홍길동";
		acc1.balance = 100000;
		// 102, 김길동, 200000
		acc2.account = "102";
		acc2.name = "김길동";
		acc2.balance = 200000;
		// 두 계좌 정보 출력
		System.out.println(acc1.info());
		System.out.println(acc2.info());
	}
}
```

## 10. 어노테이션(Annotation)

* 어노테이션은 컴파일 과정과 실행 과정에서 코드를 어떻게 컴파일하고 처리할지 알려주는 메타데이터
* 어노테이션의 용도
  * 컴파일러에게 작성한 코드 문법 에러를 체크하도록 정보 제공
  * 소프트웨어 개발 툴이 빌드나 배치 시 코드를 자동 생성하도록 정보 제공
  * 실행 시(런타임 시) 특정 기능을 실행하도록 정보 제공

# 💡 상속

