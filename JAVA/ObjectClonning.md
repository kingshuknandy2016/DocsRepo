### Types:

* Shallow Copy 
* Deep Copy
* Clonning

#### Shallow Copy
One object but two references. <br/>
Example:
```java
public class ShallowCopy {

	public static void main(String[] args) {
		Test1 test=new Test1();
		Test1 test2=test;
	}
}

class Test1{
	int i;
	int j;
}
```

#### Deep Copy:
```java
public class DeepCopy {

	public static void main(String[] args) {
		TestA test=new TestA();
		test.i=10;
		test.j=20;
		TestA test2=new TestA();
		test2.i=test.i;
		test2.j=test.j;
	}
}

class TestA{
	int i;
	int j;
}

```

#### Clonning
```java

```
