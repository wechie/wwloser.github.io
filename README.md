#### 1.集合概览

集合包括两大接口：`Collection` 和 `Map`。

- `Collection`
  - `Set`
    - `HashSet`
    - `TreeSet`
    - `LinkedHashSet`
  - `List`
    - `ArrayList`
    - `LinkedList`
  - `Queue`
    - `PriorityQueue`
    - `Deque`
      - `ArrayDeque`
      - `LinkedList`
- `Map`
  - `TreeMap`
  - `HashMap`
  - `LinkedHashMap`

集合中常用的四个接口：`Comparable, Comparator, Iterable, Iterator`。

##### 1. `Comparable`

```java
interface Comparable<E>{
	public int compareTo(E obj);
}
```

##### 2. `Comparator`

```java
interface Comparator<E>{
	public int compare(E obj1, E obj2);
    boolean equals(Object obj);
}
```

##### 3. `Iterable`

```java
interface Iterable<E>{
    Iterator<E> iterator(){};
}
```

##### 4. `Iterator`

```java
interface Iterator<E>{
    public boolean hasNext(){};
    public E next(){};
    public void remove(){};
}
```

`remove()` 方法可以删除 `next()` 方法返回的元素，但是不可以连续使用 `remove()` 方法。因为 `Iterator` 对象中有一个成员变量`current`保存 `next()` 方法的返回值，当调用 `remove()` 删除元素后，`current` 会被置为 `null`。所以不能连续调用 `remove()`。

当使用 `Iterator` 迭代器访问 `Collection` 集合元素时，`Collection` 中的元素不能被改变，只能通过 `Iterator` 的 `remove` 方法删除上一次 `next` 方法返回的集合元素才可以；否则将会引发异常。

`Iterator` 采用了快速失败（`fast-fail`）机制，一旦在迭代过程中检测到该集合已经被修改（增加或删除元素），程序立即引发 `ConcurrentModifactionException `异常，而不是显示修改后的结果，这样可以避免共享资源而引发的潜在问题。

`Collection` 接口中定义的方法：也是 `Set` 的方法

```java
public interface Collection<E> extends Iterable<E> {
    int size();
    boolean isEmpty();
    boolean contains(Object o);
    Iterator<E> iterator();
    Object[] toArray();
    <T> T[] toArray(T[] a);
    boolean add(E e);	//当对象存在时，不添加，返回false
    boolean remove(Object o);
    boolean containsAll(Collection<?> c);
    boolean addAll(Collection<? extends E> c);
    boolean removeAll(Collection<?> c);
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }
    boolean retainAll(Collection<?> c);
    void clear();
    boolean equals(Object o);
    int hashCode();  
}
```

`AbstractCollection`抽象类，它为除 `size()`和`iterator()`之外的其他方法提供了默认实现。

#### `Set`

方法见 `Collection` 方法

##### `HashSet` 

`HashSet` 中的元素可以是 `null` 。

`HashSet` 集合插入和删除元素的过程是：

- 先调用元素的 `hashCode()`，计算出元素所在的槽号。
- 然后在对应槽与链表中的每个结点使用 `equal()` 方法进行比较，判断是否相等。由于无须判断大小关系。所以不会使用到 `Comparable/Comparator` 接口。
- 此外，`HashCode` 中的 `contains(Object),remove(Object)` 和 `Iterator` 中的 `remove()` 方法都是按照上述方法再对应槽中进行处理。不会遍历整个集合。

##### `TreeSet`

`TreeSet`可以确保集合元素处于有序状态。与 `HashSet`集合相比，`TreeSet`还提供了如下几个额外的方法：

```java
Comparator comparator() ：如果 TreeSet采用了定制排序，则该方法返回定制排序所用的 Comparator；如果采用自然排序，则返回 null。
Object first()：返回集合中的第一个元素。
Object last()：返回集合中的最后一个元素。
Object lower(Object e)：返回集合中小于指定元素的最大元素，参考元素不需要是 TreeSet集合里的元素。
Object higher(Object e)：返回集合中大于指定元素的最小元素，参考元素不需要是TreeSet集合里的元素。
Sorted subSet(Object fromElement, Object toElement)：返回此 TreeSet 的子集合，范围从 fromElement（包含）到toElement（不包含）。
Sorted headSet(Object toElement)：返回此TreeSet中小于toElement 的元素组成的子集。
Sorted tailSet(Object fromElement)：返回此TreeSet中由大于或等于 fromElement 的元素组成的子集。
```

`TreeSet` 采用红黑树的数据结构来存储集合元素。在对集合进行遍历时，每个值将按照排序后的顺序呈现。`TreeSet` 支持两种排序方法：自然排序和定制排序。默认情况下，`TreeSet` 采用自然排序。如果需要定制排序，需要在创建 `TreeSet` 对象时，添加 `Comparator` 对象作为参数。

