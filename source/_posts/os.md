---
title: Operating System
date: 2022-07-08 15:47:57
tags:
- computer science
categories:
- other study
---
# Introduction

​		一个运行的程序仅仅执行一件简单的事情：**执行指令**。处理器从内存中**取指令（fetch）**，进行**译码（decode）**，并且**执行（execute）**，这一过程每秒发生百万次甚至亿万次。完成这一过程后，处理器继续下一条指令，直到程序彻底完成。

> Many millions (and these days, even billions) of times every second, the processor fetches an instruction from memory, decodes it (i.e., figures out which instruction this is), and executes it (i.e., it does the thing that it is supposed to do, like add two numbers together, access memory, check a condition, jump to a function, and so forth). After it is done with this instruction, the processor moves on to the next instruction, and so on, and so on, until the program finally completes.

---
<!-- more -->

### Virtualizing CPU

因为**虚拟化**允许大量程序运行（共享CPU），同时存取它们自己的指令和数据（共享内存），访问设备（共享硬盘之类），操作系统被认为是**资源管理器**。任意的CPU，内存和设备都是系统中的资源。

``````c
//cpu.c
#include <stdio.h>
#include <stdlib.h>
#include <sys/time.h> 
#include <assert.h>
#include "common.h"

int
main(int argc, char *argv[])
{
	if (argc != 2) {
			fprintf(stderr, "usage: cpu <string>\n");
            exit(1);
	}
	char *str = argv[1]; 
	while (1) {
		Spin(1);
        printf("%s\n", str);
    }
    return 0;
}
``````

*该程序所做的只有call `Spin()`，一个重复检查时间并且当它运行一秒时返回。然后，它将用户输入在命令行中的字符串打印，并且一直重复该过程*

```shell
prompt> gcc -o cpu cpu.c -Wall
prompt> ./cpu "A"
A
A
A
A
ˆC
prompt>
```

```shell
prompt> ./cpu A & ./cpu B & ./cpu C & ./cpu D & 
[1] 7353 
[2] 7354 
[3] 7355 
[4] 7356 
A 
B 
D 
C
A
B 
D 
C 
A 
...
```

* **虚拟化CPU（virtualizing the CPU）**：将单个CPU（或者其中一小部分）转化为看起来数量为无限的CPU，并且使许多程序看似能够同时运行。

* > Turning a single CPU (or a small set of them) into a seemingly infinite number of CPUs and thus allowing many programs to seemingly run at once is what we call **virtualizing the CPU**.

---
### Virtualizing Memory

```c
//mem.c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include "common.h"
int
main(int argc, char *argv[])
{
	int *p = malloc(sizeof(int));
    assert(p != NULL);
	printf("(%d) address pointed to by p: %p\n", getpid(), p);
	*p = 0;
	while (1) { 
		Spin(1);
		*p = *p + 1;
        printf("(%d) p: %d\n", getpid(), *p);
    } 
return 0;
}
```

```shell
prompt> ./mem
(2134) address pointed to by p: 0x200000
(2134) p: 1 
(2134) p: 2 
(2134) p: 3 
(2134) p: 4 
(2134) p: 5
ˆC
```

```shell
prompt> ./mem &; ./mem & 
[1] 24113 
[2] 24114 
(24113) address pointed to by p: 0x200000 
(24114) address pointed to by p: 0x200000 
(24113) p: 1 
(24114) p: 1 
(24114) p: 2 
(24113) p: 2 
(24113) p: 3
(24114) p: 3
(24113) p: 4
(24114) p: 4
...
```

* **虚拟化内存：**每一个进程访问其**私有虚拟地址空间**（有时被称作地址空间，*address space*），操作系统以某种方式映射到物理内存上。

* > Indeed, that is exactly what is happening here as the OS is virtualizing memory. Each process accesses its own private virtual address space (sometimes just called its address space), which the OS somehow maps onto the physical memory of the machine.

---

### Concurrency

