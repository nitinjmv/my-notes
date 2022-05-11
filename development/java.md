Basics
------

**Oops Concepts**

- Encapsulation - data hiding, e.g. variables can be hidden.
- Abstraction - hiding implementation details e.g. Interface.
- Polymorphism - Many name e.g. method overloading & overriding.
- Inheritance - reusability e.g. class can be extended to another class.

**Question:** Describe the Meaning of the Final Keyword When Applied to a Class, Method, Field or a Local Variable.

**Answer:** The final keyword has multiple different meanings when applied to different language constructs:

A final class is a class that cannot be subclassed
A final method is a method that cannot be overridden in subclasses
A final field is a field that has to be initialized in the constructor or initializer block and cannot be modified after that
A final variable is a variable that may be assigned (and has to be assigned) only once and is never modified after that

**Question:** What Is a Default Method?

**Answer:** Prior to Java 8, interfaces could only have abstract methods, i.e. methods without a body. Starting with Java 8, interface methods can have a default implementation. If an implementing class does not override this method, then the default implementation is used. Such methods are suitably marked with a default keyword.

One of the prominent use cases of a default method is adding a method to an existing interface. If you don't mark such interface method as default, then all existing implementations of this interface will break. Adding a method with a default implementation ensures binary compatibility of legacy code with the new version of this interface.

A good example of this is the Iterator interface which allows a class to be a target of the for-each loop. This interface first appeared in Java 5, but in Java 8 it received two additional methods, forEach, and spliterator. They are defined as default methods with implementations and thus do not break backward compatibility:

```
public interface Iterable<T> {

    Iterator<T> iterator();

    default void forEach(Consumer<? super T> action) { /* */ }

    default Spliterator<T> spliterator() { /* */ }
}
```

**Question:** What Are Static Class Members?

**Answer:** Static fields and methods of a class are not bound to a specific instance of a class. Instead, they are bound to the class object itself. The call of a static method or addressing a static field is resolved at compile time because, contrary to instance methods and fields, we don't need to walk the reference and determine an actual object we're referring to.

**Question:** May a Class Be Declared Abstract If It Does Not Have Any Abstract Members? What Could Be the Purpose of Such Class?

**Answer:** Yes, a class can be declared abstract even if it does not contain any abstract members. As an abstract class, it cannot be instantiated, but it can serve as a root object of some hierarchy, providing methods that can be useful to its implementations.

**Question:** What Is Constructor Chaining?

**Answer:** Constructor chaining is a way of simplifying object construction by providing multiple constructors that call each other in sequence.

The most specific constructor may take all possible arguments and may be used for the most detailed object configuration. A less specific constructor may call the more specific constructor by providing some of its arguments with default values. At the top of the chain, a no-argument constructor could instantiate an object with default values.

Here's an example with a class that models a discount in percents that are available within a certain amount of days. The default values of 10% and 2 days are used if we don't specify them when using a no-arg constructor:

```
public class Discount {

    private int percent;

    private int days;

    public Discount() {
        this(10);
    }

    public Discount(int percent) {
        this(percent, 2);
    }

    public Discount(int percent, int days) {
        this.percent = percent;
        this.days = days;
    }

}
```

**Question:** What Is Overriding and Overloading of Methods? How Are They Different?

**Answer:** Overriding of a method is done in a subclass when you define a method with the same signature as in superclass. This allows the runtime to pick a method depending on the actual object type that you call the method on. Methods toString, equals, and hashCode are overridden quite often in subclasses.

Overloading of a method happens in the same class. Overloading occurs when you create a method with the same name but with different types or number of arguments. This allows you to execute a certain code depending on the types of arguments you provide, while the name of the method remains the same.

Here's an example of overloading in the java.io.Writer abstract class. The following methods are both named write, but one of them receives an int while another receives a char array.

```
public abstract class Writer {

    public void write(int c) throws IOException {
        // ...
    }

    public void write(char cbuf[]) throws IOException {
        // ...
    }

}
```

**Question:** Q7. Can You Override a Static Method?

**Answer:** No, you can't. By definition, you can only override a method if its implementation is determined at runtime by the type of the actual instance (a process known as the dynamic method lookup). The static method implementation is determined at compile time using the type of the reference, so overriding would not make much sense anyway. Although you can add to subclass a static method with the exact same signature as in superclass, this is not technically overriding.

**Question:** What Is an Immutable Class, and How Can You Create One?

**Answer:** An instance of an immutable class cannot be changed after it's created. By changing we mean mutating the state by modifying the values of the fields of the instance. Immutable classes have many advantages: they are thread-safe, and it is much easier to reason about them when you have no mutable state to consider.

To make a class immutable, you should ensure the following:

