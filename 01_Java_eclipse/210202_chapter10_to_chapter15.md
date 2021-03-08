# 💡 10장. 확인문제 4, 5번

## ✅ throw와 throws의 개념

### throws

* 생성자나 메소드의 선언 끝 부분에 사용되어 내부에서 발생된 예외를 떠넘긴다.
* `throws` 뒤에는 떠넘겨야 할 예외를 쉼표(`,`)로 구분해서 기술한다.
* 모든 예외를 떠넘기기 위해 간단하게 `throws Exception`으로 작성할 수 있다.

📌 새로운 예외를 발생시키기 위해 사용된다 👉 _**throw**에 대한 설명!_

### throw

* 예외를 최초로 발생시키는 코드이다.
* `throw`로 발생된 예외는 일반적으로 생성자나 메소드 선언부에 `throws`로 떠넘겨진다.
* `throw` 키워드 뒤에는 예외 객체 생성 코드가 온다.

📌 예외를 호출한 곳으로 떠넘기기 위해 메소드 선언부에 작성된다. 👉 _**throws**에 대한 설명!_

# 💡 멀티 스레드 (이어서)

## 6. 스레드 상태 제어

> 👉 실행 중인 스레드 상태를 변경하는 것

### 상태 제어  메소드의 종류

* join(): 스레드A가 실행되는 중에는 대기 상태였다가 스레드A가 끝나면 실행
* wait(): 일시 정지 상태로 만드는 메소드로 notify() 또는 notifyall()과 쌍으로 사용
* 사용하지 않는 것을 권장하는 메소드
  * resume(): 일시 정지 상태인 스레드를 강제로 실행 대기 상태로 변경
  * suspend(): 스레드를 일시 정지 상태로 변경
  * stop(): 스레드 즉시 종료

### 1️⃣ sleep()

> 👉 주어진 시간 동안 일시 정지

- 일시 정지할 시간은 밀리 세컨드(1/1000) 단위로 지정
- interrupt() 메소드를 호출해서 실행 대기 상태로 변경 가능하나 `InterruptedException`이 발생하므로 예외 처리 필수

```java
public class BeepPrintExample1 {
    
	public static void main(String[] args) {
		Thread thread = new BeepThread();
		thread.start();
		for(int i=0; i<5; i++) {
			System.out.println("띵");
			try { Thread.sleep(500); } catch(InterruptedException e) {}
		}
	}
}
```

### 2️⃣ yield()

> 👉 다른 스레드에게 실행 양보

* 같거나 더 높은 우선순위를 가진 다른 스레드에게 실행을 양보하고 실행 대기 상태로 간다.

### 3️⃣ join()

> 👉 다른 스레드의 종료를 기다림

- 계산 작업을 하는 스레드가 모든 계산 작업을 마쳤을 때 그 결과값을 받아서 이용하는 경우에 주로 사용하는 메소드

### 4️⃣ wait(), notify(), notifyAll()

> 👉 스레드 간 협업

- 동기화 메소드 또는 동기화 블록에서만 호출 가능한 Object 클래스의 메소드
- 두 개의 스레드가 교대로 번갈아서 실행되어야 할 경우에 사용

### 5️⃣ stop 플래그, interrupt()

> 👉 스레드가 실행 중인데 즉시 종료할 경우, stop() 메소드 대신 안전하게 스레드를 종료하는 방법

#### stop 플래그

- stop 필드가 true 또는 false가 될 때를 이용해서 실행 중인 스레드를 종료한다.

```java
public class PrintThread1 extends Thread {
	boolean stop;
	public void setStop(boolean stop) {
		this.stop = stop;
	}
	@Override
	public void run() {
		while(stop==false) {
			System.out.println("실행 중");
		}
		System.out.println("자원 정리");
		System.out.println("실행 종료");
	}
}
```

```java
public class StopFlagExample {

	public static void main(String[] args) {
		PrintThread1 printThread = new PrintThread1();
		printThread.start();
		try {
			Thread.sleep(1000);
		} catch(InterruptedException e) {}
		printThread.setStop(true);
		//printThread.stop(); // 갑자기 실행중단됨. stop은 사용하지 않길 권고함
	}
}
```

#### interrupt() 메소드