```c
//threads.c
#include <stdio.h>
#include <stdlib.h>
#include "common.h"
#include "common_threads.h"

volatile int counter = 0;
int loops;

void *worker(void *arg) {
	int i;
	for (i = 0; i < loops; i++) { 
		counter++;
	} 
    return NULL;
}
int main(int argc, char *argv[]) {
	if (argc != 2) {
		fprintf(stderr, "usage: threads <value>\n");
        exit(1);
	}
	loops = atoi(argv[1]);
    pthread_t p1, p2;
	printf("Initial value : %d\n", counter);
    
    Pthread_create(&p1, NULL, worker, NULL);
	Pthread_create(&p2, NULL, worker, NULL);
    Pthread_join(p1, NULL);
    Pthread_join(p2, NULL);
	printf("Final value : %d\n", counter);
    return 0;
}
```

```shell
prompt> gcc -o thread thread.c -Wall -pthread
prompt> ./thread 1000
Initial value : 0
Final value : 2000
```

```shell
prompt> ./thread 100000 
Initial value : 0
Final value : 143012	//错误
prompt> ./thread 100000
Initial value : 0
Final value : 137298	//错误
```

*主程序通过`Pthread create()`创造两个**线程（thread）***

​		上面过程的关键部分是增加共享计数器。它需要 3 条指令： 一条将计数器的值从内存加载到寄存器， 一条将其递增，另一条将其保存回内存。因为这3条指令并不是以**原子方式 ( atomically)** 执行（所有的指令一次性执行）的，所以会发生错误。

---

###  Persistence

> 与操作系统为CPU和内存提供的抽象不同，操作系统并不为任意应用程序创造私有的、虚拟的磁盘。

**操作系统如何将文件写入磁盘：**

1. 确定新数据写入到磁盘的位置*（first figuring out where on disk this new data will reside）*
2. 在文件系统维护的各种结构中对其进行记录*（keeping track of it in various structures the file system maintains）*

---

### Design Goal

1. 提供高性能*（provide high performance）*或者说最小化操作系统的开销*（minimize the overheads of the OS）*
2. 在应用程序之间及操作系统和应用程序之间提供保护*（provide protection between applications, as
   well as between the OS and applications）*

------

# Virtualization

进程：运行中的程序

> The definition of a **process**, informally, is quite simple: it is a running program.

通过运行一个程序，然后停止该程序，再运行另一个程序，以此类推，操作系统提供了仅有一个物理CPU时存在多个虚拟CPU的假象，这就是**时分共享**CPU技术，能够允许用户如愿运行多个并行程序。



**时分共享 (time sharing) **是操作系统共享资源所使用的最基本的技术之一。 通过允许资源由一个实 体使用一小段时间，然后由另一个实体使用一小段时间，如此下去，所谓的资源（例如，CPU或网络链接）可以被许多人共享。时分共享的自然对应技术是空分共享，资源在空间上被划分给希望使用它的人。例如，磁盘空间自然是一个空分共享资源，因为一旦将块分配给文件，在用户删除文件之前，不可能将它分配给其他文件。

> **Time sharing** is a basic technique used by an OS to share a resource. By allowing the resource to be used for a little while by one entity, and then a little while by another, and so forth, the resource in question (e.g., the CPU, or a network link) can be shared by many. The counterpart of time sharing is space sharing, where a resource is divided (in space) among those who wish to use it. For example, disk space is naturally a space-shared resource; once a block is assigned to a file, it is normally not assigned to another file until the user deletes the original file.

---

### The Abstraction: A Process

**机器状态（*machine state*）：**程序在运行时可以读取或者更新的内容

**内存（*memory*）**是进程的机器状态的一个明显的组成部分，指令存放在内存中，运行中的程序读写的数据也存放在内存中。因此，进程可以访问的内存（称为**地址空间**， *address space*）是进程的一部分。

**寄存器（*register*）**是进程的机器状态的另一部分。很多指令显式地读取或者更新寄存器，因此，寄存器明显对进程的执行有重要的作用。

注意：一个特殊的寄存器组成机器状态的一部分。例如：**程序计数器**（***program counter***，有时也被称作**指令指针，Instruction Pointer， IP**）告诉我们程序将会执行的下一条指令；**栈指针** (***stack pointer***) 和相关的**帧指针** (***frame pointer*** ) 用于管理函数参数栈、局部变量和返回地址。

------

#### Process API

现代操作系统必须以某种形式提供这些API：

* 创建（create）：一个操作系统必须包括一些创建新进程的方法
* 销毁（destroy）：由于存在进程创建的接口，系统应当提供强制销毁进程的接口
* 等待（wait）：有时候等待进程结束运行是有用的，因此某种等待接口会被提供
* 其他控制（miscellaneous control）：除了杀死或者等待进程，有时存在一些其他控制。例如，大部分的操作系统提供一些方法来暂停进程，然后重启
* 状态（status）：通常存在一些接口能够获得一些进程的状态信息

