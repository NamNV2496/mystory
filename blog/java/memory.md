# Memory


Unlike C++, Java has automatic memory management, a nice and quiet garbage collector that works in the background to clean up the unused objects and free up some memory.
While this process is automatic in Java, it does not guarantee that all objects has been deleted by Garbage collector because it’s not eligible for garbage collecting even if we’re no longer using them ⇨ This might cause the OutOfMemoryError. 
🡺 So knowing how memory actually works in Java is important, as it gives you the advantage of writing high-performance and optimized applications.


![img.png](blog/java/img/memory1.png)

![img.png](blog/java/img/memory2.png)

## 1. Stack Memory

Responsible for holding references to heap objects and for storing value types (also known in Java as primitive types).

Variables on the stack have a certain visibility, also called scope. Only objects from the active scope are used.
For example: Assuming that we do not have any global scope variables (fields), and only local variables. 

⇨ If the compiler executes a method’s body, it can access only objects from the stack that are within the method’s body and cannot access by other local variables

Once the method completes and returns, the top of the stack pops out. 

## 2. Heap memory

This part of memory stores the actual object in memory. Those are referenced by the variables from the stack.
The new keyword is responsible for ensuring that there is enough free space on heap, creating an object of the StringBuilder type in memory and referring to it via the “builder” reference, which goes on the stack.

**Example of creating an object**

    String example = new String();


Only one heap memory for each running JVM process and it shares memory regardless of how many threads are running.


The heap itself is divided into a 2 parts: Young and Old generation.
Each of them has its own garbage collector which facilitates the process of garbage collection.
The maximum stack and the heap sizes are not predefined — this depends on the running machine.

![img.png](blog/java/img/memory3.png)

### Young generation

Young generation is the memory space where all the new objects are created.
Garbage collection in the Young Generation memory space is called Minor Garbage Collector.
Young Generation is divided into three parts namely Eden Memory space and two Survivor Memory spaces which facilitates for Minor garbage collector at working.
Minor garbage collection checks the survivor objects and move them to the other survivor space. Thus, one of the survivor space is always empty.
Objects that are survived after many cycles of garbage collection, are moved to the Old generation memory space.

### Old generation

Old Generation memory contains the objects that are long lived and survived after many rounds of Minor garbage collection.
Garbage collection in the Old Generation memory space is called Major garbage collector and usually takes much longer than Minor garbage collection.
When Old generation memory space is filled with objects, major garbage collector will be performed.
Number of Major garbage collections less than number of Minor garbage collections, so the time taken by Major also more than Minor.

## 3. Reference Types

The arrows representing the references to the objects from the heap are actually of different types as 4 different types of references: strong, weak, soft, and phantom references.
The difference between the types of references is that the objects on the heap they refer to are eligible for garbage collecting under the different criteria

![img.png](blog/java/img/memory4.png)

- **Strong Reference**: These are the most popular reference types that we all are used to like the example StringBuilder. The object on the heap which is not garbage collected while there is a strong reference pointing to it.

- **Weak Reference**: weak reference is most likely not survive after the next garbage collection process. A weak reference is created as follows:

- **Soft Reference**: are going to be garbage collected only when your application is running low on memory. Java guarantees that all soft referenced objects are cleaned up before it throws an OutOfMemoryError. A soft reference is created as follows:


- **Phantom reference**: Used to schedule post-mortem cleanup actions, since we know for sure that objects are no longer alive. Used onlysur with a reference queue, since the .get() method of such references will always return null. These types of references are considered preferable to finalizers.


## 4. Garbage Collection

Depending on the type of reference that a variable from the stack holds to an object from the heap, at a certain point in time, that object becomes eligible for the garbage collector.
This process is triggered automatically by Java, and it is up to Java when and whether or not to start this process. We could explicitly call System.gc() to garbage objects but it’s not advised.
It is actually an expensive process. When the garbage collector runs, all threads in your application are paused.

![img.png](blog/java/img/memory5.png)

![img.png](blog/java/img/memory6.png)

The Metaspace which is changed from PERM since Java 8, is used to store the meta data about your loaded classes in the JVM. 
From those Garbage Collection working process, the leak memory problem usually happeneded in old generation when major garbage collector can not reduce objects. And it would take the form of java.lang.OutOfMemoryError: Permgen space.


## 5. String In Memory

JVM creates a String object with the given value in a String constant pool.
And whenever we try to create another String as the code below. JVM verifies whether any String object with the same value exists in the String constant pool, if so, instead of creating a new object JVM assigns the reference of existing object to the new variable.

![img.png](blog/java/img/memory7.png)