- interrupt() 메소드 사용 시 `InterruptedException`을 발생시키는 것을 이용하여 실행 중인 스레드를 종료한다.
- **주의:** 스레드가 실행 대기 또는 실행 상태일 때는 `InterruptedException`이 발생하지 않고, 나중에 일시 정지 상태가 되면 발생한다.

```java
public class PrintThread2 extends Thread {
	@Override
	public void run() {
		// 아래의 try나 while문 중 하나를 사용할 수 있다.
//		try {
//			while(true) {
//				System.out.println("실행 중");
//				Thread.sleep(1);  // 스레드를 일부러 일시 정지시켜 예외 발생 유도
//			}
//		} catch(InterruptedException e) {
//			System.out.println(e.getMessage());  // interrupt는 예외를 발생시켜서 강제로 빠져나가게 하므로 catch에 interrupt 발생 시 할 작업을 넣어준다.
//		} finally {
//			System.out.println("마무리 작업");
//		}
	
		while(true) {
			System.out.println("실행 중");
			if(Thread.interrupted()) {  // 현재 스레드가 interrupted되었는지 확인용
				System.out.println("마무리 작업");
				break;
			}
		}
		System.out.println("마무리 작업");
	}
}
```

```java
public class InterruptExample {

	public static void main(String[] args) {
		Thread thread = new PrintThread2();
		thread.start();
		try {
			Thread.sleep(1000);
		} catch(InterruptedException e) {}
		thread.interrupt();
	}
}
```

## 7. 데몬 스레드 (Daemon Thread)

> 👉 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드

- 주 스레드가 종료되면 데몬 스레드도 강제 자동 종료

- 데몬 스레드 만드는 방법

  > 주 스레드가 데몬 스레드의 `setDaemon(true)`를 호출

  - `start()` 메소드 호출 전에 데몬 스레드를 호출해야 `IllegalThreadStateException`이 발생하지 않는다!

- 현재 실행 중인 스레드가 데몬 스레드인지 구별하는 방법

  > `isDaemon()` 메소드 사용

```java
public class AutoSaveThread extends Thread {
	public void save() {
		System.out.println("작업 내용을 저장함.");
	}
	@Override
	public void run() {
		while(true) {
			try {
				Thread.sleep(1000);
			} catch(InterruptedException e) {
				break;
			}
			save();
		}
	}
}
```

```java
public class DaemonExample {

	public static void main(String[] args) {
		AutoSaveThread autoSaveThread = new AutoSaveThread();
		autoSaveThread.setDaemon(true);
		autoSaveThread.start();
		try {
			Thread.sleep(3000);
		} catch(InterruptedException e) {}
		System.out.println("메인 스레드 종료");
	}
}

```

## 8. 스레드 그룹

- 스레드 그룹은 관련된 스레드를 묶어서 관리 목적으로 이용
- 계층적으로 하위 스레드 그룹을 가질 수 있다.
- 스레드는 반드시 하나의 스레드 그룹에 포함되며, 기본적으로 자신을 생성한 스레드와 같은 그룹에 속한다.
- system 스레드 그룹
  - JVM이 자동으로 생성하는 스레드 그룹
- system/main 스레드 그룹
  - system 그룹의 하위 스레드 그룹으로 main 스레드가 포함된 그룹
- 부모 그룹을 지정해주지 않은 경우에는 현재 스레드가 속한 그룹의 하위 그룹으로 생성

### 스레드 그룹 이름 얻기

* 현재 스레드가 속한 스레드 그룹의 이름을 얻는 코드

  ```java
  ThreadGroup group = Thread.currentThread.getThreadGroup();
  String groupName = group.getName();
  ```

### 스레드 그룹 생성

* 스레드 그룹을 명시적으로 생성하는 코드

  ```java
  ThreadGroup tg = new ThreadGroup(String name);
  ThreadGroup tg = new ThreadGroup(ThreadGroup parent, String name);
  ```

# 💡 컬렉션 프레임워크

## 1. 컬렉션 프레임워크 소개

### ⚠  배열의 문제점

- 배열은 쉽게 생성하고 사용할 수 있지만 저장할 수 있는 객체 수가 배열 생성시에 이미 결정되기 때문에 불특정 다수의 객체를 저장할 수 없음
- 객체를 삭제했을 때 해당 인덱스가 비어 있게 되어 나중에 수정하기가 어려움

### 🗝 자바의 컬렉션 프레임워크

#### 컬렉션 프레임워크

