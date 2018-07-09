# Collections

A *collection* - sometimes called a container - is simply an object that groups multiple elements into a single unit. Collections are used to store, retrieve, manipulate, and communicate aggregate data.

- [Interfaces](#interfaces)
  - [The Collection Interface](#the-collection-interface)
  - [The Set Interface](#the-set-interface)
  - [The List Interface](#the-list-interface)

## Interfaces

![The core collection interfaces](colls-coreInterfaces.gif)

### The Collection Interface

A `Collection` represents a group of objects known as its elements. The `Collection` interface is used to pas around collections of objects where maximum generality is desired.

#### Traversing Collections

##### Aggregate Operations (JDK 8)

```java
myShapeCollection.stream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));
```

```java
myShapeCollection.parallelStream()
.filter(e -> e.getColor() == Color.RED)
.forEach(e -> System.out.println(e.getName()));
```



##### for-each Construct

```java
for (Object o : collection)
    System.out.println(o);
```

##### Iterators

Use Iterator instead of the for-each construct when you need to:

- Remove the current element. The `for-each` construct hides the iterator, so you cannot call remove. Therefore, the `for-each` construct is not usable for filtering.
- Iterate over multiple collections in parallel.

```java
static void filter(Collection<?> c) {
    for (Iterator<?> it = c.iterator; it.hasNext();)
        if (!cond(it.next()))
            it.remove();
}
```

#### Collection Interface Bulk Operations

- `containsAll` - returns `true` if the target `Collection` contains all of the elements in the specified `Collection`.
- `addAll` - adds all of the elements in the specified `Collection` to the target `Collection`.
- `removeAll` - removes from the target `Collection` all of its elements that are also contained in the specified `Collection`.
- `retainAll` - remove from the target `Collection` all its elements that are *not* also contained in the specified `Collection`. That is, it retains only those elements in the target `Collection` that are also contained in the specified `Collection`.
- `clear` - removes all elements from the `Collection`.

```java
// Remove all instances of a specified element, e, from a Collection, c.
c.removeAll(Collections.singleton(e));

// Remove all of the null elements from a Collection, c.
c.removeAll(Collections.singleton(null));
```

#### Collection Interface Array Operations

```java
Object[] a1 = c.toArray();
String[] a2 = c.toArray(new String[0]);
```

### The Set Interface

A `Set` is a `Collection` that cannot contain duplicate elements.

```java
// Collection c
Collection<Type> noDups = new HashSet<Type>(c);

// using JDK 8
c.stream().collect(Collectors.toSet());

Set<String> set = people.stream()
        .map(Person::getName)
        .collect(Collectors.toCollection(TreeSet::new));

Collection<Type> noDups = new LinkedHashSet<Type>(c);

public static <E> Set<E> removeDups(Collection<E> c) {
    return new LinkedHashSet<E>(c);
}
```

#### Set Interface Basic Operations

```java
// print out all distinct words in the argument list.
import java.util.*;
import java.util.stream.*;

public class FindDups {
    public static void main(String[] args) {
        // using JDK 8
        Set<String> distinctWords = Arrays.asList(args).stream()
                .collect(Collectors.toSet());
        System.out.println(distinctWords.size() + " distinct words: " + distinctWords);
        
        Set<String> s = new HashSet<String>();
        for (String a : args)
            s.add(a);
        System.out.println(s.size() + " distinct words: " + s);
    }
}
```

#### Set Interface Bulk Operations

- `s1.containsAll(s2)` - returns `true` if `s2` is a **subset** of `s1`. (`s2` is a subset of `s1` if set `s1` contains all of the elements in `s2`.)
- `s1.addAll(s2)` - transforms `s1` into the **union** of `s1` and `s2`. (The union of two sets is the set containing all of the elements contained in either set.)
- `s1.retainAll(s2)` - transforms `s1` into the intersection of `s1` and `s2`. (The intersection of two sets is the set containing only the elements common to both sets.)
- `s1.removeAll(s2)` - transforms `s1` into the (asymmetric) set difference of `s1` and `s2`. (For example, the set difference of `s1` minus `s2` is the set containing all of the elements found in `s1` but not in `s2`.)

```java
// Calculate the union of two sets nondestructively.
Set<Type> union = new HashSet<Type>(s1);
union.addAll(s2);

// Calculate the intersection of two sets nondestructively.
Set<Type> intersection = new HashSet<Type>(s1);
intersection.retainAll(s2);

// Calculate the difference of two sets nondestructively.
Set<Type> difference = new HashSet<Type>(s1);
difference.removeAll(s2);
```

```java
import java.util.*;
public class FindDups2 {
    public static void main(String[] args) {
        Set<String> uniques = new HashSet<String>();
        Set<String> dups = new HashSet<String>();

        for (String a : args)
            if (!uniques.add(a))
                dups.add(a);

        // Destructive set-difference
        uniques.removeAll(dups);

        System.out.println("Unique words: " + uniques);
        System.out.println("Duplicate words: " + dups);
    }
}
```

```java
// Symmetric set difference
Set<Type> symmetricDiff = new HashSet<Type>(s1);
symmetricDiff.addAll(s2);
Set<Type> tmp = new HashSet<Type>(s1);
tmp.retainAll(s2);
symmetricDiff.removeAll(tmp);
```

### The List Interface

A `List` is an ordered `Collection` (sometimes called a *sequence*). Lists may contain duplicate elements. In addition to the operations inherited from `Collection`, the `List` interface includes operations for the following:

- `Positional access` - manipulates elements based on their numerical position in the list. This includes methods such as `get`, `set`, `add`, `addAll`, and `remove`.
- `Search` - searches for a specified object in the list and returns its numerical position. Search methods include `indexOf` and `lastIndexOf`.
- `Iteration` - extends `Iterator` semantics to take advantage of the list's sequential nature. The `listIterator` methods provide this behavior.
- `Range-view` - The `sublist` method performs arbitrary *range* operations on the list.

#### Collection Operations

```java
list1.addAll(list2);

// nondestructive
List<Type> list3 = new ArrayList<Type>(list1);
list3.addAll(list2);

// JDK 8
List<String> list = people.stream()
        .map(Person::getName)
        .collect(Collections.toList());
```

#### Positional Access and Search Operations

```java
import java.util.*; // for Random

// Swap two indexed values in a List.
public static <E> void swap(List<E> a, int i, int j) {
    E tmp = a.get(i);
    a.set(i, a.get(j));
    a.set(j, tmp);
}

// Polymorphic algorithm that uses swap()
public static void shuffle(List<?> list, Random rnd) {
    for (int i = list.size(); i > 1; i--)
        swap(list, i - 1, rnd.nextInt(i));
}

// Program using shuffle() to print the words in its argument list in random order
public class Shuffle {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        for (String a : args)
            list.add(a);
        Collections.shuffle(list, new Random());
        System.out.println(list);
    }
}
```

```java
// A shorter implementation
import java.util.*;
public class Shuffle {
    public static void main(String[] args) {
        List<String> list = Arrays.asList(args);
        Collections.shuffle(list);
        System.out.println(list);
    }
}
```



Collection 集合（接口）
   |-- List 列表，允许有相同元素，有整数索引
   |-- Set  集，不允许有相同元素，没有索引

List 
  |-- ArrayList：动态数组，地址是连续的，随机访问速度非常快，
		添加、删除元素可能会导致数组元素的集体复制，效率低下
  |-- LinkedList：双向循环链表，地址不连续，靠节点不断寻址相连
		随机访问速度比较慢，
		但是添加和删除元素只要修改相邻节点的指向就可以，效率很高
		尤其是针对于头尾的添加和删除操作
  |-- Vector（课后自己去看）




Collections 工具类
 有一组静态方法专门用来操作Collection对象的
 reverse
 swap
 shuffle
 
 static <T extends Comparable<? super T>> void sort(List<T> list) : 按照自然顺序排
 static <T> void sort(List<T> list, Comparator<? super T> c)  ：按照比较器顺序排
 

 
4444444444444444444444444444444444444444444444
 
Collection
 |-- List : 列表、序列：有序、元素可以重复，每个元素都有一个整数索引
 |-- Set  : 集：无序、元素不能重复
     |-- HashSet: 基于hash算法的set，性能非常高
     |-- TreeSet：基于树的set，具有自然排序的功能

 - 有序指的是：遍历的顺序和输入的顺序一致


 hashSet 如何判断 2个对象是否是同一个对象的呢？

 先比较hashCode值
 1. 相等
	再比较equals
		1. 相等   -> 是一个对象
		2. 不相等 -> 不是一个对象
 2. 不相等 -> 不是一个对象

 为什么重写equals方法一定要重写hashCode方法呢？
 因为基于hash算法的集合，在保存对象时，会先计算hashCode值
 如果hashCode的值不相同，那直接就会认为不是同一个对象，而不会顾及equals的结果。
 如果hashCode的值相同，还会再比较一次equals方法，根据equals方法的结果来判断两个对象是否是同一个对象

  hashCode的规则：
  1. 同一个对象多次调用，结果必须一样
  2. equals认为相同的对象，hashCode值必须相同
  3. equals认为不相同的对象，hashCode值最好不相同；
  如果尽可能的为不同的对象生成不同的hashCode结果，会提高基于hash的集合的性能


## TreeSet是如何判断两个对象是否是同一个对象的呢？ 
 根据compareTo方法是否返回0



Map：字典集
 以键值对的方式进行存储的集合
 |-- HashMap：基于哈希，最常用的map，本质是对于hash值得存储，具有非常高的访问速度
 |-- TreeMap：基于树，能够按照键的自然顺序排序


map的的遍历

 1. 如果只需要key，或者只需要value
 可以调用 keySet() 或者 values() 获取key或value的集合，
 再用foreach循环遍历

 2. 既需要key，又需要value
 调用entrySet()，取出键值对集，再用foreach循环遍历

 3. 用迭代器迭代entrySet
 效率和2完全一致，
 好处1：兼容1.5以下版本
 好处2：可以在迭代的同时删除

 4. 遍历keySet，取value
 虽然代码很简洁易懂，但是性能非常垃圾
 开发中不允许使用




【public boolean equals(Object obj)】
Object类中默认的实现方式是  :   return this == obj  。那就是说，只有this 和 obj引用同一个对象，才会返回true。

而我们往往需要用equals来判断 2个对象是否等价，而非验证他们的唯一性。这样我们在实现自己的类时，就要重写equals.

按照约定，equals要满足以下规则。

自反性:  x.equals(x) 一定是true

对null:  x.equals(null) 一定是false

对称性:  x.equals(y)  和  y.equals(x)结果一致

传递性:  a 和 b equals , b 和 c  equals，那么 a 和 c也一定equals。

一致性:  在某个运行时期间，2个对象的状态的改变不会不影响equals的决策结果，
         那么，在这个运行时期间，无论调用多少次equals，都返回相同的结果。

【public int hashCode()】
这个方法返回对象的散列码，返回值是int类型的散列码。
对象的散列码是为了更好的支持基于哈希机制的Java集合类，例如 Hashtable, HashMap, HashSet 等。

关于hashCode方法，一致的约定是：
1. 重写了equals方法的对象必须同时重写hashCode()方法。

2. 如果2个对象通过equals调用后返回是true，那么这个2个对象的hashCode方法也必须返回同样的int型散列码

3. 如果2个对象通过equals返回false，他们的hashCode返回的值允许相同。
(然而，程序员必须意识到，hashCode返回独一无二的散列码，会让存储这个对象的hashtables更好地工作。)


【使用情况总结】
1、equals方法用于比较对象的内容是否相等（覆盖以后）

2、hashcode方法只有在集合中用到

3、当覆盖了equals方法时，比较对象是否相等将通过覆盖后的equals方法进行比较（判断对象的内容是否相等）。

4、将对象放入到集合(Set)中时，首先判断要放入对象的hashcode值与集合中的任意一个元素的hashcode值是否相等，
 如果不相等直接将该对象放入集合中。
 如果hashcode值相等，然后再通过equals方法判断要放入对象与集合中的任意一个对象是否相等，
 如果equals判断不相等，直接将该元素放入到集合中，否则不放入。