---

#### Process Creation: A Little More Detail

​		操作系统运行程序首先做的一件事是：**加载（*load*）**程序的代码和所有静态数据（例如初始化变量）到内存中，加载到进程的地址空间中。程序最初以某种可执行格式驻留在磁盘上，因此，将程序和静态数据加载到内存中的过程，需要操作系统从磁盘中读取这些字节，并放到内存中的某处。

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/Loading%20From%20Program%20To%20Process.jpg" alt="Loading: From Program To Process" style="zoom:67%;" />

​		程序运行之前操作系统仍有一些事要做：必须为程序的**运行时栈**（***run-time stack***，或者**栈**，***stack***）分配内存。操作系统也会为程序的**堆**（***heap***）分配一些内存，堆会被诸如链表，散列表，树之类的数据结构所需要。操作系统还执行一些其他的初始化任务，尤其是与I/O相关的任务。例如，在Unix操作系统中，每一个进程都默认有三个打开的**文件描述符**（***file description***），用于标准输入、输出、错误，这些文件描述符让程序轻松地读取终端中的输入并输出至屏幕。

​		通过将代码和静态数据加载到内存中，通过创建和初始化栈以及执行与 I/O 设置相关的其他工作，操作系统仅剩下随后一项任务：在入口处运行，启动程序，即`main()`。通过跳转到`main()`例程，操作系统将CPU的控制权交到新创建的进程中，从而程序开始执行。

---

#### Process States

一个程序可以是下列三个状态的其中一个：

* 运行（running）：在运行状态，进程正运行在处理器上，这意味着正在执行指令
* 就绪（ready）：在就绪状态下，进程已准备好运行，但是由于一些原因，操作系统选择不在此时运行
* 阻塞（blocked）：在阻塞状态下，进程执行了某些操作，直到发生了其他事件时才会运行

---

#### Data Structure

为了追踪每个进程的状态，操作系统可能会为所有就绪的进程准备某种**进程列表**（***process list***），以及跟踪当前正在运行的进程的一些附加信息

```c
// the registers xv6 will save and restore
// to stop and subsequently restart a process
struct context {
    int eip; 
    int esp; 
    int ebx;
    int ecx; 
    int edx; 
    int esi;
    int edi;
    int ebp;
};

/ the different states a process can be in enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE };
// the information xv6 tracks about each process 
// including its register context and state
struct proc { 
    char *mem;	// Start of process memory 
    uint sz;	// Size of process memory
	char *kstack;	// Bottom of kernel stack 
    				// for this process
    enum proc_state state;	// Process state
    int pid;	// Process ID
	struct proc *parent;	// Parent process
    void *chan;		// If !zero, sleeping on chan
    int killed;		// If !zero, has been killed
	struct file *ofile[NOFILE]; // Open files
    struct inode *cwd;		//Curent directory
    struct context context;		//Switch here to run process
    struct trapframe *tf;		//Trap frame for the
    							//current interrupt
```

​		**上下文切换（*context switch*）：**对于停止的进程，**寄存器上下文**（***register context***）将保存其寄存器的内容。当一个进程停止时，它的寄存器会被保存到它的内存位置。通过恢复这些寄存器（将它们的值放回到实际的物理寄存器中），操作系统能够恢复运行进程。

​		有时候，系统会有一个**初始**（***initial***）状态，表示进程处于被创建的状态。除此以外，进程也会处于已退出但未被清理的**最终**（***final***）状态，*在UNIX-based 系统中，这被称作僵尸（zombie）状态*。最终状态非常有用，因为它允许其他进程（通常时创建进程的父进程）检验进程的返回代码，并且查看刚刚结束的进程是否成功执行。完成后，父进程将执行一个最后一个调用，以等待子进程的完成，并告诉操作系统可以清理这个正在结束的进程的所有相关数据结构。

---

* 进程是运行中的程序的一个主要操作系统抽象。在任何时间，进程都可以它的状态来描述：位于其地址空间的内存的内容，CPU寄存器中的内容（包括程序计数器，栈帧，等等）和I/O相关的信息
* 进程接口包括与进程相关的调用程序
* 进程存在多种不同的进程状态，包括运行、就绪和阻塞。不同的事件将进程从一个状态转换为其他的状态
* 一个进程列表包含了系统中所有进程的信息。每一个入口能在process control block（PCB）中被发现