- All fields should be declared private and final; this infers that they should be initialized in the constructor and not changed ever since;
- The class should have no setters or other methods that mutate the values of the fields;
- All fields of the class that were passed via constructor should either be also immutable, or their values should be copied before field initialization (or else we could change the state of this class by holding on to these values and modifying them);
- The methods of the class should not be overridable; either all methods should be final, or the constructor should be private and only invoked via static factory method.

**Question:** How Do You Compare Two Enum Values: With equals() or With ==?

**Answer:** Actually, you can use both. The enum values are objects, so they can be compared with equals(), but they are also implemented as static constants under the hood, so you might as well compare them with ==. This is mostly a matter of code style, but if you want to save character space (and possibly skip an unneeded method call), you should compare enums with ==.

**Question:** What Is an Initializer Block? What Is a Static Initializer Block?

**Answer:** An initializer block is a curly-braced block of code in the class scope which is executed during the instance creation. You can use it to initialize fields with something more complex than in-place initialization one-liners.

Actually, the compiler just copies this block inside every constructor, so it is a nice way to extract common code from all constructors.

A static initializer block is a curly-braced block of code with the static modifier in front of it. It is executed once during the class loading and can be used for initializing static fields or for some side effects.

**Question:** What Is a Marker Interface? What Are the Notable Examples of Marker Interfaces in Java?

**Answer:** A marker interface is an interface without any methods. It is usually implemented by a class or extended by another interface to signify a certain property. The most widely known marker interfaces in standard Java library are the following:

- Serializable is used to explicitly express that this class can be serialized;

- Cloneable allows cloning objects using the clone method (without Cloneable interface in place, this method throws a CloneNotSupportedException)

- Remote is used in RMI to specify an interface which methods could be called remotely.

**Question:** Q12. What Is a Singleton and How Can It Be Implemented in Java?

**Answer:** Singleton is a pattern of object-oriented programming. A singleton class may only have one instance, usually globally visible and accessible.

There are multiple ways of creating a singleton in Java. The following is the most simple example with a static field that is initialized in-place. The initialization is thread-safe because static fields are guaranteed to be initialized in a thread-safe manner. The constructor is private, so there is no way for outer code to create more than one instance of the class.

```
public class SingletonExample {

    private static SingletonExample instance = new SingletonExample();

    private SingletonExample() {}

    public static SingletonExample getInstance() {
        return instance;
    }
}
```

But this approach could have a serious drawback — the instance would be instantiated when this class is first accessed. If initialization of this class is a heavy operation, and we would probably like to defer it until the instance is actually needed (possibly never), but at the same time keep it thread-safe. In this case, we should use a technique known as double-checked locking.

**Question:** What Is a Var-Arg? What Are the Restrictions on a Var-Arg? How Can You Use It Inside the Method Body?

**Answer:** Var-arg is a variable-length argument for a method. A method may have only one var-arg, and it has to come last in the list of arguments. It is specified as a type name followed by an ellipsis and an argument name. Inside the method body, a var-arg is used as an array of specified type.

Here's an example from the standard library — the Collections.addAll method that receives a collection, a variable number of elements, and adds all elements to the collection:

```
public static <T> boolean addAll(
  Collection<? super T> c, T... elements) {
    boolean result = false;
    for (T element : elements)
        result |= c.add(element);
    return result;
}
```

**Question:** Q14. Can You Access an Overridden Method of a Superclass? Can You Access an Overridden Method of a Super-Superclass in a Similar Way?

**Answer:** To access an overridden method of a superclass, you can use the super keyword. But you don't have a similar way of accessing the overridden method of a super-superclass.

As an example from the standard library, LinkedHashMap class extends HashMap and mostly re-uses its functionality, adding a linked list over its values to preserve iteration order. LinkedHashMap re-uses the clear method of its superclass and then clears head and tail references of its linked list:

```
public void clear() {
    super.clear();
    head = tail = null;
}
```

**Question:** What New Features Were Added in Java 8?

**Answer:**

- Lambda Expressions − a new language feature allowing us to treat actions as objects
- Method References − enable us to define Lambda Expressions by referring to methods directly using their names
- Optional − special wrapper class used for expressing optionality
- Functional Interface – an interface with maximum one abstract method; implementation can be provided using a Lambda Expression
- Default methods − give us the ability to add full implementations in interfaces besides abstract methods
- Nashorn, JavaScript Engine − Java-based engine for executing and evaluating JavaScript code
- Stream API − a special iterator class that allows us to process collections of objects in a functional manner
- Date API − an improved, immutable JodaTime-inspired Date API
Along with these new features, lots of feature enhancements are done under the hood at both the compiler and JVM level.

**Question:** Q1. What Is a Method Reference?

**Answer:** A method reference is a Java 8 construct that can be used for referencing a method without invoking it. It's used for treating methods as Lambda Expressions. They only work as syntactic sugar to reduce the verbosity of some lambdas. This way the following code:

```
(o) -> o.toString();
```

Can become:

```
Object::toString();
```

A method reference can be identified by a double colon separating a class or object name, and the name of the method. It has different variations, such as constructor reference:

```
String::new;
```

Static method reference:

```
String::valueOf;
```

Bound instance method reference:

```
str::toString;
```

Unbound instance method reference:

```
String::toString;
```

**Question:** Q2. What Is the Meaning of String::Valueof Expression?

**Answer:** It's a static method reference to the valueOf method of the String class.

**Question:** Q1. What Is Optional? How Can It Be Used?
  
**Answer:** Optional is a new class in Java 8 that encapsulates an optional value, i.e. a value that is either there or not. It's a wrapper around an object, and we can think of it as a container of zero or one element.

Optional has a special Optional.empty() value instead of wrapped null. Thus it can be used instead of a nullable value to get rid of NullPointerException in many cases.

The main purpose of Optional, as designed by its creators, is to be a return type of methods that previously would return null. Such methods would require us to write boilerplate code to check the return value, and we could sometimes forget to do a defensive check. In Java 8, an Optional return type explicitly requires us to handle null or non-null wrapped values differently.

For instance, the Stream.min() method calculates the minimum value in a stream of values. But what if the stream is empty? If it wasn't for Optional, the method would return null or throw an exception.

However, it returns an Optional value, which may be Optional.empty() (the second case). This allows us to easily handle such cases:

```
int min1 = Arrays.stream(new int[]{1, 2, 3, 4, 5})
  .min()
  .orElse(0);
assertEquals(1, min1);

int min2 = Arrays.stream(new int[]{})
  .min()
  .orElse(0);
assertEquals(0, min2);
```

It's worth noting that Optional is not a general purpose class like Option in Scala. It's not recommended that we use it as a field value in entity classes, which is clearly indicated by it not implementing the Serializable interface.

**Question:** Q1. Describe Some of the Functional Interfaces in the Standard Library

**Answer:** There are a lot of functional interfaces in the java.util.function package. The more common ones include, but are not limited to:

- Function – it takes one argument and returns a result

- Consumer – it takes one argument and returns no result (represents a side effect)

- Supplier – it takes no arguments and returns a result

- Predicate – it takes one argument and returns a boolean

- BiFunction – it takes two arguments and returns a result

- BinaryOperator – it is similar to a BiFunction, taking two arguments and returning a result. The two arguments and the result are all of the same types.

- UnaryOperator – it is similar to a Function, taking a single argument and returning a result of the same type

**Question:** What Is a Functional Interface? What Are the Rules of Defining a Functional Interface?

**Answer:** A functional interface is an interface with one single abstract method (default methods do not count), no more, no less.

Where an instance of such an interface is required, a Lambda Expression can be used instead. More formally put: Functional interfaces provide target types for lambda expressions and method references.

The arguments and return type of such an expression directly match those of the single abstract method.

For instance, the Runnable interface is a functional interface, so instead of:

```
Thread thread = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello World!");
    }
});
```

We could simply do:

Thread thread = new Thread(() -> System.out.println("Hello World!"));
Functional interfaces are usually annotated with the @FunctionalInterface annotation, which is informative and doesn't affect the semantics.

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

Collection
----------

**Queue vs PriorityQueue**

**Iterator vs ListIterator**

**HashMap vs LinkedHashMap vs ConcurrentHashMap**

**LinkedList vs ArrayList**

**HashSet vs TreeSet**

**Question:** How Is Hashmap Implemented in Java? How Does Its Implementation Use Hashcode and Equals Methods of Objects? What Is the Time Complexity of Putting and Getting an Element from Such Structure?

**Answer:** The HashMap class represents a typical hash map data structure with certain design choices.

The HashMap is backed by a resizable array that has a size of power-of-two. When the element is added to a HashMap, first its hashCode is calculated (an int value). Then a certain number of lower bits of this value are used as an array index. This index directly points to the cell of the array (called a bucket) where this key-value pair should be placed. Accessing an element by its index in an array is a very fast O(1) operation, which is the main feature of a hash map structure.

A hashCode is not unique, however, and even for different hashCodes, we may receive the same array position. This is called a collision. There is more than one way of resolving collisions in the hash map data structures. In Java's HashMap, each bucket actually refers not to a single object, but to a red-black tree of all objects that landed in this bucket (prior to Java 8, this was a linked list).

So when the HashMap has determined the bucket for a key, it has to traverse this tree to put the key-value pair in its place. If a pair with such key already exists in the bucket, it is replaced with a new one.

To retrieve the object by its key, the HashMap again has to calculate the hashCode for the key, find the corresponding bucket, traverse the tree, call equals on keys in the tree and find the matching one.

