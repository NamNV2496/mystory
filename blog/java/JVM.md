# JVM

JVM rất quan trọng vì nó là cách 1 chương trình hoạt động

JVM (Java Virtual Machine) is an abstract machine that one of the thing which enables your computer to run a Java program

A Virtual Machine is a software implementation of a physical machine. Java was developed with the concept of WORA (Write Once Run Anywhere), which runs on a VM.
The compiler compiles the Java .java file into a Java .class file, then that .class file is input into the JVM, which loads and executes the class file.


![img.png](blog/java/img/jvm.png)


## **Wore in java**

Once source code (.java file) is compiled on one platform (bytecode is formed .class file), that bytecode can be executed (interpreted) on any other platform running a JVM 

![img.png](blog/java/img/jvm1.png)

## **JDK**

- JDK contains tools required to write Java programs, and JRE to execute them.
- JRE contains class libraries, JVM, and other supporting files. It does not contain any tool for Java development like a debugger, compiler, etc.
- JVM provides a platform-independent way of executing Java source code.

![img.png](blog/java/img/jvm2.png)

## **JVM structure**

![img.png](blog/java/img/jvm3.png)


## **1. Class Loader Subsystem**

Java 's dynamic class loading functionality is handled by the ClassLoader subsystem. It `loads`, `links`. and `initializes the class file` when it refers to a class for the first time at runtime, not compile time

![img.png](blog/java/img/jvm4.png)

### Step 1: Loading

Classes will be loaded by this component. BootStrap ClassLoader, Extension ClassLoader, and Application ClassLoader are the three ClassLoaders that will help in achieving it.

**Boostrap Classloader**: Responsible for loading classes from the bootstrap classpath, nothing but runtime.jar (all the standard lib like java.lang.*,…). Highest priority will be given to this loader.

**Extension ClassLoader**:  Responsible for loading extension classes which are included inside the ext folder (jre\lib) like OJDBC, MQ lib,… 

**Application ClassLoader**: Responsible for loading local application level classpath, path mentioned Environment Variable, etc.

🡺 The above ClassLoaders will follow Delegation Hierarchy Algorithm while loading the class files. 

![img.png](blog/java/img/jvm5.png)

The order of its hierachy is Application ClassLoader 🡪 Extension ClassLoader 🡪 Bootstrap ClassLoader. The Bootstrap ClassLoader is always given the higher priority, next is Extension ClassLoader and then Application ClassLoader.


### Step 2: Linking

**Verify**: Once the class files are loaded to the memory, there is a verify phase where the bytecode class file are verified if the class files conform to standards. If verification fails we will get the verification error.

**Prepare**: For all static variables memory will be allocated and assigned with default values.

**Resolve**: All symbolic memory references are replaced with the original references.

### Step 3: Initialization

All static variables are asigned with the original values, and also executes the static block at this point. 


## **2. Run-time Data Area**

JVM need runtime data area to store the class files and execute them. The Runtime Data Area is divided into five major components:

![img.png](blog/java/img/jvm6.png)

### Area 1: Method and Heap memory

- **Method Area**: All the class-level data (kind of global varibles in class) will be stored here, including static variables. There is only one method area per JVM, and it is a shared resource.
- **Heap Area**: All the Objects and their corresponding instance variables will be stored here. There is also one Heap Area per JVM. Since the Method and Heap areas share memory for multiple threads, the data stored is not thread-safe.

### Area 2: Stack memory

For each thread, a separate runtime stack will be created. For every method call, one entry will be made in the stack memory which is called Stack Frame.

All local variables will be created in the stack memory. The stack area is thread-safe since it is not a shared resource. The Stack Frame is divided into three subentities:

- **Local Variable Array**: Related to the method how many local variables are involved and the corresponding values will be stored here.
- **Operand stack**: If any intermediate operation is required to perform, operand stack acts as runtime workspace to perform the operation.
- **Frame data**: In the case of any exception, the catch block information will be maintained in the frame data.


### Area 3: PC register and native method stacks memory

- **PC Registers**:  Each thread will have separate PC Registers, to hold the address of current executing instruction once the instruction is executed the PC register will be updated with the next instruction.
- **Native Method stacks**:  Native Method Stack holds native method information related to native platforms. For every thread, a separate native method stack will be created.

⇨For example, if we're running the JVM on Windows, it will contain Windows-related information. Likewise, if we're running on Linux, it will have all the Linux-related information we need.
Or a repeat code which compile to the new machine code for better perfomance by JIT


## **3. Execution Engine**

The bytecode, which is assigned to the Runtime Data Area, will be executed by the Execution Engine. The Execution Engine reads the bytecode and executes it piece by piece. It seperated to 4 part: 

![img.png](blog/java/img/jvm7.png)


### Interpreter

The interpreter interprets the bytecode faster but executes slowly. The disadvantage of the interpreter is that when one method is called multiple times, every time a new interpretation is required.

### JIT(Just In Time ) compiler

The JIT Compiler neutralizes the disadvantage of the interpreter. The Execution Engine will be using the help of the interpreter in converting byte code, but when it finds repeated code it uses the JIT compiler, which compiles the entire bytecode and changes it to native code. 
- **Intermediate Code Generator**: Produces intermediate code.
- **Code Optimizer**: Responsible for optimizing the intermediate code generated above
- **Target Code Generator**: Responsible for Generating Native Machine Code.
- **Profiler**: A special component, responsible for finding hotspots, i.e. whether the method is called multiple times or not.

### Working process of JIT

- Methods are not compiled when they are called the first time. The JVM maintains a call count, which is incremented every time the method is called.
- The methods are interpreted by the JVM until the call count exceeds the JIT compilation threshold.
- Very frequently used methods are compiled as soon as the JVM has started, and less frequently used methods are compiled later.
- After a method is compiled by JIT, its call count is reset to zero.
- The call count of a method reaches a JIT recompilation threshold, JIT compiler compiles method a second time.

### Garbage collector and Java Native Interface (JNI)

- **Garbage Collector**: Collects and removes unreferenced objects from heap to reclaim heap space. Garbage Collection can be triggered by calling System.gc(), but the execution is not guaranteed. Garbage collection of the JVM collects the objects that are created.
- **Java Native Interface (JNI)**: JNI will be interacting with the Native Method Libraries and provides the Native Libraries required for the Execution Engine. 

🡺Example: When classes in java library doesn’t support the platform-dependent features needed by the application. To let your java code to access the library already written in any other language by JNI.


### Some note and conclusion of JVM

The most important JVM Components related to performance are:
- Heap
- JIT (Just in time) Compiler
- Garbage collection (compiles bytecode to machine code at runtime and improves the performance of Java applications.)

There are many JVM to which option we can config:
- Increasing and decreasing the heap size for managing object for best performance.
- Selecting different garbage collectors, depending on your requirement.