>* The process is the major OS abstraction of a running program. At any point in time, the process can be described by its state: the contents of memory in its address space, the contents of CPU registers (including the program counter and stack pointer, among others), and information about I/O (such as open files which can be read or written).
>* The process API consists of calls programs can make related to processes. Typically, this includes creation, destruction, and other useful calls.
>* Processes exist in one of many different process states, including running, ready to run, and blocked. Different events (e.g., getting scheduled or descheduled, or waiting for an I/O to complete) transition a process from one of these states to the other.
>* A process list contains information about all processes in the system. Each entry is found in what is sometimes called a process control block (PCB), which is really just a structure that contains information about a specific process.

---

### Interlude: Process API

#### The fork() System Call

**进程描述符**（***process identifier***）：在UNIX系统中，如果要操作一个进程，就要通过PID指明

```c
//call.c
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid()); 
    int rc = fork(); 
    if (rc < 0) {
        // fork failed
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0) {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
    }else {
        // parent goes down this path (main)
        printf("hello, I am parent of %d (pid:%d)\n", rc, (int) getpid());
    }
    return 0;
 }
```

```shell
$ ./call
hello world (pid:178)
hello, I am parent of 179 (pid:178)
hello, I am child (pid:179)
```

`fork()`被调用时存在三种可能的返回值：

* 在父进程中，`fork()`返回新创建子进程的进程ID
* 在子进程中，`fork()`返回0
* 如果出现错误，`fork()`返回一个负值



​		当程序刚开始运行时，进程打印了一个“hello world”的信息，其中包括了它的进程描述符（PID）。然后，进程调用了`fork()`系统调用。新创建的进程几乎与调用进程完全一样。新创建的进程称为子进程（child），原来的进程称为父进程（parent）。子进程并不会从`main()`函数开始执行（由于hello world只输出了一次），而是直接从`fork()`系统调用返回。

​		子进程并不是一份完全的拷贝，虽然它拥有自己的地址空间，寄存器，程序计数器，等等，但是它从`fork()`中的返回值是不同的。父进程接收到的是它新创建的子进程的PID，而子进程获得的返回值是`0`。

---

#### The wait() System Call

```c
//call2.c
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>
int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid()); 
    int rc = fork(); 
    if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n"); 
        exit(1);
    } else if (rc == 0) { 
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid());
        } else {        // parent goes down this path (main)
            int rc_wait = wait(NULL);
            printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n", rc, rc_wait, (int) getpid());
        } 
    return 0;
}
```

```shell
$  ./call2
hello world (pid:195)
hello, I am child (pid:196)
hello, I am parent of 196 (rc_wait:196) (pid:195)
```

在该例子中，父进程调用了`wait()`来延迟执行直到子进程结束执行。当子进程结束后，`wait()`返回到父进程。

对`wait()`的调用会阻塞调用进程，直到它的一个子进程退出或接收到信号。子进程终止后，父进程继续执行`wait()`系统调用后面的指令。

* 如果进程有多个子进程，则在调用`wait()`后，如果没有子进程终止，则父进程必须处于等待状态
* 如果只有一个子进程终止，则返回`wait()`返回终止的子进程的进程ID
* 如果终止了多个子进程，则`wait()`将获取任意的子进程并返回该子进程的进程ID

---

#### The exec() System Call

```c
//call3.c
#include <stdio.h>
#include <stdlib.h> 
#include <unistd.h>
#include <string.h> 
#include <sys/wait.h>
int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid()); 
    int rc = fork(); 
    if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n"); 
        exit(1);
    } else if (rc == 0) { 
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int) getpid()); 
        char *myargs[3];
        myargs[0] = strdup("wc"); // program: "wc" (word count) 
        myargs[1] = strdup("call3.c"); // argument: file to count 
        myargs[2] = NULL; // marks end of array
        execvp(myargs[0], myargs); // runs word count 
        printf("this shouldn’t print out");
    }   else {                  // parent goes down this path (main)
        int rc_wait = wait(NULL);
        printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n", rc, rc_wait, (int) getpid());
    } 
    return 0; 
}
```