`TreeSet` 使用 `Comparable/Comparator` 来比较元素的大小，以及判断元素是否相等。`TreeSet` 中元素不可以为 `null`，因为 `null` 无法比较大小。

#### `List`

List 集合代表一个元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。List 集合默认按元素添加顺序设置元素的索引。因此List集合里增加了一些根据索引来操作集合元素的方法。

```java
void add(int index, Object element)：将元素 element 插入 List 集合的 index 处。
boolean addAll(int index, Collection c)：将集合 c 所包含的所有元素都插入到 List 集合的 index 处。
Object get(int index)：返回集合 index 索引处的元素。
int indexOf(Object o)：返回对象在 List 集合中第一次出现的位置索引。
int lastIndexOf(Object o)：返回对象在List 集合中最后一次出现的位置索引。
Object remove(int index)：删除并返回 index 索引处的元素。
Object set(int index , Object element)：将 index 处的元素替换成 element对象，返回被替换的就元素。
List subList(int fromIndex,int toIndex)：返回从索引 fromIndex（包含）到索引toIndex（不包含）处所有集合元素组成的子集合。
void sort(Comparator c)：根据 Comparator 对 List 集合的元素排序。
```

`List` 不考虑元素的大小关系，所以通过元素的 `equals` 方法判断元素是否相等，仅在查找元素时使用。

##### `ListIterator ` 接口

List 额外提供了一个 `listIterator()` 方法，该方法返回一个 `ListIterator` 对象，`ListIterator` 接口是 `Iterator `的子接口，提供了专门操作 `List `的方法。`ListIterator`接口在`Iterator `接口基础上增加了如下方法：

```java
boolean hasPrevious()：返回该迭代器关联的集合是否还有上一个元素。
Object previous()：返回该迭代器的上一个元素。
void add(Object o)
void set(Object o)：用一个新元素取代调用 next() 或previous() 返回的上一个元素。
int nextIndex()：返回next域所指元素的索引
int previousIndex()：返回下一次调用previous 方法时返回元素的索引，即next域索引- 1
```

`iteraotr` 中有一个成员 `cur` 下标，记录遍历的当前位置，

- 调用 `next()`，会返回 `cur` 位置处的元素，并将 `cur = cur + 1 or cur = cur.next`。
- 调用 `prevoius()`，会返回 `cur - 1 or cur.previous` 处的元素，并将 `cur = cur - 1 or cur = cur.previous`。
- 调用 `add()`，对于 `arrayList` 而言，会将元素插入 `cur` 位置处，后面的元素后移一位，且 `cur = cur + 1`。对于 `LinkedList` 而言，会将元素插入 `cur.previous` 处。即会插在 `next()` 元素的前面， `previous()` 元素的后面。

#### `Queue`

新元素插入(offer)到队列的尾部，访问元素(poll)会返回队列头部的元素。通常，队列不允许随机访问队列中的元素。`Queue `接口中定义了如下几个方法：

```java
void add(Object e)：将指定元素加入此队列的尾部。如果队列满，则抛出异常。
Object remove()：获取队列头部的元素，并删除该元素。如果队列为空，则抛出异常。
Object element()：获取队列头部的元素，但是不删除该元素。如果队列为空，则抛出异常。
boolean offer(Object e)：将指定元素加入此队列的尾部，如果队列满，返回false。
Object poll()：获取队列头部的元素，并删除该元素，如果队列为空，则返回null。
Object peek()：获取队列头部的元素，但是不删除该元素。如果队列为空，则返回null。
```

`PriorityQueue ` 内部数据结构是最小堆。`PriorityQueue `不允许插入 null 元素，`PriorityQueue `的元素有两种排序方式：自然排序和定制排序。

#### `Deque`

Deque 接口里定义了一些双端队列的方法，这些方法允许从两端来操作队列的元素。

