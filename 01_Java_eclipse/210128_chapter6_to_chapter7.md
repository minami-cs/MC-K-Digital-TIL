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

## 1. 객체 지향 프로그램에서의 상속

* 자식 클래스가 부모 클래스의 멤버를 물려받는 것
* **자식이 부모를 선택**한다!
* 상속 대상: 부모의 필드와 메소드

### 상속의 효과

- 부모 클래스를 재사용하므로 자식 클래스를 만들기 때문에 빠른 개발이 가능하다.
- 반복되는 코드 중복을 줄여준다.
- 유지 보수가 편리하다.
- 객체의 다형성 구현이 가능하다.

### 상속 대상의 제한

* 부모 클래스의 `private` 필드와 메소드는 제외

* 부모 클래스가 자식 클래스와 다른 패키지에 있는 경우 부모 클래스의 `default` 접근 필드와 메소드는 제외

  > `private`
  >
  > 👉 같은 클래스 내부에서만 생성자 호출 및 객체 생성 가능!
  >
  > `default`
  >
  > 👉 생성자 선언 시 public을 생략한 경우 자동으로 적용되고 같은 패키지 내에서만 접근 가능!

## 2. 클래스 상속 방법

* 자식 클래스명 뒤에 `extends` 키워드와 부모 클래스명을 작성한다.

```java
// 클래스 상속 작성 예시
class 자식 클래스명 extends 부모 클래스명 {
}
```

> **📌 자바에서는 단일상속만 가능하므로 `extends` 키워드 뒤에 여러 개의 부모 클래스 이름 나열 불가!!**

### `super()` 키워드

* 부모 클래스를 상속한 자식 클래스 내에서 부모 클래스의 생성자를 호출하기 위해 사용하는 키워드

```java
// 클래스 상속 예제
class Person {
	int age;
	String name;
	public Person() {}  // 오버로딩 시 기본 생성자는 반드시 만들어줘야 된다.
	public Person(int age, String name) {
		this.age = age;
		this.name = name;
	}
    public String info() {
		return "이름: " + name + ", 나이: " + age;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}

class Student extends Person {
	String major;
	public Student(int age, String name, String major) {
		super(age, name);
		this.major = major;
	}
    public String getMajor() {
		return major;
	}
	public void setMajor(String major) {
		this.major = major;
	}
    public String info() {
		return "이름: " + name + ", 나이: " + age;
	}
}

class Professor extends Person {
	String department;
	public Professor() {}
	public Professor(int age, String name, String department) {
		super(age, name);
		this.department = department;
	}
	public String getDepartment() {
		return department;
	}
	public void setDepartment(String department) {
		this.department = department;
	}
	@Override  // Source -> Override로 부모 클래스 요소 중 오버라이드 할 것 선택
	public String info() {  // 부모 메소드 재정의
		return super.info()+", 부서: " + department;
	}
}

public class InheritExample {
	public static void main(String[] args) {
		Student s = new Student(20, "정윤호", "연극영화학과");
		System.out.println(s.info());  // 부모 클래스는 자식 클래스를 모르기 때문에 major는 출력되지 않음
    }
}
```

### `@Override` 와 `Overload`

> *오버라이딩과 오버로딩은 단어가 비슷하지만 헷갈리지 말자!*

* **오버로드:** 동일한 이름의 메소드를 여러 개 생성하는 것
* **오버라이드:** 부모 클래스에서 상속받은 메소드를 자식 클래스에서 재정의하는 것
* 오버라이드 작성 규칙
  * 리턴타입, 메소드명, 매개 변수 모두 부모와 동일해야 한다.
  * 접근 제한도를 부모보다 더 강하게 오버라이드할 수 없다.
* 오버라이드 방법
  * Source ➡ Override/Implement Methods

```java
// 오버라이딩과 오버로딩을 활용한 예제
public class Base2 {
	// overload 예시
	protected void method() {}  // protected는 같은 패키지의 다른 클래스나 자식 클래스만 접근 허용
//	public int method(int n) {  // 리턴타입이 다른 것은 오버로드 성립 불가능!!
//	}
	public void method(int n) {
		
	}
	public void method(int n, int m) {
		
	}
	public void method(double d, int n) {
		
	}
	public void method(int n, double d) {
		
	}
}

class Derived2 extends Base2 {
    // Override 예시
	@Override
	protected void method() {  // 자식은 부모의 접근제한 이상만 가능(=>예제는 protected, public)		
	}
}
```

