# Working with Regex.

A basic Example
```java
    Pattern pattern=Pattern.compile(" worl");
		Matcher matcher=pattern.matcher("The world is very beutiful world. And the worl is .");
		int count=0;
		while (matcher.find()){
			count++;
		}
		System.out.println(count);
```