```shell
$ ./call3
hello world (pid:240)
hello, I am child (pid:241)
 26 122 974 call3.c
hello, I am parent of 241 (rc_wait:241) (pid:240)
```

​		给定可执行程序的名称和参数后，`exec()`从可执行程序中加载代码（以及静态数据），并且覆写代码段（及现有的静态数据）；堆和栈，以及程序内存空间的其他部分被重新初始化。然后，操作系统简单地运行程序，将参数通过`argv`传递给进程。这并不会创建一个新的进程，它将正在运行的程序（`call3`)转换为一个不同的程序（`wc`）子进程调用`exec()`后，就像从来没有运行过`call3`一样。成功的`exec()`调用永远不会返回。

> 实际上， exec() 有几种变体： `execl`， `execlp()` ，`execle()` ，`execv()`， `execvp()`和`execvp()` 。

---

```c
#include <stdio.h>
#include <stdlib.h> 
#include <unistd.h>
#include <string.h> 
#include <fcntl.h>
#include <sys/wait.h>
int main(int argc, char *argv[]) {
    int rc = fork(); 
    if (rc < 0) {
        // fork failed; exit
        fprintf(stderr, "fork failed\n"); 
        exit(1);
    } else if (rc == 0) { 
        // child: redirect standard output to a file 
        close(STDOUT_FILENO);
        open("./call4.output", O_CREAT|O_WRONLY|O_TRUNC, S_IRWXU);
        
        // now exec "wc"...
        char *myargs[3];
        myargs[0] = strdup("wc"); // program: "wc" (word count) 
        myargs[1] = strdup("call4.c"); // argument: file to count 
        myargs[2] = NULL; // marks end of array
        execvp(myargs[0], myargs); // runs word count 
    }   else {                  // parent goes down this path (main)
        int rc_wait = wait(NULL);
    } 
    return 0; 
}
```

```shell
$ ./call4
$ cat call4.output
 27 108 887 call4.c
```

​		**重定向**（***redirected***）的工作原理，是基于对操作系统管理文件描述符的假定。UNIX系统从0开始寻找可以使用的文件描述符。在这个例子中，STDOUT _FILENO 将成为第一个可用的文件描述符，因此在`open()`被调用时，得到赋值。然后子进程向标准输出文件描述符的写入（例如通过`printf()`这样的函数），都被透明地转向新打开的文件，而不是屏幕。`call4`确实调用了`fork()`来创建新的子进程，然后通过对`execvp()`的调用运行`wc`程序。没有在屏幕上看见任何输出的原因在于输出被重定向到了文件`call.output`。

------

### Mechanism : Limited Direct Execution

#### Basic technique: Limited Direct Execution

​		当操作系统想要启动程序运行时，会在进程列表中创建一个进程入口，为进程分配一些内存，将程序代码从磁盘中加载到内存中，找到入口点，跳转到那里，并开始运行用户的代码。

​																						*直接运行协议（无限制*）

|                           操作系统                           |                   程序                    |
| :----------------------------------------------------------: | :---------------------------------------: |
| 在进程列表创建条目<br>为程序分配内存<br> 将程序加载到内存<br> 根据`argc`/`argv`设置栈 |                                           |
|             清楚寄存器<br> 执行`call main()`方法             |                                           |
|                                                              | 执行`main()`<br> 从`main()`种执行`return` |
|              释放进程的内存<br> 从进程列表清除               |                                           |

------

#### Problem #1： Restricted Operations

**用户模式**（***user mode***）：在用户模式下运行的代码会受到限制

**内核模式**（***kernel mode***）：与用户模式不同，操作系统（或者内核）就在这个模式下运行。在这个模式下，运行的代码可以执行它想做的事情，包括特权操作，比如发出I/O请求和执行所有类型的受限指令



​		几乎所有的现代硬件都为用户程序提供了进行系统调用的能力。系统调用允许内核小心翼翼地向用户程序暴露一定的关键功能，例如访问文件系统，创建和销毁进程，与其他进程进行交流，和分配更多的内存。

​		为了执行系统调用，程序必须执行一个特殊的**陷阱**（***trap***）指令。该指令同时跳入内核并且将特权级别提升到内核模式。一旦进入内核，系统就能够进行任何需要的特权操作（如果被允许的话），从而为调用进程提供需要的工作。当结束时，操作系统调用一个特殊的**从陷阱返回**（***return-from-trap***）指令，就像你所期望的，该指令返回到发起调用的用户程序时，同时减少特权等级至用户模式。

