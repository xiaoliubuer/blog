---
layout: post
title:  "Java collection"
date:   2017-09-05 09:40:10 -0400
categories: articles
---

# __1. Interface__

#### __Colleciton__
This enables you to work with groups of objects; it is at the top of the collections hierarchy.

#### __List__
This extends Collection and an instance of List stores an ordered collection of elements. A list is an ordered collection of elements.


```java
// ArrayList 
      List a1 = new ArrayList();
      a1.add("Zara");
      a1.add("Mahnaz");
      a1.add("Ayan");
      System.out.println(" ArrayList Elements");
      System.out.print("\t" + a1);

      // LinkedList
      List l1 = new LinkedList();
      l1.add("Zara");
      l1.add("Mahnaz");
      l1.add("Ayan");
      System.out.println();
      System.out.println(" LinkedList Elements");
      System.out.print("\t" + l1);
```


|List|Add|Remove|Get|Contains|Data Structure|
|:---|:---:|:---:|:---:|:---:|:---:| 
|ArrayList | O(1) |O(n) |O(1) |O(n) |Array|
|LinkedList| O(1)|O(1)|O(n)|O(n)|Linked List|
|CopyonWriteArrayList| O(n)|O(n)|O(1)|O(n)|Array|


#### __Set__
This extends Collection to handle sets, which must contain unique elements. A collection that contains no duplicate elements.

```java
// HashSet
      Set s1 = new HashSet(); 
      s1.add("Zara");
      s1.add("Mahnaz");
      s1.add("Ayan");
      System.out.println();
      System.out.println(" Set Elements");
      System.out.print("\t" + s1);

```

#### __SortedSet__
This extends Set to handle sorted sets.

|Set|Add|Contains|Next|Data Structure|
|:---|:---:|:---:|:---:|:---:|
|__HashSet__|O(1)|O(1)|O(h/n)|Hash Table|
|__LinkedHashSet__|O(1)|O(1)|O(1)|Hash Table + Linked List|
|__EnumSet__|O(1)|O(1)|O(1)|Bit Vector|
|__TreeSet__|O(log n)|O(log n)|O(log n)|Red-black tree|
|__CopyonWriteArraySet__|O(n)|O(n)|O(1)|Array|
|__ConcurrentSkipList__|O(log n)|O(log n)|O(1)|Skip List|




#### __Map__
This maps unique keys to values.

```java
// HashMap
      Map m1 = new HashMap(); 
      m1.put("Zara", "8");
      m1.put("Mahnaz", "31");
      m1.put("Ayan", "12");
      m1.put("Daisy", "14");
      System.out.println();
      System.out.println(" Map Elements");
      System.out.print("\t" + m1);

```

#### __Map.Entry__
This describes an element (a key/value pair) in a map. This is an inner class of Map.

#### __SortedMap__
This extends Map so that the keys are maintained in an ascending order.

#### __Enumeration__
This is legacy interface defines the methods by which you can enumerate (obtain one at a time) the elements in a collection of objects. This legacy interface has been superceded by Iterator.

|Map|Get|ContainsKey|Next|Data Structure|
|:---|:---:|:---:|:---:|:---:| 
|HashMap|O(1)|O(1)|O(h / n)|Hash Table|
|LinkedHashMap|O(1)|O(1)|O(1)|Hash Table + Linked List|
|IdentityHashMap|O(1)|O(1)|O(h / n)|Array|
|WeakHashMap|O(1)|O(1)|O(h / n)|Hash Table|
|EnumMap|O(1)|O(1)|O(1)|Array|
|TreeMap|O(log n)|O(log n)|O(log n)|Red-black tree|
|ConcurrentHashMap|O(1)|O(1)|O(h / n)|Hash Tables|
|ConcurrentSkipListMap|O(log n)|O(log n)|O(1)|Skip List|

# __2. Collection Classes__

#### _AbstractCollection_
Implements most of the Collection interface.

#### _AbstractList_
Extends AbstractCollection and implements most of the List interface.

#### _AbstractSequentialList_
Extends AbstractList for use by a collection that uses sequential rather than random access of its elements.


#### __LinkedList__
Implements a linked list by extending AbstractSequentialList.


#### __ArrayList__
Implements a dynamic array by extending AbstractList.

#### _AbstractSet_
Extends AbstractCollection and implements most of the Set interface.

#### __HashSet__
Extends AbstractSet for use with a hash table.

#### __LinkedHashSet__
Extends HashSet to allow insertion-order iterations.

#### __TreeSet__
Implements a set stored in a tree. Extends AbstractSet.

#### _AbstractMap_
Implements most of the Map interface.

#### __HashMap__
Extends AbstractMap to use a hash table.

#### __TreeMap__
Extends AbstractMap to use a tree.

#### __WeakHashMap__
Extends AbstractMap to use a hash table with weak keys.


#### __LinkedHashMap__
Extends HashMap to allow insertion-order iterations.

#### __IdentityHashMap__
Extends AbstractMap and uses reference equality when comparing documents.


|Queue|Offer|Peak|Poll|Size|Data Structure|
|:---|:---:|:---:|:---:|:---:|:---:|
|PriorityQueue|O(log n )|O(1)|O(log n)|O(1)|Priority Heap|
|LinkedList| O(1)|O(1)|O(1)|O(1)|Array|
|ArrayDequeue| O(1)|O(1)|O(1)|O(1)|Linked List|
|ConcurrentLinkedQueue| O(1)|O(1)|O(1)|O(n)|Linked List|
|ArrayBlockingQueue| O(1)|O(1)|O(1)|O(1)|Array|
|PriorirityBlockingQueue|O(log n)|O(1)|O(log n)|O(1)|Priority Heap|
|SynchronousQueue|O(1)|O(1)|O(1)|O(1)|None!|
|DelayQueue|O(log n)|O(1)|O(log n)|O(1)|Priority Heap|
|LinkedBlockingQueue|O(1)|O(1)|O(1)|O(1)|Linked List|


[More Info](http://tutorials.jenkov.com/java-collections/queue.html)

[One More](http://www.javapractices.com/topic/TopicAction.do?Id=9)