HashMap has O(1) complexity, or constant-time complexity, of putting and getting the elements. Of course, lots of collisions could degrade the performance to O(log(n)) time complexity in the worst case, when all elements land in a single bucket. This is usually solved by providing a good hash function with a uniform distribution.

When the HashMap internal array is filled (more on that in the next question), it is automatically resized to be twice as large. This operation infers rehashing (rebuilding of internal data structures), which is costly, so you should plan the size of your HashMap beforehand.

**Question:** What Is the Purpose of the Initial Capacity and Load Factor Parameters of a Hashmap? What Are Their Default Values?

**Answer:** The initialCapacity argument of the HashMap constructor affects the size of the internal data structure of the HashMap, but reasoning about the actual size of a map is a bit tricky. The HashMap‘s internal data structure is an array with the power-of-two size. So the initialCapacity argument value is increased to the next power-of-two (for instance, if you set it to 10, the actual size of the internal array will be 16).

The load factor of a HashMap is the ratio of the element count divided by the bucket count (i.e. internal array size). For instance, if a 16-bucket HashMap contains 12 elements, its load factor is 12/16 = 0.75. A high load factor means a lot of collisions, which in turn means that the map should be resized to the next power of two. So the loadFactor argument is a maximum value of the load factor of a map. When the map achieves this load factor, it resizes its internal array to the next power-of-two value.

The initialCapacity is 16 by default, and the loadFactor is 0.75 by default, so you could put 12 elements in a HashMap that was instantiated with the default constructor, and it would not resize. The same goes for the HashSet, which is backed by a HashMap instance internally.

Consequently, it is not trivial to come up with initialCapacity that satisfies your needs. This is why the Guava library has Maps.newHashMapWithExpectedSize() and Sets.newHashSetWithExpectedSize() methods that allow you to build a HashMap or a HashSet that can hold the expected number of elements without resizing.

**Question:** Describe Special Collections for Enums. What Are the Benefits of Their Implementation Compared to Regular Collections?

**Answer:** EnumSet and EnumMap are special implementations of Set and Map interfaces correspondingly. You should always use these implementations when you're dealing with enums because they are very efficient.

An EnumSet is just a bit vector with “ones” in the positions corresponding to ordinal values of enums present in the set. To check if an enum value is in the set, the implementation simply has to check if the corresponding bit in the vector is a “one”, which is a very easy operation. Similarly, an EnumMap is an array accessed with enum's ordinal value as an index. In the case of EnumMap, there is no need to calculate hash codes or resolve collisions.

**Question:** What Is the Difference Between Fail-Fast and Fail-Safe Iterators?

**Answer:** Iterators for different collections are either fail-fast or fail-safe, depending on how they react to concurrent modifications. The concurrent modification is not only a modification of collection from another thread but also modification from the same thread but using another iterator or modifying the collection directly.

Fail-fast iterators (those returned by HashMap, ArrayList, and other non-thread-safe collections) iterate over the collection's internal data structure, and they throw ConcurrentModificationException as soon as they detect a concurrent modification.

Fail-safe iterators (returned by thread-safe collections such as ConcurrentHashMap, CopyOnWriteArrayList) create a copy of the structure they iterate upon. They guarantee safety from concurrent modifications. Their drawbacks include excessive memory consumption and iteration over possibly out-of-date data in case the collection was modified.

**Question:** How Can You Use Comparable and Comparator Interfaces to Sort Collections?

**Answer:** The Comparable interface is an interface for objects that can be compared according to some order. Its single method is compareTo, which operates on two values: the object itself and the argument object of the same type. For instance, Integer, Long, and other numeric types implement this interface. String also implements this interface, and its compareTo method compares strings in lexicographical order.

The Comparable interface allows to sort lists of corresponding objects with the Collections.sort() method and uphold the iteration order in collections that implement SortedSet and SortedMap. If your objects can be sorted using some logic, they should implement the Comparable interface.

The Comparable interface usually is implemented using natural ordering of the elements. For instance, all Integer numbers are ordered from lesser to greater values. But sometimes you may want to implement another kind of ordering, for instance, to sort the numbers in descending order. The Comparator interface can help here.

The class of the objects you want to sort does not need to implement this interface. You simply create an implementing class and define the compare method which receives two objects and decides how to order them. You may then use the instance of this class to override the natural ordering of the Collections.sort() method or SortedSet and SortedMap instances.

As the Comparator interface is a functional interface, you may replace it with a lambda expression, as in the following example. It shows ordering a list using a natural ordering (Integer‘s Comparable interface) and using a custom iterator (Comparator<Integer> interface).