### final 클래스와 final 메소드

* final 클래스
  * final 클래스는 최종 클래스이므로 상속할 수 없는 클래스!
  * = 자식 클래스를 만들 수 없다!
* final 메소드
  * final 메소드는 최종 메소드이므로 오버라이딩할 수 없는 클래스!
  * = 자식 클래스에서 재정의 불가!

## 3. 타입 변환과 다형성

> **다형성:** 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질
>
> **타입 변환:** 데이터 타입을 다른 데이터 타입으로 변환하는 행위

### 자동 타입 변환

* 자식 클래스는 부모 클래스를 상속받았으므로 자식 객체를 생성해서 부모 타입으로 대입하면 자동 타입 변환이 된다.

* 부모 타입으로 자동 타입 변환되면 부모 클래스의 필드와 메소드에만 접근 가능!

  > **📌 주의**
  >
  > 자식 클래스에서 부모 클래스의 메소드를 오버라이드했다면 자식 클래스의 오버라이드된 메소드가 호출된다.
  >
  > 이것이 바로 다형성!

* Up-casting이라고도 한다.

### 강제 타입 변환(Casting)

* 부모 타입을 자식 타입으로 강제 변환하는 것.
* 모든 부모 타입을 자식 타입으로 강제 변환 가능한 것은 아니다.
  * **자식 타입을 부모 타입으로 자동 변환한 뒤에 다시 자식 타입으로 변환할 때만 가능!**
* Down-casing이라고도 한다.

```java
// 타입 변환에 관한 예제
public class InheritExample {
	public static void main(String[] args) {
        // 자동 타입 변환과 강제 타입 변환 예제
		Person p = new Student(20, "정윤호", "연극영화학과");  // 자식 객체를 부모타입의 변수에 넣을 수 있음: up-casting
		Student s2 = (Student) p;  // 부모타입의 객체를 자식 타입 변수에 넣기: down-casting
        
        // 다형성: 상속, 오버라이드했으므로 부모인 Person으로 타입 변환해도 결과는 자식이 Override한 것으로 나타난다.
		Person p = new Student(32, "정윤호", "실용음악학과");
		System.out.println(p.info());
	}
```

### 하나의 배열로 객체 관리

* 동일한 타입의 값은 배열로 관리하는 것이 효율적이고 코드가 더 깔끔해 보인다!

```java
// 배열로 객체를 생성한 예제
public class InheritExample {
	static Person[] pers = new Person[10];
	public static void main(String[] args) {		
		pers[0] = new Student(32, "정윤호", "실용음악학과");
		pers[1] = new Student(30, "최창민", "연극영화학과");
		pers[2] = new Professor(53, "이수만", "경영학과");
        // 다형성: student는 student, professor는 professor의 info()가 호출된다.
		for (int i=0; i<3; i++) {
			System.out.println(pers[i].info());
		}
	}
}
```

### `instanceof` 연산자

* 어떤 객체가 어떤 클래스의 인스턴스인지 확인하기 위해 사용하는 연산자
* 주로 강제 타입 변환이 필요할 때 참조하는 객체를 확인하기 위해 사용한다.
* 연산자에 쓰인 객체를 참조하면 `true`, 아니면 `false`를 반환한다.

```java
// instanceof 연산자 활용 예제
public class InheritExample {
	static Person[] pers = new Person[10];

	public static void main(String[] args) {
		pers[0] = new Student(32, "정윤호", "실용음악학과");
		pers[1] = new Student(30, "최창민", "연극영화학과");
		pers[2] = new Professor(53, "이수만", "경영학과");
        
		for (int i=0; i<3; i++) {
			if (pers[i] instanceof Student) {
				Student s = (Student) pers[i];
				System.out.println(s.getMajor());
			}
		}		
		for (int i=0; i<3; i++) {
			if (pers[i] instanceof Professor) {
				Professor p = (Professor) pers[i];
				System.out.println(p.getDepartment());
			}
		}
	}
}
```

# 💡 Project Bank - next()와 nextLine()

## `next()`와  `nextLine()`의 공통점

* `next()`와 `nextLine()`은 모두 자바의 Scanner 클래스의 메소드이다.
* 둘 다 입력받은 값을 문자열로 반환해 준다.

## `next()`와 `nextLine()의 차이점

* `next()`: 공백 이전의 문자나 문자열만 반환
* `nextLine()`: 엔터를 치기까지의 문자열을 모두 받아서 엔터값을 버리고 그 이전까지의 값을 반환