> 자료구조를 바탕으로 객체를 효율적으로 추가, 삭제, 검색할 수 있도록 제공되는 컬렉션 라이브러리

- java.util 패키지에 컬렉션과 관련된 인터페이스와 클래스가 포함되어 있다.
- **자바 컬렉션:** 객체를 수집해서 저장하는 역할
- **프레임워크:** 사용 방법을 미리 정해 놓은 라이브러리

#### 컬렉션 프레임워크의 주요 인터페이스

- Collection
  - List: 순서 유지 및 저장, 중복 저장 가능 (ArrayList, Vector, LinkedList)
  - Set: 순서없이 저장하지만 동일한 데이터는 중복 저장 불가능 (HashSet, TreeSet)
- Map: key-value 한 쌍으로 저장하며 key는 중복 저장 불가능 (HashMap, Hashtable, TreeMap, Properties)

## 2. List 컬렉션

### 📌 특징

1. 객체를 인덱스로 관리한다.
2. 동일한 객체를 중복 저장 가능하며, 이때에는 동일한 번지를 참조한다.
3. null값도 저장이 가능하다.
4. List 인터페이스는 제네릭 타입이다 - 파라미터: `<E>`

### List 인터페이스의 구현 클래스

* ArrayList, Vector, LinkedList
  * 객체 추가: `add()`
  * 객체 찾기: `get()`
  * 객체 삭제: `remove()`

### 1️⃣ ArrayList

> **📌 배열과 달리 저장 용량을 초과한 객체가 들어오면 저장 용량이 자동으로 늘어난다!!**

- 기본 생성자로 생성 시 초기 용량은 10
- 객체를 추가하면 0부터 차례대로 저장
- 특정 인덱스의 객체를 제거하면 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨진다.
- 객체를 삽입하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 밀려난다.
- 객체 제거 및 삽입을 자주 할 때에는 **LinkedList**를 사용하는 편이 성능면에서 더 낫다.
- 인덱스 검색 및 맨 마지막에 객체 추가 시 **ArrayList** 사용

```java
// ArrayList 사용 예시
import java.util.ArrayList;

public class ArrayListExample {
	public static void main(String[] args) {
		ArrayList<String> list = new ArrayList<String>();  // 2. generic을 사용한다. <String> 제네릭을 사용하면 배열의 타입도 String이 된다.
		list.add("hong");
		list.add("gil");
		list.add("dong");
		//String str = list.get(0);  // 1. 매번 casting을 해줘야 해서
//		for(int i=0; i<list.size(); i++) {
//			System.out.println(list.get(i));
//		}
		//System.out.println(str);
		
		// 향상된 for문 사용에 최적화되어 있음
		for( String name : list ) {
			System.out.println(name);
		}
		list.remove(0);
		System.out.println();
		for(String name : list) {
			System.out.println(name);
		}		
		list.remove("dong");
		System.out.println();
		for(String name : list) {
			System.out.println(name);
		}
	}
}
```

### 2️⃣ Vector

- `ArrayList`와 동일한 구조

> 📌 차이점: 동기화된 메소드로 구성되어 있어서 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제 가능!
>
> ​				   **➡ 스레드가 안전하다**

```java
public class Korean {
	String juminNo;
	String name;
	String addr;
	public Korean(String juminNo, String name, String addr) {}
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListObjExample {
	public static void main(String[] args) {
		List<Korean> kors = new ArrayList<Korean>();
		Korean k = new Korean("121212", "park", "seoul");
		kors.add(k);
		kors.add(new Korean("131313", "lee", "incheon"));
		kors.add(new Korean("141414", "song", "busan"));
		kors.add(new Korean("121212", "park", "seoul"));
		kors.remove(k);
	}

}
```

### 3️⃣ LinkedList

> **📌 인접 참조를 링크해서 체인처럼 관리**

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class LinkedListExample {
	public static void main(String[] args) {
		List<String> list1 = new ArrayList<String>();
		List<String> list2 = new LinkedList<String>();
		
		long startTime;
		long endTime;
		startTime = System.nanoTime();
        
		for(int i=0; i<10000; i++) {
			list1.add(0, String.valueOf(i));
		}
		endTime = System.nanoTime();
		System.out.println("ArrayList 걸린 시간: " + (endTime-startTime) + " ns");
		
		startTime = System.nanoTime();
		for(int i=0; i<10000; i++) {
			list2.add(0, String.valueOf(i));
		}
		endTime = System.nanoTime();
		System.out.println("LinkedList 걸린 시간: " + (endTime-startTime) + " ns");
		
		startTime = System.nanoTime();
		list1.get(list1.size()-1);
		endTime = System.nanoTime();
		System.out.println("ArrayList 걸린 시간: " + (endTime-startTime) + " ns");
		
		startTime = System.nanoTime();
		list2.get(list2.size()-1);
		endTime = System.nanoTime();
		System.out.println("LinkedList 걸린 시간: " + (endTime-startTime) + " ns");
	}
}