```
List<Integer> list1 = Arrays.asList(5, 2, 3, 4, 1);
Collections.sort(list1);
assertEquals(new Integer(1), list1.get(0));

List<Integer> list1 = Arrays.asList(5, 2, 3, 4, 1);
Collections.sort(list1, (a, b) -> b - a);
assertEquals(new Integer(5), list1.get(0));
```

**Question:** Explain the Difference Between Primitive and Reference Types.

**Answer:** Reference types inherit from the top java.lang.Object class and are themselves inheritable (except final classes). Primitive types do not inherit and cannot be subclassed.

Primitively typed argument values are always passed via the stack, which means they are passed by value, not by reference. This has the following implication: changes made to a primitive argument value inside the method do not propagate to the actual argument value.

Primitive types are usually stored using the underlying hardware value types.

For instance, to store an int value, a 32-bit memory cell can be used. Reference types introduce the overhead of object header which is present in every instance of a reference type.

The size of an object header can be quite significant in relation to a simple numeric value size. This is why the primitive types were introduced in the first place — to save space on object overhead. The downside is that not everything in Java technically is an object — primitive values do not inherit from Object class.

**Question:** Describe the Different Primitive Types and the Amount of Memory They Occupy.

**Answer:** Java has 8 primitive types:

- boolean — logical true/false value. The size of boolean is not defined by the JVM specification and can vary in different implementations.
- byte — signed 8-bit value,
- short — signed 16-bit value,
- char — unsigned 16-bit value,
- int — signed 32-bit value,
- long — signed 64-bit value,
- float — 32-bit single precision floating point value corresponding to the IEEE 754 standard,
- double — 64-bit double precision floating point value corresponding to the IEEE 754 standard.

**Question:** What Is the Difference Between an Abstract Class and an Interface? What Are the Use Cases of One and the Other?

**Answer:** An abstract class is a class with the abstract modifier in its definition. It can't be instantiated, but it can be subclassed. The interface is a type described with interface keyword. It also cannot be instantiated, but it can be implemented.

The main difference between an abstract class and an interface is that a class can implement multiple interfaces, but extend only one abstract class.

An abstract class is usually used as a base type in some class hierarchy, and it signifies the main intention of all classes that inherit from it.

An abstract class could also implement some basic methods needed in all subclasses. For instance, most map collections in JDK inherit from the AbstractMap class which implements many methods used by subclasses (such as the equals method).

An interface specifies some contract that the class agrees to. An implemented interface may signify not only the main intention of the class but also some additional contracts.

For instance, if a class implements the Comparable interface, this means that instances of this class may be compared, whatever the main purpose of this class is.

**Question:** What Are the Restrictions on the Members (Fields and Methods) of an Interface Type?

**Answer:** An interface can declare fields, but they are implicitly declared as public, static and final, even if you don't specify those modifiers. Consequently, you can't explicitly define an interface field as private. In essence, an interface may only have constant fields, not instance fields.

All methods of an interface are also implicitly public. They also can be either (implicitly) abstract, or default.

**Question:** What Is the Difference Between an Inner Class and a Static Nested Class?

**Answer:** Simply put – a nested class is basically a class defined inside another class.

Nested classes fall into two categories with very different properties. An inner class is a class that can't be instantiated without instantiating the enclosing class first, i.e. any instance of an inner class is implicitly bound to some instance of the enclosing class.

Here's an example of an inner class – you can see that it can access the reference to the outer class instance in the form of OuterClass1.this construct:

```
public class OuterClass1 {

    public class InnerClass {

        public OuterClass1 getOuterInstance() {
            return OuterClass1.this;
        }

    }

}
```

To instantiate such inner class, you need to have an instance of an outer class:

```
OuterClass1 outerClass1 = new OuterClass1();
OuterClass1.InnerClass innerClass = outerClass1.new InnerClass();
```

Static nested class is quite different. Syntactically it is just a nested class with the static modifier in its definition.

In practice, it means that this class may be instantiated as any other class, without binding it to any instance of the enclosing class:

```
public class OuterClass2 {

    public static class StaticNestedClass {
    }

}
```

To instantiate such class, you don't need an instance of outer class:

```
OuterClass2.StaticNestedClass staticNestedClass = new OuterClass2.StaticNestedClass();

```

**Question:** Does Java Have Multiple Inheritance?

**Answer:** Java does not support the multiple inheritance for classes, which means that a class can only inherit from a single superclass.

But you can implement multiple interfaces with a single class, and some of the methods of those interfaces may be defined as default and have an implementation. This allows you to have a safer way of mixing different functionality in a single class.

**Question:** What Are the Wrapper Classes? What Is Autoboxing?

**Answer:** For each of the eight primitive types in Java, there is a wrapper class that can be used to wrap a primitive value and use it like an object. Those classes are, correspondingly, Boolean, Byte, Short, Character, Integer, Float, Long, and Double. These wrappers can be useful, for instance, when you need to put a primitive value into a generic collection, which only accepts reference objects.