```java
Object getFirst()：获取但不删除双端队列的第一个元素。
Object getLast()：获取但不删除双端队列的最后一个元素。
Object removeFirst()：获取并删除双端队列的第一个元素。
Object removeLast()：获取并删除双端队列的最后一个元素。
boolean offerFirst(Object e)：将指定元素插入该双端队列的开头。
boolean offerLast(Object e)：将指定元素插入该双端队列的末尾。
void addFirst(Object e)：将指定元素插入该双端队列的开头。
void addLast(Object e)：将指定元素插入该双端队列的尾部。
Object removeFirstOccurrence(Object o)：获取并删除双端队列的第一次出现的元素o。
Object removeLastOccurrence(Object o)：获取并删除双端队列的最后一次出现的元素o。
Object pollFirst()：获取并删除双端队列头部的元素。如果队列为空，则返回null；
Object pollLast()：获取并删除双端队列尾部的元素。如果队列为空，则返回null；
Object peekFirst()：获取队列头部的元素，但是不删除该元素。如果队列为空，则返回null；
Object peekLast()：获取队列尾部的元素，但是不删除该元素。如果队列为空，则返回null；
Object pop()（栈方法）：pop出该双端队列所表示的栈的栈顶元素。相当于 removeFirst()。
void push(Object e)（栈方法）：将一个元素 push 进该双端队列所表示的栈的栈顶。相当于 addFirst(e)；
Iterator descendingIterator()：返回该双端队列对应的迭代器，该迭代器将以逆向顺序来迭代队列中的元素。
```

实现`Deque` 接口的类都可以用来作为栈使用，但是实现栈的栈顶是在队列的**头部**。

##### `ArrayDeque` 实现类

`Deque`  接口提供了一个典型的实现类： `ArrayDeque`，它是一个基于数组实现的双端队列，创建 `Deque`  时同样可指定一个 `numElements`参数，该参数用于指定 `Object[]` 数组的长度；如果不指定` numElements` 参数，`Deque`  底层数组的长度为16；

##### `LinkdeList `实现类

`LinkdeList `类是 List 接口的实现类 --- 这意味着它是一个 List 集合。除此之外，`LinkdeList `还实现了`Deque`接口，可以被当成双端队列来使用，因此既可以作为栈来使用，也可以作为队列使用。

### `Map`

如果把 Map 里的所有 key放在一起，他们组成了一个keySet集合（所有的 key没有顺序，key与 key 之间不能重复）。

如果把 Map 里的所有 value 放在一起来看，他们非常类似于一个 List：元素可以重复，每个元素可以根据索引来查找，只是 Map 中的索引不再是下标，而是对应的key。

Map 接口中定义的常用方法有：

```java
int size()：返回该Map里的键值对的个数。
void clear()：删除该 Map 对象中的所有 key-value对。
boolean isEmpty()：查询该Map 是否为空，如果为空，则返回true；
boolean containsKey(Object key)：查询 Map 中是否包含指定的 key，如果包含则返回true。
boolean containsValue(Object value)：查询 Map 中是否包含一个或多个 value,如果包含则返回 true。
V get(Object key)：返回指定key 所对应的 value；如果此 Map 中不包含该 key，则返回null。
default V getOrDefault(Object key, V defaultValue)：获取与key对应的value，若key不存在，则返回defaultValue
V put(K key,V value)：添加一个键值对，如果当前 Map 中已有一个相同的key，则用新的键值对覆盖原来的键值对，并返回原来的value。
void putAll(Map<？ extends K,? extends V> m)：将指定Map中的所有键值对复制到本Map中。
Object remove(Object key)：删除指定key对应的键值对，返回被删除key关键的value，如果该key不存在，则返回null；
Set<K> keySet()：返回该Map 中所有 key 组成的Set视图。可以从集中删除元素，Map中对应的键值对会被删除，但不能添加元素
Collection<V> values()：返回该Map里所有value组成的Collection视图。可以从集合中删除元素（只会删除一个键值对），所删除的值及对应的键将从Map中删除，不能增加元素。
Set<Map.Entry<K,V>> entrySet()：返回 Map中所有键值对组成的Set视图，每个集合元素都是 Map.Entry（Entry 是 Map 的内部类）对象。可以从集中删除元素，它们将从映射中删除元素，但不能增加任何元素。
```

Map 接口中包括一个内部接口 Entry，该接口封装了一个键值对。Entry 包含如下三个方法：

```java
Object getKey()：返回该Entry里包含的value值。
Object getVlaue()：返回该Entry里包含的value值。
Object setValue(V value)：设置该Entry里包含的value值，并返回新设置的alue值
```

为了成功地在 `HashMap` 中存储元素，用作key 的对象必须实现 `hashCode()` 方法 和 equals() 方法。

判断value相等的方法是：使用value的equals()方法返回值来判断。

综上可以，不考虑元素大小序的集合(`HashSet,ArrayList,LinkedList,ArrayDeque,HashMap`)都是通过 `equals()` 判断元素是否相等，且允许元素为 `null`。考虑元素大小关系的集合(`TreeSet`，`PriorityQueue`，`TreeMap`) 都是通过 `compareTo/compare()` 来判断相等。

##### `Map` 视图(`view`)