// RESULT!
//ArrayList 걸린 시간: 14641500 ns
//LinkedList 걸린 시간: 6516200 ns
//ArrayList 걸린 시간: 135100 ns
//LinkedList 걸린 시간: 36800 ns
```

- 특정 인덱스에서 객체를 제거하거나 추가하면 해당 객체 앞뒤 링크만 변경되므로 객체 삭제 또는 삽입이 자주 발생할 경우에는 `ArrayList`보다 좋은 성능 발휘

## 3. Set 컬렉션

### 📌 특징

1. 수학의 집합처럼 저장 순서가 유지되지 않으며 객체 중복 저장 불가능
2. 하나의 null만 저장 가능
3. Set 인터페이스는 제네릭 타입
4. 인덱스로 객체를 검색해서 가져오는 메소드 없음🙅‍♀️!!
5. 전체 객체를 대상으로 한 번씩 반복해서 가져오는 반복자(`iterator()`메소드)를 제공!

### Set 인터페이스의 구현 클래스

- HashSet, LinkedHashSet, TreeSet
  - 객체 추가: `add()`
  - 객체 삭제: `remove()`

### Iterator 인터페이스

- `hasNext()`: 가져올 객체가 있으면 `true`, 없으면 `false` 반환
- `next()`: `hasNext()`에서 `true`가 반환됐을 때 사용
- 하지만 향상된 for문을 사용해서 전체 객체를 대상으로 반복 가능!

### 1️⃣ HashSet

- 객체들을 순서 없이 저장하고 동일한 객체 및 동등한 객체 중복 저장🙅‍♀️
- 동등 객체 판단 방법은 `hashCode()`와 `equals()`의 리턴값으로 판별

```java
public class Member {
	public String name;
	public int age;	
	public Member(String name, int age) {
		this.name = name;
		this.age = age;
	}

	// 같은 데이터 값이 중복으로 들어가는 것을 판별해 주는 코드
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member) obj;
			return member.name.equals(name) && (member.age==age) ;
		} else {
			return false;
		}
	}
	public int hashCode() {
		return name.hashCode() + age;
	}
}
```

```java
import java.util.HashSet;

public class HashSetExample2 {
	public static void main(String[] args) {
		HashSet<Member> mems = new HashSet<Member>();
		
		Member mem1 = new Member("hong", 20);
		Member mem2 = new Member("hong", 20);
		Member mem3 = new Member("hong", 30);
		Member mem4 = new Member("park", 20);
		
		mems.add(mem1);
		mems.add(mem2);
		mems.add(mem3);
		mems.add(mem4);
		
		for(Member m : mems) {
			System.out.println(m.name);
		}
	}
}
```

### 2️⃣ TreeSet

- 출력 시 데이터를 정렬해서 순서대로 출력

```java
import java.util.TreeSet;

public class TreeSetExample {
	public static void main(String[] args) {
		TreeSet<Integer> ts = new TreeSet<Integer>();  // Integer는 int(기본변수)를 클래스화 시켜주는(boxing) 레퍼런스 클래스
		ts.add(10);
		ts.add(5);
		ts.add(7);
		ts.add(20);
		
		// 정렬되어서 순서대로 출력해줌
		for(int n : ts) {
			System.out.println(n);
		}
	}
}
```

## 4. Map 컬렉션

### 📌 특징

1. 키(key)와 값(value)으로 구성된 Entry 객체를 저장하는 구조
2. 키와 값 모두 객체
3. 하나의 키에 여러 개의 값 중복 저장 가능

### Map 인터페이스의 구현 클래스

- HashMap, Hashtable, LinkedHashMap, Properties, TreeMap 등
  - 객체 추가: `put()`
  - 객체 찾기: `get()`
  - 객체 삭제: `remove()`
- 저장된 모든 객체를 대상으로 객체 찾는 방법
  	1. `keySet()` 메소드로 전체 키를 Set 컬렉션으로 얻음 ➡ 반복자(`Iterator`)를 사용해 키를 하나씩 얻음 ➡ `get()` 메소드로 값을 얻음
   	2. `entrySet()` 메소드로 모든 Map.Entry를 Set 컬렉션으로 얻음 ➡ 반복자를 통해 Map.Entry를 하나씩 얻음 ➡ `getKey()`와 `getValue()` 메소드로 키와 값을 얻음

### 1️⃣ HashMap

- 키로 사용할 객체는 `hashCode()`와 `equals()` 메소드를 재정의해서 동등 객체 조건을 지정해야 한다.
- 키 타입은 주로 String을 많이 사용
- 키와 값의 타입은 클래스 및 인터페이스 타입만 가능
- 최근에는 Hashtable보다 더 많이 사용한다.

```java
public class HashMapExample {