​		执行陷阱时，硬件必须小心，它必须确保存储足够的调用者寄存器来保证当操作系统发起从陷阱返回指令时能够正确返回。在***X86***上，比如，处理器将程序计数器，标志和一些其他的寄存器推送到每个进程的**内核栈**（***kernel stack***）。从陷阱返回将会将这些值从栈种推出并恢复执行用户模式程序。

​		内核通过在启动时设置**陷阱表**（***trap table***）来实现。



​																								*受限直接运行协议*

|                操作系统启动@启动（内核模式）                 |                             硬件                             |                    程序（用户模式）                     |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :-----------------------------------------------------: |
|                         初始化陷阱表                         |                                                              |                                                         |
|                                                              |                  记住系统调用处理程序的地址                  |                                                         |
|                  操作系统＠运行（内核模式）                  |                             硬件                             |                    程序（应用模式）                     |
| 在进程列表上创建条目<br> 为程序分配内存<br> 将程序加载到内存<br> 根据`argv`设置程序栈<br> 用寄存器/程序计数器填充内核栈从陷阱返回 |                                                              |                                                         |
|                                                              |       从内核栈恢复程序<br> 转向用户模式<br> 跳到`main`       |                                                         |
|                                                              |                                                              | 运行`main`<br> ······<br> 调用系统调用<br> 陷入操作系统 |
|                                                              | 将寄存器保存到内核栈<br> 转向内核模式<br/> 跳到陷阱处理程序  |                                                         |
|        处理陷阱<br> 做系统调用的工作<br> 从陷阱中返回        |                                                              |                                                         |
|                                                              | 从内核栈恢复寄存器<br/> 转向用户模式<br/> 跳到陷阱之后的程序计数器 |                                                         |
|                                                              |                                                              |       ······从`main`返回<br> 陷入（通过`exit()`）       |
|              释放进程的内存将进程从进程列表清除              |                                                              |                                                         |

​		为了指定精确的系统调用，经常会为每个系统调用分配**系统调用数字**（***system-call number***）。用户代码有责任将所需要的系统调用数字放置在寄存器中或者在栈中的指定位置。操作系统，当处理陷阱处理器中的系统调用时，检验这个数字，确保它是有效的，并且，如果它是有效的，执行相关的代码。

​		在**受限直接运行协议**（***limited direct execution， LDE***）中有两个阶段。在第一个阶段中，内核初始化陷阱表，并且CPU记住它的位置以供随后使用。内核完成该项操作通过一个特权指令。在第二个阶段中（当运行一个进程时），内核在使用从陷阱返回指令来启动执行进程之前设置一些事情（例如，在进程列表中分配一个节点，分配内存）。这将CPU切换到用户模式并且开始运行进程。当进程想要发起系统调用时，它会重新陷入操作系统，然后再次将控制权通过`return-from-trap`交还给进程。进程然后完成它的工作，并且返回到`main()`。这通常会返回到一些存根代码，它将正确退出程序。此时操作系统清除干净，然后任务完成了。

------

#### Problem #2：Switching Between Processes

​		运行过久的进程会被假定会定期放弃CPU，因此操作系统可以决定运行其他任务。大多数的进程，最终都会通过发起系统调用频繁地将CPU的控制权交给操作系统。像这样的系统通常包括一个**yield**系统，它什么都不做，仅仅将控制权交给操作系统，以便系统可以运行其他进程。当应用程序进行非法操作时，应用程序也会将控制权交给操作系统。因此，在**协作调用系统**（***cooperative scheduling system***）中，操作系统通过等待系统调用或者一些非法的操作来重新获得CPU的控制权。

------

#### A Non-Cooperative Approach： The OS Takes Control

**时钟中断**（***timer interrupt***）：一个可以编程来每隔几毫秒产生一次中断。当中断发生了，现在正在运行的进程会被停止，操作系统中预先配置好的**中断处理器**（***interrupt handler***）会运行。同时，操作系统会重新获得CPU的控制权，并且因此它可以做想做的事：停止现有的进程，并且开始一个不同的进程。

