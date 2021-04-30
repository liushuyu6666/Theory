# Java Data Structure

## build-in data structure

### String

#### length

```java
String s = "aabfc";
s.length();
```

### Array

#### new

```java
int[] array = new int[size];
```

#### sort

```java
Arrays.sort(array); // sort in-place
```

#### fill

```java
Arrays.fill(array, 0); // fill or element to 0
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

#### list to array

```java
int[] array = new int[l.size()];
int i = 0;
for(int lnum:l){
    array[i] = lnum;
}
```

#### shuffle

```java
Collections.shuffle(l);
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
Set<Integer> set1 = new HashSet<>();
```

#### contains

```java
set1.contains(1); // true or false
```

#### array to set

```java
int[] array = new int[]{1, 2, 10, 10};
Integer[] arrayInteger = new Integer[array.length];
for(int i = 0; i < array.length; i++){
    arrayInteger[i] = array[i];
}
set1.addAll(Arrays.asList(arrayInteger));
```

#### add

```java
Set<Integer> set2 = new HashSet<>();
for(int i = 0; i < 5; i++){
    set2.add(i);
}
```

#### union

```java
Set<Integer> union = new HashSet<>(set1);
union.addAll(set2);
```

#### intersection

```java
Set<Integer> intersection = new HashSet<>(set1);
intersection.retainAll(set2);
```

### `Stack`

#### iterator

```java
Iterator iter = stack.iterator();
int curr;
while(iter.hasNext()){
    curr = (int)iter.next();
}
```



### `Queue`

#### new

```java
Queue<Integer> q = new LinkedList<>();
```

#### poll

```java
q.poll()
```

- first in first out
- if q is empty, return null;

## nested data structure

### Array

#### new

```java
Set<Character>[] setArray = new HashSet[9]; // now you have 9 Set pointers points to null
for(int i = 0; i < 9; i++){
    setArray[i] = new HashSet<Character>();
}
```



# Java Theoretical Knowledge

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