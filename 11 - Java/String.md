There are two ways to create String object:

1. By string literal
2. By new keyword

### 1) String Literal

Java String literal is created by using double quotes. For Example:

1. String s="welcome";  

Each time you create a string literal, the <mark class="hltr-o">JVM checks the "string constant pool" first</mark>. If the string already exists in the pool, a reference to the pooled instance is returned. If the string doesn't exist in the pool, a new string instance is created and placed in the pool. For example:

1. String s1="Welcome";  
2. String s2="Welcome";//It doesn't create a new instance

![[Pasted image 20240604101050.png]]

In the above example, only one object will be created. Firstly, JVM will not find any string object with the value "Welcome" in string constant pool that is why it will create a new object. After that it will find the string with the value "Welcome" in the pool, it will not create a new object but will return the reference to the same instance.

### Why Java uses the concept of String literal?

To make Java more memory efficient (because no new objects are created if it exists already in the string constant pool).

### 2) By new keyword

1. String s=new String("Welcome");//creates two objects and one reference variable  

In such case, [JVM](https://www.javatpoint.com/jvm-java-virtual-machine) will <mark class="hltr-o">create a new string object in normal (non-pool) heap memory, and the literal "Welcome" will be placed in the string constant pool.</mark> The variable s will refer to the object in a heap (non-pool).

### Immutable

In Java, **String objects are immutable**. Immutable simply means unmodifiable or unchangeable.
Once String object is created its data or state can't be changed but a new String object is created.

```java
1. class Testimmutablestring{  
2.  public static void main(String args[]){  
3.    String s="Sachin";  
4.    s.concat(" Tendulkar");//concat() method appends the string at the end  
5.    System.out.println(s);//will print Sachin because strings are immutable objects  
6.  }  
7. }
```

![[Pasted image 20240604101357.png]]

```java
1. class Testimmutablestring1{  
2.  public static void main(String args[]){  
3.    String s="Sachin";  
4.    s=s.concat(" Tendulkar");  
5.    System.out.println(s);  
6.  }  
7. }
```

Output - Sachin Tendulkar
Bcoz here we are doing s=s.Concat..., in first example we are not doing that

In such a case, s points to the "Sachin Tendulkar". Please notice that still Sachin object is not modified.

### Why String objects are immutable in Java?

**Thread Safe:**
As the String object is immutable we don't have to take care of the synchronization that is required while sharing an object across multiple threads.

**Heap Space:**
The immutability of String helps to minimize the usage in the heap memory. When we try to declare a new String object, the JVM checks whether the value already exists in the String pool or not. If it exists, the same value is assigned to the new object. This feature allows Java to use the heap space efficiently.

### StringBuffer & StringBuilder
Since String is immutable in Java, whenever we do String manipulation like concatenation, substring, etc. it generates a new String and discards the older String for garbage collection. These are heavy operations and generate a lot of garbage in heap. So Java has provided StringBuffer and StringBuilder classes that should be used for String manipulation. <mark class="hltr-g">StringBuffer and StringBuilder are mutable objects in Java.</mark> They provide append(), insert(), delete(), and substring() methods for String manipulation.

StringBuffer was the only choice for String manipulation until Java 1.4. But, it has one disadvantage that all of its public methods are synchronized. <mark class="hltr-o">StringBuffer provides Thread safety but at a performance cost.</mark> In most of the scenarios, we don’t use String in a multithreaded environment. So Java 1.5 introduced a new class <mark class="hltr-o">StringBuilder, which is similar to StringBuffer except for thread-safety and synchronization</mark>.

If you are in a single-threaded environment or don’t care about thread safety, you should use StringBuilder. Otherwise, use StringBuffer for thread-safe operations.

1. String is immutable whereas StringBuffer and StringBuilder are mutable classes.
2. StringBuffer is thread-safe and synchronized whereas StringBuilder is not. That’s why StringBuilder is faster than StringBuffer.
3. String concatenation operator (+) internally uses StringBuffer or StringBuilder class.
4. For String manipulations in a non-multi threaded environment, we should use StringBuilder else use StringBuffer class.