	public static void main(String[] args) {
		HashMap<Integer, String> hm = new HashMap<Integer, String>();
		//Map<Integer, String> map = Collections.synchronizedMap(hm);  // 수동으로 동기화
		hm.put(100, "hong");
		hm.put(101, "park");
		hm.put(103, "choi");
		
		System.out.println(hm.get(103));
		Set<Integer> kset = hm.keySet();  // keySet(): key를 set객체에 담아서 리턴
		for(int k : kset) {
			System.out.println(k);
		}
		
		Set<Map.Entry<Integer, String>> eset = hm.entrySet();
		Iterator<Map.Entry<Integer, String>> sit = eset.iterator();
		while(sit.hasNext()) {
			Map.Entry<Integer, String> entry = sit.next();
			Integer key = entry.getKey();  // key값 가져오기
			String value = entry.getValue();  // value값 가져오기
			System.out.println(key + ":" + value);
		}
		System.out.println();
		
		// 실제로 많이 쓰이는 것은 key에서 value를 찾아내는 것
		for (String value : hm.values()) {
			System.out.println(value);
		}
	}
}
```

### 2️⃣ Hashtable

- HashMap과 같은 점
  - 키로 사용할 객체는 `hashCode()`와 `equals()` 메소드를 재정의해서 동등 객체 조건을 지정해야 한다.
- HashMap과 다른 점
  - 동기화된(`synchronized`) 메소드로 구성되어 있어서 **멀티 스레드 환경에서 안전**하다

### 3️⃣ Properties

- Hashtable의 하위 클래스
- Hashtable과의 차이점은 키와 값이 String 타입으로 제한되어 있다.
- 프로퍼티 파일(.properties)을 읽을 때 주로 사용
- 프로퍼티 파일을 읽는 방법
  1. Properties 객체 생성
  2. `load()` 메소드 호출
     - `load()` 메소드는 `FileReader` 객체를 매개값으로 받는다.

```java
public class PropertiesExample {
	public static void main(String[] args) throws IOException {
		Properties prop = new Properties();
		prop.setProperty("driver", "oracle.jdbc.OracleDriver");
		prop.setProperty("url", "jdbc:oracle:thin:@localhost:1521:xe");
		prop.setProperty("username", "hr");
		prop.setProperty("password", "hr");		
	}
}
```

```properties
#database set
#Tue Feb 02 17:24:34 KST 2021
password=hr
driver=oracle.jdbc.OracleDriver
url=jdbc:oracle:thin:@localhost:1521:xe
username=hr
```

```java
import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;

