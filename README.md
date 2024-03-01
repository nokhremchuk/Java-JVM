

# Java Virtual Machine Memory Structure (version 17)

JVM contains two main memory structures: one of them created on JVM start and is shared between all the threads (Heap) and second structure created for every thread and is not shared (Stack). Heap hold classes, interfaces and method area. Sets of stack are used for method invocations, partial results, return values and throwing exceptions.

![JVM%20Memory%20Structure](https://github.com/nokhremchuk/Java-JVM/blob/main/JVM%20Memory%20Structure.png)

In the picture above you can see the schema of these two structures. There is a heap and a set of stacks, a set of program counter registers (as stack created for each thread) and a set of native method stacks (optionally - only if your code has native methods). In the picture L - is a number of classes, M - is a number of methods, N - is a number of threads.

## Stack
It holds local variables and partial results, and plays a part in method invocation and return. It stores frames that are used to store data and partial results, as well as to perform dynamic linking, return values for methods, and dispatch exceptions. A new frame is created each time a method is invoked. Each frame has its own array of local variables, its own operand stack, and a reference to the runtime constant pool of the class of the current method. 
JVM specification permits Java Virtual Machine stacks either to be of a fixed size or to dynamically expand and contract as required by the computation. 
If you need to set fixed size of the stack you should use JVM argument _-Xss size_

**For example:**

_-Xss2g_

_-Xss1024k_

Sets the thread stack size (in bytes). Append the letter k or K to indicate KB, m or M to indicate MB, or g or G to indicate GB.

### Frame

JVM specification contains contradiction according to place of the frame. First it says 
> Because the Java Virtual Machine stack is never manipulated directly except to push and pop frames, frames may be heap allocated.

But then explains

> Frames are allocated from the Java Virtual Machine stack (§2.5.2) of the thread creating the frame.

Lastly, it seems to be true because a new frame is created each time a method is invoked.

Note that a frame created by a thread is local to that thread and cannot be referenced by any other thread.

## Heap

The heap is created on virtual machine start-up. The heap may be of a fixed size or may be expanded as required by the computation and may be contracted if a larger heap becomes unnecessary.

The heap is the run-time data area from which memory for all class instances and arrays is allocated. Heap contains a method area that is shared among all Java Virtual Machine threads. It stores per-class structures such as the run-time constant pool, field and method data, and the code for methods and constructors, including the special methods used in class and interface initialization and in instance initialization.

Configuration of the heap could be done with the following JVM arguments.

Options for **fixed memory** _-Xmx_ or _-XX:MaxHeapSize_ should be used.

**_-Xmx size_**

Specifies the maximum size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, or g or G to indicate gigabytes. The _-Xmx_ option is equivalent to -_XX:MaxHeapSize_ .

**For example:**

_-Xmx2048k_

_-Xmx80m_
<br>
<br>
**_-XX:MaxHeapSize_=_size_**

Sets the maximum size (in byes) of the memory allocation pool. This value must be a multiple of 1024 and greater than 2 MB. Append the letter k or K to indicate kilobytes, m or M to indicate megabytes, or g or G to indicate gigabytes.

**For example:**

_-XX:MaxHeapSize_=_16G_

_-XX:MaxHeapSize_=_80m_

The _-XX:MaxHeapSize_ option is equivalent to _-Xmx_ .

Options for **expanding memory** as required _-XX:MaxHeapFreeRatio_ and _-XX:MinHeapFreeRatio_ .

_-XX:MaxHeapFreeRatio_=_percent_

Sets the maximum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space expands above this value, then the heap is shrunk. By default, this value is set to 70%.

**For example:**

_-XX:MaxHeapFreeRatio_=_10_ 

_-XX:MinHeapFreeRatio_=_5_

Customers trying to keep the heap small should also add the option _-XX:-ShrinkHeapInSteps_ .

_-XX:MinHeapFreeRatio_ = _percent_

Sets the minimum allowed percentage of free heap space (0 to 100) after a GC event. If free heap space falls below this value, then the heap is expanded. By default, this value is set to 40%.


## Resources:

[The Java® Virtual Machine Specification] (https://docs.oracle.com/javase/specs/jvms/se17/html/index.html)

[The java Command](https://docs.oracle.com/en/java/javase/17/docs/specs/man/java.html)

## Tools:

[draw.io] (https://drawio.com)