```
List<Integer> list = new ArrayList<>();
list.add(new Integer(5));
```

To save the trouble of manually converting primitives back and forth, an automatic conversion known as autoboxing/auto unboxing is provided by the Java compiler.

```
List<Integer> list = new ArrayList<>();
list.add(5);
int value = list.get(0);

```

**Question:** Describe the Difference Between equals() and ==

**Answer:** The == operator allows you to compare two objects for “sameness” (i.e. that both variables refer to the same object in memory). It is important to remember that the new keyword always creates a new object which will not pass the == equality with any other object, even if they seem to have the same value:

```
String string1 = new String("Hello");
String string2 = new String("Hello");

assertFalse(string1 == string2);
```

Also, the == operator allows to compare primitive values:

```
int i1 = 5;
int i2 = 5;

assertTrue(i1 == i2);
```

The equals() method is defined in the java.lang.Object class and is, therefore, available for any reference type. By default, it simply checks that the object is the same via the == operator. But it is usually overridden in subclasses to provide the specific semantics of comparison for a class.

For instance, for String class this method checks if the strings contain the same characters:

```
String string1 = new String("Hello");
String string2 = new String("Hello");

assertTrue(string1.equals(string2));
```

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

Threading/Concurrency
---------------------

**Runnable vs Callable**

The Runnable interface has a single run method. It represents a unit of computation that has to be run in a separate thread. The Runnable interface does not allow this method to return value or to throw unchecked exceptions.

The Callable interface has a single call method and represents a task that has a value. That's why the call method returns a value. It can also throw exceptions. Callable is generally used in ExecutorService instances to start an asynchronous task and then call the returned Future instance to get its value.

**Question:** Describe the Different States of a Thread and When Do the State Transitions Occur.

**Answer:** The state of a Thread can be checked using the Thread.getState() method. Different states of a Thread are described in the Thread.State enum. They are:

- NEW — a new Thread instance that was not yet started via Thread.start()
- RUNNABLE — a running thread. It is called runnable because at any given time it could be either running or waiting for the next quantum of time from the thread scheduler. A NEW thread enters the RUNNABLE state when you call Thread.start() on it
- BLOCKED — a running thread becomes blocked if it needs to enter a synchronized section but cannot do that due to another thread holding the monitor of this section
- WAITING — a thread enters this state if it waits for another thread to perform a particular action. For instance, a thread enters this state upon calling the Object.wait() method on a monitor it holds, or the Thread.join() method on another thread
- TIMED_WAITING — same as the above, but a thread enters this state after calling timed versions of Thread.sleep(), Object.wait(), Thread.join() and some other methods
- TERMINATED — a thread has completed the execution of its Runnable.run() method and terminated

**Question:** What Is a Daemon Thread, What Are Its Use Cases? How Can You Create a Daemon Thread?

**Answer:** A daemon thread is a thread that does not prevent JVM from exiting. When all non-daemon threads are terminated, the JVM simply abandons all remaining daemon threads. Daemon threads are usually used to carry out some supportive or service tasks for other threads, but you should take into account that they may be abandoned at any time.

To start a thread as a daemon, you should use the setDaemon() method before calling start():

```
Thread daemon = new Thread(()
  -> System.out.println("Hello from daemon!"));
daemon.setDaemon(true);
daemon.start();
```

Curiously, if you run this as a part of the main() method, the message might not get printed. This could happen if the main() thread would terminate before the daemon would get to the point of printing the message. You generally should not do any I/O in daemon threads, as they won't even be able to execute their finally blocks and close the resources if abandoned.

**Question:** What Is the Thread’s Interrupt Flag? How Can You Set and Check It? How Does It Relate to the Interruptedexception?

**Answer:** The interrupt flag, or interrupt status, is an internal Thread flag that is set when the thread is interrupted. To set it, simply call thread.interrupt() on the thread object.

If a thread is currently inside one of the methods that throw InterruptedException (wait, join, sleep etc.), then this method immediately throws InterruptedException. The thread is free to process this exception according to its own logic.

If a thread is not inside such method and thread.interrupt() is called, nothing special happens. It is thread's responsibility to periodically check the interrupt status using static Thread.interrupted() or instance isInterrupted() method. The difference between these methods is that the static Thread.interrupted() clears the interrupt flag, while isInterrupted() does not.

**Question:** What Are Executor and Executorservice? What Are the Differences Between These Interfaces?

**Answer:** Executor and ExecutorService are two related interfaces of java.util.concurrent framework. Executor is a very simple interface with a single execute method accepting Runnable instances for execution. In most cases, this is the interface that your task-executing code should depend on.