public class PropertiesExample2 {
	public static void main(String[] args) throws IOException {
		Properties prop = new Properties();
		prop.load(new FileReader("db.properties"));
		
		String driver = prop.getProperty("driver");
		String url = prop.getProperty("url");
		String username = prop.getProperty("username");
		String password = prop.getProperty("password");
		
		System.out.println("driver: " + driver);
		System.out.println("url: " + url);
		System.out.println("username: " + username);
		System.out.println("password: " + password);
	}
}
```



## 5. 검색 기능을 강화시킨 컬렉션

### 📌 특징

- 이진 트리(binary tree) 구조를 이용해서 계층적인 구조를 갖는 컬렉션이다.
- 각 노드 당 최대 1개의 노드를 연결할 수 있으며, 위아래로 연결된 두 노드를 부모-자식 관계라고 부른다.
- 부모 노드의 값보다 작은 노드는 부모 노드의 왼쪽, 부모 노드의 값보다 큰 노드는 부모 노드의 오른쪽에 위치
- 루트 노드: 첫 번째로 저장되는 값
- 문자를 저장할 경우에는 문자의 유니코드 값을 비교한다.
- 값이 정렬되어 있어서 그룹핑이 쉽고, 검색 속도가 빠르다!

### 1️⃣ TreeSet

- 이진 트리 기반의 Set 컬렉션
- 노드 구성 요소
  - value: 노드의 값
  - 변수1: 부모 노드 값보다 낮은 값을 가진 왼쪽 자식 노드를 참조하는 변수
  - 변수2: 부모 노드 값보다 높은 값을 가진 오른쪽 자식 노드를 참조하는 변수
- 값을 정렬해서 출력할 때 사용하는 메소드
  - `descendingKeySet()`: 내림차순으로 정렬된 키의 `NavigableSet`객체 리턴

```java
import java.util.TreeSet;

public class TreeSetExample {
	public static void main(String[] args) {
		TreeSet<Integer> ts = new TreeSet<Integer>();  // Integer는 int(기본변수)를 클래스화 시켜주는(boxing) 레퍼런스 클래스
		ts.add(10);
		ts.add(5);
		ts.add(7);
		ts.add(20);
		// 정렬되어서 순서대로 출력해줌
		for(int n : ts) {
			System.out.println(n);
		}
	}
}
```

### 2️⃣ TreeMap

- 이진 트리 기반의 Map 컬렉션
- TreeSet과 다른 점
  - 키와 값이 저장된 `Map.Entry`를 저장
- 부모의 키값보다 낮은 것은 왼쪽 자식 노드, 부모의 키값보다 높은 것은 오른쪽 자식 노드에 자동 정렬
- 값을 정렬해서 출력할 때 사용하는 메소드
  - `descendingKeyMap()` : 내림차순으로 정렬된 `NavigableMap`의 객체 리턴

```java
// 특정 Map.Entry를 찾는 예제와 정렬 메소드 사용 예제
import java.util.NavigableMap;
import java.util.TreeMap;

public class TreeMapExample1 {
	public static void main(String[] args) {
		TreeMap<Integer,String> scores = new TreeMap<Integer,String>();
		scores.put(new Integer(87), "홍길동");
		scores.put(new Integer(98), "이동수");
		scores.put(new Integer(75), "박길순");
		scores.put(new Integer(95), "신용권");
		scores.put(new Integer(80), "김자바");
		
//		Map.Entry<Integer, String> entry = null;
//		
//		entry = scores.firstEntry();
//		System.out.println("가장 낮은 점수: " + entry.getKey() + "-" + entry.getValue());
//		
//		entry = scores.lastEntry();
//		System.out.println("가장 높은 점수: " + entry.getKey() + "-" + entry.getValue() + "\n");
//		
//		entry = scores.lowerEntry(new Integer(95));
//		System.out.println("95점 아래 점수: " + entry.getKey() + "-" + entry.getValue());
//		
//		entry = scores.higherEntry(new Integer(95));
//		System.out.println("95점 위의 점수: " + entry.getKey() + "-" + entry.getValue() + "\n");
//		
//		entry = scores.floorEntry(new Integer(95));
//		System.out.println("95점 이거나 바로 아래 점수: " + entry.getKey() + "-" + entry.getValue());
//		
//		entry = scores.ceilingEntry(new Integer(85));
//		System.out.println("85점 이거나 바로 위의 점수: " + entry.getKey() + "-" + entry.getValue() + "\n");
//		
//		while(!scores.isEmpty()) {
//			entry = scores.pollFirstEntry();
//			System.out.println(entry.getKey() + "-" + entry.getValue() + "(남은 객체 수: " + scores.size() + ")");
//		}
		
		NavigableMap<Integer,String> dscores = scores.descendingMap();
		for(Integer k : dscores.keySet()) {
			System.out.println("key: " + k + ", value: " + dscores.get(k));
		}
	}
}
```

## 6. 동기화된 컬렉션

### 📌 특징

- ArrayList, HashSet, HashMap은 동기화된 메소드로 구성되어 있지 않아서 멀티 스레드 환경에서 안전🙅‍♀️
- `synchronizedXXX()`: 비동기화된 메소드를 동기화된 메소드로 래핑하는 Collections의 메소드

