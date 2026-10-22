# Multitasking
Multitasking is a process of executing multiple tasks simultaneously. We use multitasking to utilize the CPU effectively. 

Multitasking can be achieved by two ways:
1. Process-based Multitasking (Multiprocessing)
	- Each process have its own address in memory i.e. each process allocates separate memory area.
	- Process is heavyweight.
	- Cost of communication between the process is high.
	- Switching from one process to another require some time for saving and loading registers, memory maps, updating lists etc.
2. Thread-based Multitasking (Multithreading)
	- Threads share the same address space.
	- Thread is lightweight.
	- Cost of communication between the thread is low

# Multithreading
Multithreading in java is a process of executing multiple threads simultaneously.
Thread is basically a lightweight sub-process, a smallest unit of processing. Multiprocessing and multithreading, both are used to achieve multitasking.

**Advantages**
1) It doesn't block the user because threads are independent and you can perform multiple operations at same time.
2) You can perform many operations together so it saves time.
3) Threads are independent so it doesn't affect other threads if exception occur in a single thread.

```java
public class A extends Thread {
	public void run() {
		System.out.println("thread is running !!");
	}
	public static void main(String[] args) {
		A t1 =new A();
		t1.start();
	}
}
```

```java
public class A implements Runnable {
	public void run() {
		System.out.println("running !!");
	}
	public static void main(String[] args) {
		A m1 = new A();
		Thread t1 = new Thread(m1);
		t1.start();
	}
}
```

## Thread Lifecycle
| State             | Description                         |
| :---------------- | :---------------------------------- |
| NEW               | Thread created but not yet started  |
| RUNNABLE          | Thread ready to run                 |
| RUNNING           | Actively executing                  |
| BLOCKED / WAITING | Waiting for a resource or condition |
| TERMINATED        | Execution finished                  |
## Thread Methods
| Method                | Description                                     |
| :-------------------- | :---------------------------------------------- |
| start()               | Starts the thread and calls run()               |
| run()                 | Code executed by the thread                     |
| sleep(ms)             | Temporarily stops thread for given milliseconds |
| join()                | Waits for another thread to finish              |
| setName() / getName() | Sets or gets thread name                        |
| setPriority()         | Sets priority (1–10)                            |
| isAlive()             | Checks if thread is still running               |