`Map` 有三种视图：`keySet、values、entrySet`。需要说明的是：`keySet` 方法返回了一个实现类 `Set<K>` 接口的类对象，而不是 `HashSet`  或 `TreeSet`，这个类的方法对原 `map`进行操作。这种集合称为**视图**。`values`返回值是 `Collection<V>` 接口的实现对象、`entrySet` 返回的是一个实现 `Set<Map.Entry<K,V>>` 类型的对象。可以通过 `keySet、values、entrySet` 来遍历 `Map` 的键、值、键值对。

如果在键集视图上调用迭代器的 `remove()` 方法，实际上会从映射中删除这个键对应的键值对。不能向键集试图增加元素，如果试图调用 `add()`，会抛出异常。`entrySet` 有同样的限制。

##### 轻量级集合包装器

`Arrays` 的静态方法 `asList(Object ... a)` 将返回包装了普通数组的 `List` 包装器。这个方法可以将数组传递给一个期望得到列表或集合参数的方法。它是数组的视图对象，类型为 `List<>`，带有访问底层数组的 `get` 和 `set` 方法。但是改变数组大小的所有方法（例如，与迭代器相关的 `add` 和 `remove` 方法）都会抛出异常。

##### 操作集合的工具类：`Collections`

`Java` 提供了一个操作 `Set、List` 和 `Map` 等集合的工具类：`Collections`，该工具类里提供了大量方法对集合元素进行排序、查询和修改等操作，还提供了将集合对象设置为不可变、对集合对象实现同步控制等方法。

###### 排序操作

`Collections` 提供了如下常用的类方法用于对 `List` 集合元素进行排序：

```java
void reverse(List list)：反转指定 List集合中元素的排序。
void shuffle(List list)：对List 集合元素进行随机排序。
void sort(List list)：根据元素的自然顺序对指定List集合的元素按升序进行排序。
void sort(List list, Comparator c)：根据指定Comparator 产生的顺序对List集合元素进行排序。
void swap(List list, int i,int j)：将指定List 集合中的 i处元素和 j处元素进行交换。
void rotate(List list, int distance)：当 distance 为正数时，将 list 集合的后 distance个元素“整体”移到前面；当 distance为负数时，将 list集合的前 distance 个元素“整体”移到后面。
```

###### 查找、替换操作

`Collections` 还提供了如下常用的用于查找、替换集合元素的类方法。

```java
int binarySearch(List list, Object key)：使用二分搜索大搜索指定的 List集合，以获得指定对象在 List 集合中的索引。如果要使该方法可以正常工作，必须保证 List 中的元素已经处于有序状态。
Object max(Collection coll)：根源元素的自然顺序，返回给定集合中的最大元素。
Object max(Collection coll ,Comparator comp)：根据Comparator 指定的顺序，返回给定集合中的最大元素。
Object min(Collection coll)：根源元素的自然顺序，返回给定集合中的最小元素。
Object min(Collection coll ,Comparator comp)：根据Comparator 指定的顺序，返回给定集合中的最小元素。
void fill(List list， Object obj)：使用指定元素obj替换指定List集合中的所有元素。
int frequency(Collection c, Object o)：返回指定集合中指定元素的出现次数。
int indexOfSubList(List source, List target)：返回子 List对象在父 List对象中第一次出现的位置索引；如果父List中没有出现这样的子 List，则返回 -1；
int lastIndexOfSubList(List source, List target)：返回子 List对象在父 List对象中最后一次出现的位置索引；如果父List中没有出现这样的子 List，则返回 -1；
boolean replaceAll(List list, Object olbVal, Object newVal)：使用一个新值 newVal 替换 List对象的所有旧值 oldVal。
```

###### 同步控制

`Collections` 类提供了多个 `synchromizedXxx()`方法，该方法可以将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题。例如：

```java
public class Test{
  public static void main(String[] args){
    Collection c = Collections.synchronizedCollection(new ArrayList());
    List list = Collections.synchronizedList(new ArrayList());
    Set s = Collections.synchronizedSet(new ArrayHashSet());
    Map m = Collections.synchronizedMap(new HashMap());    
  }
}
```

在上述程序中，直接将新创建的集合对象传给 `Collections` 的 `synchronizedXxx` 方法，这样就可以 直接获取 `Set、List、Map` 的线程安全实现版本。

| 集合类  | 插入元素的方法                        | 删除元素的方法                         | 访问元素的方法 |
| ------- | ------------------------------------- | -------------------------------------- | -------------- |
| `List`  | `add(Element),add(index,Element)`     | `remove(index)`                        | `get(index)`   |
| `Queue` | `offer(),add() //add添加失败会抛异常` | `poll(),remove() //remove失败会抛异常` | `peek()`       |
| `Set`   | `add(E e)`                            | `remove(Object)`                       |                |
| `Map`   | `put(key,value)`                      | `remove(key)`                          | `get(key)`     |