## ⚠ 문제1

```java
Scanner sc = new Scanner(System.in);

public void createAccount() {
	System.out.println("---------");
	System.out.println("일반계좌생성");
	System.out.println("---------");
	System.out.print("계좌번호: ");
	String id = sc.next();
	System.out.print("이름: ");
	String owner = sc.nextLine();
	System.out.print("초기입금액: ");
	int balance = sc.nextInt();
	System.out.println("개설완료");
}
```

계좌번호를 받기 위한 `sc.next()`가 실행되었을 때 마지막에 친 엔터값(`\n`)이 자동으로 `nextLine()`으로 들어가서 이름을 입력받지 않았는데 초기입금액으로 바로 넘어가는 문제 발생.

## ⚠ 문제1 해결과 문제2

```java
Scanner sc = new Scanner(System.in);

public void createAccount() {
	System.out.println("---------");
	System.out.println("일반계좌생성");
	System.out.println("---------");
	System.out.print("계좌번호: ");
	String id = sc.next();
	System.out.print("이름: ");
	String owner = sc.next();
	System.out.print("초기입금액: ");
	int balance = sc.nextInt();
	System.out.println("개설완료");
}
```

문제 1을 해결하기 위해 `owner`값도 `next()`로 입력받도록 했더니 외국인 이름(예: 마이클 버넘)처럼 중간에 공백이 들어가는 경우에는 입력한 문자열을 모두 받아오지 못하는 문제 발생.

## ⚠  문제2 해결과 문제3

```java
Scanner sc = new Scanner(System.in);

public void createAccount() {
	System.out.println("---------");
	System.out.println("일반계좌생성");
	System.out.println("---------");
	System.out.print("계좌번호: ");
	String id = sc.nextLine();
	System.out.print("이름: ");
	String owner = sc.nextLine();
	System.out.printLine("초기입금액: ");
	int balance = sc.nextInt();
}

public void createSpecialAccount() {
		System.out.println("---------");
		System.out.println("특수계좌생성");
		System.out.println("---------");
		System.out.print("계좌번호: ");
		String id = sc.nextLine();
		System.out.print("이름: ");
		String owner = sc.nextLine();
		System.out.print("초기입금액: ");
		int balance = sc.nextInt();
		System.out.println("등급\nVIP: 1, Gold: 2, Silver: 3, Normal: 4\n선택: ");
		int ngrade = Integer.parseInt(sc.nextLine());
		String grade = "Normal";
		switch(ngrade) {
		case 1: grade = "VIP"; break;
		case 2: grade = "Gold"; break;
		case 3: grade = "Silver"; break;
		case 4: grade = "Normal"; break;
		}
}
```

`id`와 `owner`를 모두 `nextLine()`으로 값을 받도록 해서 해결했지만 `balance`를 입력받을 때 사용한 `nextInt()`에서 입력한 엔터값이 특수계좌 개설 시 입력받는 `grade`의 값으로 자동으로 넘어가는 바람에 `NumberFormatException` 예외가 발생.

## 🔑 최종 해결

```java
Scanner sc = new Scanner(System.in);

public void createAccount() {
	System.out.println("---------");
	System.out.println("일반계좌생성");
	System.out.println("---------");
	System.out.print("계좌번호: ");
	String id = sc.nextLine();
	System.out.print("이름: ");
	String owner = sc.next();
	System.out.printLine("초기입금액: ");
	int balance = Integer.parseInt(sc.nextLine());
}

public void createSpecialAccount() {
	System.out.println("---------");
	System.out.println("특수계좌생성");
	System.out.println("---------");
	System.out.print("계좌번호: ");
	String id = sc.nextLine();
	System.out.print("이름: ");
	String owner = sc.nextLine();
	System.out.print("초기입금액: ");
	int balance = Integer.parseInt(sc.nextLine());
	System.out.println("등급\nVIP: 1, Gold: 2, Silver: 3, Normal: 4\n선택: ");
	int ngrade = Integer.parseInt(sc.nextLine());
	String grade = "Normal";
	switch(ngrade) {
	case 1: grade = "VIP"; break;
	case 2: grade = "Gold"; break;
	case 3: grade = "Silver"; break;
	case 4: grade = "Normal"; break;
	}
}
```

`balance`의 값도 `nextLine()`으로 받아서 받은 값을 정수로 반환해주는 `Integer.parseInt()`를 사용하여 최종적으로 문제 해결