​		硬件在发生中断时有一些责任，尤其是在中断发生时，要为正在运行的程序保存足够的状态，来确保接下来的`return-from-trap`指令能够正确地重启程序。

------

#### Saving and Restoring Context

​		如果决定切换，操作系统就会执行一些底层的代码，我们称之为**上下文切换**（***context switch***）。上下文切换在概念上非常简单L所有的操作系统必须去做的是为正在执行的进程保存一些寄存器的值（比如，在它的内核栈上）并且为将要执行的进程（从它的内核栈）恢复一些寄存器的值。通过这样做，操作系统能够确保当`return-from-trap`指令完成执行时，系统重启其他的进程而不是返回到正在执行的进程。

​		通过切换栈，内核在进入切换调用代码时，是一个进程（被中断的进程）的上下文，在返回时是另一个进程（即将被执行的进程）的上下文。当操作系统最终完成`return-from-trap`指令时，即将执行的进程成为正在执行的进程。至此上下文完成切换。

| 操作系统@启动（内核模式）                                    | 硬件                                                         |                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------- |
| 初始化陷阱表                                                 |                                                              |                  |
|                                                              | 记住以下地址<br> 系统调用处理程序<br> 时钟处理程序           |                  |
| 启动中断时钟                                                 |                                                              |                  |
|                                                              | 启动时钟<br> 每隔*x*ms中断CPU                                |                  |
| 操作系统@运行（内核模式）                                    | 硬件                                                         | 程序（应用模式） |
|                                                              |                                                              | 进程A······      |
|                                                              | 时钟中断<br> 将寄存器(A)保存到内核栈(A)<br> 转向内核模式<br> 跳到陷阱处理程序 |                  |
| 处理陷阱<br> 调用`switch()`例程<br> 将寄存器(A)保存到进程结构(A)<br> 将进程结构(B)恢复到寄存器(B)<br> 从陷阱返回（进入B） |                                                              |                  |
|                                                              | 从内核栈(B)恢复寄存器(B)<br> 转向用户模式<br> 跳到B的程序计数器 |                  |
|                                                              |                                                              | 进程B······      |

​		在例子中，进程A正在运行，然后被时钟中断中断。硬件保存了它的寄存器（在它的内核栈上）并且进入内核（切换到内核模式），在时钟中断程序中，操作系统决定从运行进程A切换到进程B。在这时，它调用了`switch()`例程，仔细保存当前寄存器的值（保存到A的进程结构中），重新恢复了进程B的寄存器（从它的进程结构），然后切换上下文，具体来说是通过改变栈帧来使用B的内核栈。最终，操作系统从陷阱返回，恢复B的寄存器并且开始运行B。

​		注意到，在此协议中有两种类型的寄存器保存/恢复。第一种是发生时钟中断时，在这种情况下，正在运行的进程的用户寄存器被硬件隐式保存，使用该进程的内核栈。第二种是当操作系统决定从A切换到B，在这种情况下，内核寄存器有软件（例如操作系统）显式保存，但是这次被存储在该进程的进程结构的内存中。后一个操作让操作系统从好像刚刚由A陷入内核变成好像刚刚由B陷入内核。

*下面给出了xv6的上下文切换代码。`context`结构`old`和`new`分别在老的和新的进程的进程结构中*

```
# void swtch(struct context **old, struct context *new); 
#
# Save current register context in old 
# and then load register context from new. 
.globl swtch
swtch:
	# Save old registers 
	movl 4(%esp), %eax # put old ptr into eax 
	popl 0(%eax)	   # save the old IP
	movl %esp, 4(%eax) # and stack
	movl %ebx, 8(%eax) # and other registers
    movl %ecx, 12(%eax) 
    movl %edx, 16(%eax) 
    movl %esi, 20(%eax) 
    movl %edi, 24(%eax) 
    movl %ebp, 28(%eax)
	
	# Load new registers
	movl 4(%esp), %eax  # put new ptr into eax 
	movl 28(%eax), %ebp # restore other registers 
	movl 24(%eax), %edi 
	movl 20(%eax), %esi 
	movl 16(%eax), %edx 
	movl 12(%eax), %ecx 
	movl 8(%eax), %ebx
	movl 4(%eax), %esp # stack is switched here 
	pushl 0(%eax) ret  # return addr put in place 
	ret                # finally return into new ctxt
```

------

主要的CPU虚拟化术语（机制）

