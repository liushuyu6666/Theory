# Java

## base data structure and build-in method

### String

#### length

```java
String s = "aabfc";
s.length();
```



### `ArrayList`

#### new

```java
List<String> l = new ArrayList<>();
```

#### length

```java
l.size()
```



### `HashMap`

#### new

```java
Map<Integer, Integer> map = new HashMap<>();
```

#### add

```java
map.put(key, value);
```

#### get

```java
map.get(key); // get the value, not the entire entity
map.getOrDefault(key, null); // get the value if the key is existed, or null
Set<Map.Entry> mapEntries = new HashSet<>(map.entrySet());
for(Map.Entry<Integer, Integer> m : map.entrySet()){
    System.out.println(m);
}
```

#### keys and values

```java
Set<Integer> keySet = map.keySet();
List<Integer> intList = new ArrayList<>(map.values());
```

#### contains

```java
map.containsKey(1);
map.containsValue(5);
```



### `HashSet`

#### new

```java
Set<String> s = new HashSet<>();
```



## catalog

https://beginnersbook.com/2013/05/java-access-modifiers/)

## classes

### interface

[detail](https://www.w3schools.com/java/java_interface.asp)

Like .h in C

### modifier

- `private`
- default
- `protected`

- `public`

![modifier](img\java_modifier.png)



### keyword

- static
  - it can be accessed without creating an object of the class, unlike `public`, which can only be accessed by objects; 
  - In the **Java** programming language, the keyword **static** indicates that the particular member belongs to a type itself, rather than to an instance of that type. This means that only one instance of that **static** member is created which is shared across all instances of the class.

## concept

### dependency injection

Dependency injection (DI) is the concept in which objects get other required objects from outside.

DI can be implemented in any programming language. The general concept behind dependency injection is called Inversion of Control.

A Java class has a dependency on another class, if it uses an instance of this class. We call this a *class* dependency. For example, a class which accesses a logger service has a dependency on this service class.

Ideally Java classes should be as independent as possible from other Java classes. This increases the possibility of reusing these classes and to be able to test them independently from other classes.