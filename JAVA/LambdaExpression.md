```java
package com.basics;

public class LamdaDemo {
	public static void main(String[] args) {
		A obj=new XYZ();
	}
}

interface A {
    void show();
}

class XYZ implements A{

	@Override
	public void show() {
		// TODO Auto-generated method stub	
	}
	
}
```

```java
A obj=new A(){  //Anonymous Inner Class
		@Override
		public void show() {
			System.out.println("Show");
		}		
	};
```
```java
	A obj=()->{
		System.out.println("Show");
	};
```

```java
A obj=()->System.out.println("Show");
```