* CPU应该支持至少两种执行模式：一种受限的**用户模式**和一种特权（没有限制的）**内核模式**
* 典型的用户应用程序运行在用户模式，并且使用系统调用进入内核来要求操作系统服务
* 陷阱指令仔细地保存寄存器状态，将硬件状态更改为内核模式，并且跳转到操作系统预先设置的目标：陷阱表
* 当操作系统结束服务一个系统调用，它通过另一个特殊的从陷阱返回指令返回到用户程序，减少特权并且在跳转到操作系统的陷阱之后，将控制权返回给指令
* 陷阱表必须在启动时段由操作系统设好，确保它们不能被用户程序修改，这些都是**limited direct execution**协议的一部分，使程序有效运行当却不会丢失操作系统控制权
* 当程序正在运行时，操作系统必须使用硬件机制来确保用户程序不会永远运行，这就是时间中断。这种方法是CPU调度的非协作方法。
* 有时候，操作系统在时钟中断或系统调用中也许会希望从正在运行进程切换到一个不同的进程，这种低级别的技术被称作上下文切换

------

### Scheduling : Introduction

​		我们做出的**工作负载**（***workload***）假说是不切实际的，但是这没有问题（目前），因为我们将会放宽这些假定，并最终开发出我们所谓的一个完全可操作的调度准则：

1. 每一个工作运行相同的时间
2. 所有的任务在同一时间到达
3. 一旦开始，任务会运行至结束
4. 所有的工作只会使用CPU
5. 每一个工作的运行时间是知道的

------

#### Scheduling Metrics

任务的周转时间定义为任务完成时间减去任务到达系统的时间：
$$
T_{turnaround} = T_{completion} - T_{arrival}
$$

#### First In，First Out（FIFO）

我们可以实现的最基本的算法是**先进先出**，有时我们也称之为**先到先服务**（***First Come， First Served（FCFS）***）。该算法会存在被称为**护航效应**（***convoy effect***）的问题，一些耗时较少的潜在资源消费者会被排到重量级的资源消费者后，导致耗时增加。

<img src = "https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/FIFO%20Simple%20Example.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/Why%20FIFO%20Is%20Not%20That%20Great.jpg" style="zoom:67%;" >



#### Shortest Job First （SJF）

**最短任务优先**：先运行最短的任务，然后是次短的任务，如此下去。但是该算法仍然会遇到同样的护航问题。

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/SJF%20Simple%20Example.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/SJF%20With%20Late%20Arrivals%20From%20B%20and%20C.jpg" style="zoom:67%;" >

#### Shortest Time-to-Completion First (STCF)

**最短完成时间优先**：或者抢占式最短作业优先。每当新工作进入系统时，它会确定剩余的工作（包括新工作）谁的剩余时间最少，然后调度该工作。

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/STCF%20Simple%20Example.jpg" style="zoom:67%;" >

------

#### A New Metric: Response Time

**响应时间**（***response time***）：从任务到达系统到首次运行的时间
$$
T_{response}=T_{firstrun}-T_{arrival}
$$

#### Round Robin

**轮转**（***round robin***）调度：轮转调度在一个**时间片**（***time slice***）内运行工作并且切换到运行队列中的下一个任务，而不是运行工作直到结束。它反复执行，直到所有任务完成。因此，轮状有时也被称作**时间切片**（***time-slicing***）。**时间片的长度必须是时间中断周期的倍数**。时间片长度对于轮转是至关重要的，越短，轮转在响应时间上的表现越好。然而，时间片过短会有一个问题：切换上下文带来的消耗会影响整体的性能。

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Operating%20Systems/SJF%20Again%20and%20Round%20Robin.jpg" style="zoom:67%;" >

$$
T_{response} = T_{firstrun}-T_{arrival}
$$

------

#### 结合IO

<img src="E:\pic\7.8.png" style="zoom: 67%;" />

---

### Scheduling: The Multi-Level  Feedback Queue

#### MLFQ：多级反馈队列

* 规则1：如果A的优先级$>$B的优先级，运行A
* 规则2：如果A的优先级$=$B的优先级，轮转运行A和B
* 规则3：工作进入系统时，放在最高优先级
* 规则4：一旦工作用完了其在某一层的时间配额，就降低其优先级
* 规则5：经过一段时间*S*，就将系统中所有工作重新加入最高优先级队列