ExecutorService extends the Executor interface with multiple methods for handling and checking the lifecycle of a concurrent task execution service (termination of tasks in case of shutdown) and methods for more complex asynchronous task handling including Futures.

**Question:** What Are the Available Implementations of Executorservice in the Standard Library?

**Answer:** The ExecutorService interface has three standard implementations:

- ThreadPoolExecutor — for executing tasks using a pool of threads. Once a thread is finished executing the task, it goes back into the pool. If all threads in the pool are busy, then the task has to wait for its turn.

- ScheduledThreadPoolExecutor allows to schedule task execution instead of running it immediately when a thread is available. It can also schedule tasks with fixed rate or fixed delay.

- ForkJoinPool is a special ExecutorService for dealing with recursive algorithms tasks. If you use a regular ThreadPoolExecutor for a recursive algorithm, you will quickly find all your threads are busy waiting for the lower levels of recursion to finish. The ForkJoinPool implements the so-called work-stealing algorithm that allows it to use available threads more efficiently.

**Question:** What Is Java Memory Model (Jmm)? Describe Its Purpose and Basic Ideas.

**Answer:** Java Memory Model is a part of Java language specification described in Chapter 17.4. It specifies how multiple threads access common memory in a concurrent Java application, and how data changes by one thread are made visible to other threads. While being quite short and concise, JMM may be hard to grasp without strong mathematical background.

The need for memory model arises from the fact that the way your Java code is accessing data is not how it actually happens on the lower levels. Memory writes and reads may be reordered or optimized by the Java compiler, JIT compiler, and even CPU, as long as the observable result of these reads and writes is the same.

This can lead to counter-intuitive results when your application is scaled to multiple threads because most of these optimizations take into account a single thread of execution (the cross-thread optimizers are still extremely hard to implement). Another huge problem is that the memory in modern systems is multilayered: multiple cores of a processor may keep some non-flushed data in their caches or read/write buffers, which also affects the state of the memory observed from other cores.

To make things worse, the existence of different memory access architectures would break the Java's promise of “write once, run everywhere”. Happily for the programmers, the JMM specifies some guarantees that you may rely upon when designing multithreaded applications. Sticking to these guarantees helps a programmer to write multithreaded code that is stable and portable between various architectures.

The main notions of JMM are:

- Actions, these are inter-thread actions that can be executed by one thread and detected by another thread, like reading or writing variables, locking/unlocking monitors and so on

- Synchronization actions, a certain subset of actions, like reading/writing a volatile variable, or locking/unlocking a monitor

- Program Order (PO), the observable total order of actions inside a single thread

- Synchronization Order (SO), the total order between all synchronization actions — it has to be consistent with Program Order, that is, if two synchronization actions come one before another in PO, they occur in the same order in SO

- synchronizes-with (SW) relation between certain synchronization actions, like unlocking of monitor and locking of the same monitor (in another or the same thread)
Happens-before Order — combines PO with SW (this is called transitive closure in set theory) to create a partial ordering of all actions between threads. If one action

- happens-before another, then the results of the first action are observable by the second action (for instance, write of a variable in one thread and read in another)
Happens-before consistency — a set of actions is HB-consistent if every read observes either the last write to that location in the happens-before order, or some other write via data race
- Execution — a certain set of ordered actions and consistency rules between them

For a given program, we can observe multiple different executions with various outcomes. But if a program is correctly synchronized, then all of its executions appear to be sequentially consistent, meaning you can reason about the multithreaded program as a set of actions occurring in some sequential order. This saves you the trouble of thinking about under-the-hood reorderings, optimizations or data caching.

**Question:** What Is a Volatile Field and What Guarantees Does the Jmm Hold for Such Field?

**Answer:** A volatile field has special properties according to the Java Memory Model (see Q9). The reads and writes of a volatile variable are synchronization actions, meaning that they have a total ordering (all threads will observe a consistent order of these actions). A read of a volatile variable is guaranteed to observe the last write to this variable, according to this order.

If you have a field that is accessed from multiple threads, with at least one thread writing to it, then you should consider making it volatile, or else there is a little guarantee to what a certain thread would read from this field.

Another guarantee for volatile is atomicity of writing and reading 64-bit values (long and double). Without a volatile modifier, a read of such field could observe a value partly written by another thread.

**Question:** Which of the Following Operations Are Atomic?

**Answer:**

- writing to a non-volatile int;
- writing to a volatile int;
- writing to a non-volatile long;
- writing to a volatile long;
- incrementing a volatile long?
A write to an int (32-bit) variable is guaranteed to be atomic, whether it is volatile or not. A long (64-bit) variable could be written in two separate steps, for example, on 32-bit architectures, so by default, there is no atomicity guarantee. However, if you specify the volatile modifier, a long variable is guaranteed to be accessed atomically.

