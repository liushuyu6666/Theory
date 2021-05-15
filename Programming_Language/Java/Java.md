# Java Data Structure

## build-in data structure

### String

#### length

```java
String s = "aabfc";
s.length();
```

#### substring

```java
s.substring(0, 1) // "a"
```

#### split

```java
List<String> list = Arrays.asList(data.split(","));
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

#### convert to `ArrayList`

```java
Arrays.asList(1, 2, 3);
```

#### 



### LinkedList

| type |            method            | return  | expression |
| :--: | :--------------------------: | :-----: | :--------: |
| add  |         list.add(1)          | boolean |            |
| add  | list.add(int idx, int value) |  void   |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |
|      |                              |         |            |



#### add

```java
list.add(3); // add at the end, return boolean
list.add(int index, int value); // add at a certain position
```



#### get

```java
list.get(int index);
list.pop();

list.getLast();
```

#### delete

#### convert to array

```java
// nested
list.toArray(new int[]ist.size()][2]);
```

### ArrayList

#### new

```java
List<String> l = new ArrayList<>();
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
```

#### length

```java
l.size()
```

#### add

```java
l.add(index, number);
```

#### get

```java
l.get(index);
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

#### slice a `ArrayList`

```java
l.subList(0, 3);
```

#### convert to `set`

```java
Set<Integer> set = new HashSet<>(l);
```

#### sort

```java
Collections.sort(l, Collections.reverseOrder());
```

#### iterator

```java
Iterator<Integer> iter = l.iterator();
while(iter.hasNext()){
    System.out.println(iter.next());
}
```



### HashMap

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

#### traverse

```java
for(Map.Entry<Integer, Integer> entry : map.entrySet()){
    entry.getKey();
    entry.getValue();
}
```



### HashSet

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

#### convert to `List`

```java
List<Integer> list = new ArrayList<>(set1);
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

### random

```java
Random rand = new Random(); //instance of random class
int upperbound = 25;
//generate random values from 0-24
int int_random = rand.nextInt(upperbound); 
```



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