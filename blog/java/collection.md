# Collection

## Java Collection Framework
- Java Collections can achieve all the operations that you perform on a data such as searching, sorting, insertion, manipulation, and deletion
- The Collection in Java is a framework that provides an architecture to store and manipulate the group of objects
- A Collection groups together elements and allows them to be accessed and retrieved later
- Note: “Collection” and “Collections” is different!
- Each collection class implements an interface from a hierarchy
- Each class is designed for a specific type of storage
- java.util package contains all class and interface of Collection


![img.png](blog/java/img/collection.png)

## **Lists**
- A list is a collection that maintains the order of its elements
- It can have the duplicate elements

+) ArrayList: 
- Stores a list of items in a dynamically sized array

+) LinkedList:
- Allows speedy insertion and removal of items from the list

## **Sets**

A set is unordered collection of unique elements

+) HashSet: 
- Uses hash tables to speed up finding, adding, and removing elements

+) TreeSet:
- Uses a binary tree to speed up finding, adding, and removing elements

## **Stack and Queues**

Another way of gaining efficiency in a collection is to reduce the number of operations available 

+) Stack: 
-   Remember the order of its elements, but it does not allow you to insert elements in 	every position 
-   You can only add and remove elements at the top

+) Queues:
-  Add items to one end (the tail)
-    Remove them from the other end (the head)
-    Example: Aline of people waiting for a bank teller

## **Maps**

A map stores keys, values, and the associations between them

+) HashMap: 
-  Keys can get null value but cant duplicate, if not it happend override
-  No order


+) TreeMap:
-  Must not null
-  In ascending order


Ref about HashMap: https://www.youtube.com/watch?v=c3RVW3KGIIE

![img.png](blog/java/img/hashMap_principle.png)