The increment operation is usually done in multiple steps (retrieving a value, changing it and writing back), so it is never guaranteed to be atomic, wether the variable is volatile or not. If you need to implement atomic increment of a value, you should use classes AtomicInteger, AtomicLong etc.

**Question:** What Special Guarantees Does the Jmm Hold for Final Fields of a Class?

**Answer:** JVM basically guarantees that final fields of a class will be initialized before any thread gets hold of the object. Without this guarantee, a reference to an object may be published, i.e. become visible, to another thread before all the fields of this object are initialized, due to reorderings or other optimizations. This could cause racy access to these fields.

This is why, when creating an immutable object, you should always make all its fields final, even if they are not accessible via getter methods.

**Question:** What Is the Meaning of a Synchronized Keyword in the Definition of a Method? of a Static Method? Before a Block?

**Answer:** The synchronized keyword before a block means that any thread entering this block has to acquire the monitor (the object in brackets). If the monitor is already acquired by another thread, the former thread will enter the BLOCKED state and wait until the monitor is released.

```
synchronized(object) {
    // ...
}
```

A synchronized instance method has the same semantics, but the instance itself acts as a monitor.

```
synchronized void instanceMethod() {
    // ...
}
```

For a static synchronized method, the monitor is the Class object representing the declaring class.

```
static synchronized void staticMethod() {
    // ...
}
```

**Question:** If Two Threads Call a Synchronized Method on Different Object Instances Simultaneously, Could One of These Threads Block? What If the Method Is Static?

**Answer:** If the method is an instance method, then the instance acts as a monitor for the method. Two threads calling the method on different instances acquire different monitors, so none of them gets blocked.

If the method is static, then the monitor is the Class object. For both threads, the monitor is the same, so one of them will probably block and wait for another to exit the synchronized method.

**Question:**  What Is the Purpose of the Wait, Notify and Notifyall Methods of the Object Class?

**Answer:** A thread that owns the object's monitor (for instance, a thread that has entered a synchronized section guarded by the object) may call object.wait() to temporarily release the monitor and give other threads a chance to acquire the monitor. This may be done, for instance, to wait for a certain condition.

When another thread that acquired the monitor fulfills the condition, it may call object.notify() or object.notifyAll() and release the monitor. The notify method awakes a single thread in the waiting state, and the notifyAll method awakes all threads that wait for this monitor, and they all compete for re-acquiring the lock.

The following BlockingQueue implementation shows how multiple threads work together via the wait-notify pattern. If we put an element into an empty queue, all threads that were waiting in the take method wake up and try to receive the value. If we put an element into a full queue, the put method waits for the call to the get method. The get method removes an element and notifies the threads waiting in the put method that the queue has an empty place for a new item.

```
public class BlockingQueue<T> {

    private List<T> queue = new LinkedList<T>();

    private int limit = 10;

    public synchronized void put(T item) {
        while (queue.size() == limit) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }
        if (queue.isEmpty()) {
            notifyAll();
        }
        queue.add(item);
    }

    public synchronized T take() throws InterruptedException {
        while (queue.isEmpty()) {
            try {
                wait();
            } catch (InterruptedException e) {}
        }
        if (queue.size() == limit) {
            notifyAll();
        }
        return queue.remove(0);
    }
    
}
```

**Question:** Describe the Conditions of Deadlock, Livelock, and Starvation. Describe the Possible Causes of These Conditions.

**Answer:**

- Deadlock is a condition within a group of threads that cannot make progress because every thread in the group has to acquire some resource that is already acquired by another thread in the group. The most simple case is when two threads need to lock both of two resources to progress, the first resource is already locked by one thread, and the second by another. These threads will never acquire a lock to both resources and thus will never progress.

- Livelock is a case of multiple threads reacting to conditions, or events, generated by themselves. An event occurs in one thread and has to be processed by another thread. During this processing, a new event occurs which has to be processed in the first thread, and so on. Such threads are alive and not blocked, but still, do not make any progress because they overwhelm each other with useless work.

- Starvation is a case of a thread unable to acquire resource because other thread (or threads) occupy it for too long or have higher priority. A thread cannot make progress and thus is unable to fulfill useful work.

**Question:** Describe the Purpose and Use-Cases of the Fork/Join Framework.

**Answer:** The fork/join framework allows parallelizing recursive algorithms. The main problem with parallelizing recursion using something like ThreadPoolExecutor is that you may quickly run out of threads because each recursive step would require its own thread, while the threads up the stack would be idle and waiting.

The fork/join framework entry point is the ForkJoinPool class which is an implementation of ExecutorService. It implements the work-stealing algorithm, where idle threads try to “steal” work from busy threads. This allows to spread the calculations between different threads and make progress while using fewer threads than it would require with a usual thread pool.

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**

**Question:**

**Answer:**
Others
------
