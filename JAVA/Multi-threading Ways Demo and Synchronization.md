```java
package com.basics;

public class ThreadingDemo1 {

	public static void main(String[] args) {
		new Thread(new A("Thread 1", 200)).start();
		new Thread(new A("Thread 2", 200)).start();
	}
}

class A implements Runnable {
	private String name;
	private int val;

	public A(String name, int val) {
		this.name = name;
		this.val = val;
	}

	@Override
	public void run() {
		System.out.println("NAME:" + name);
		for (int i = 0; i < val; i++) {
			System.out.println(name + " : " + i);
		}
	}
}

```
```java
package com.basics;

public class ThreadingDemo2 {

	public static void main(String[] args) {
		new B("Thread 1", 200).start();
		new B("Thread 2", 200).start();
	}
}

class B extends Thread {
	private String name;
	private int val;

	public B(String name, int val) {
		this.name = name;
		this.val = val;
	}

	@Override
	public void run() {
		System.out.println("NAME:" + name);
		for (int i = 0; i < val; i++) {
			System.out.println(name + " : " + i);
		}
	}

}

```
```java
Synchronized Method:
class Table {
	synchronized void printTable(int n) {// method not synchronized
		for (int i = 1; i <= 5; i++) {
			System.out.println(n * i);
			try {
				Thread.sleep(400);
			} catch (Exception e) {
				System.out.println(e);
			}
		}

	}
}

class ThreadA extends Thread {
	
	Table table;
	
	public ThreadA(Table table) {
		this.table=table;
	}
	@Override
	public void run() {
		table.printTable(5);
	}
}


class ThreadB extends Thread {
	
	Table table;
	
	public ThreadB(Table table) {
		this.table=table;
	}
	@Override
	public void run() {
		table.printTable(100);
	}
}

public class SynchronizationExample {

	public static void main(String[] args) {
		Table table=new Table();
		
		new ThreadA(table).start();
		new ThreadB(table).start();
	}
}

```

The Output
```java
Without Synchronization: Both the  Thread are using the same object method as it is not locked
5
100
10
200
15
300
20
400
25
500

With Synchronization: Unless the 1st object method relases the lock the second object cannot use it
5
10
15
20
25
100
200
300
400
500
```

*** Synchronized Block: ***
```java
package com.basics;

public class SynchronizationBlock {

	public static void main(String[] args) {
		TableOther table = new TableOther();

		new ThreadC(table).start();
		new ThreadD(table).start();
	}
}

class TableOther {
	void printTable(int n) {// method not synchronized
		synchronized (this) {
			for (int i = 1; i <= 5; i++) {
				System.out.println(n * i);
				try {
					Thread.sleep(400);
				} catch (Exception e) {
					System.out.println(e);
				}
			}
		}
	}
}

class ThreadC extends Thread {

	TableOther table;

	public ThreadC(TableOther table) {
		this.table = table;
	}

	@Override
	public void run() {
		table.printTable(5);
	}
}

class ThreadD extends Thread {

	TableOther table;

	public ThreadD(TableOther table) {
		this.table = table;
	}

	@Override
	public void run() {
		table.printTable(100);
	}
}

```
