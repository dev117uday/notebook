
# Java Threads

A Thread is a small set of instructions designed to be scheduled and executed by the CPU independently of the parent process.
To run the process in another thread, you the support of both the operating system and the process. When you create another thread, JVM communicates OS to create a new thread which can then run on the "multi-threaded CPU".
Well this article isn't about explain threads, it's about how you can play with them in Java

### Threads are everywhere by default

- When write and run a simple java program, JVM is creating the main thread for you to print hello world

```java
public class Solution {

	public static void main(String[] args) throws Exception {
		System.out.println("hello world");
		throw new Exception();
	}
}

// output : 
hello world
Exception in thread "main" java.lang.Exception
        at Solution.main(Solution.java:5)
```

If you read the error message carefully, it says `Exception in thread "main"` , hence proved ...

### How to create a thread ?

Well, cuz Java is an Object-Oriented Language, everything is wrapped in OOP concepts. To run a method of some class in-parallel

- Extend the class from the Thread class 
- Create a method overriding the run() method

This run method is what is executed separately in a thread.

Here is an example :

```java
class Hi extends Thread {
	@Override
	public void run() {
		for (int i = 1; i < 5; i++) {
			System.out.println("hi");
			
			// just sleeping for some time
			try {Thread.sleep(400);} catch (Exception e) {}
		}
	}
}

public class App {
	public static void main(String[] args) {
		Hi obj1 = new Hi();
		obj1.start();
	}
}
```

To run two thread of seperate class

```java
class Hi extends Thread {
	@Override
	public void run() {
		for (int i = 1; i < 5; i++) {
			System.out.println("hi");

			try { Thread.sleep(400); } catch (Exception e) {}
		}

	}
}

class Hello extends Thread {
	public void run() {
		for (int i = 1; i < 5; i++) {
			System.out.println("hello");
			
			try { Thread.sleep(400); } catch (Exception e) {}
		}
	}
}

public class App {
	public static void main(String[] args) {
		Hi obj1 = new Hi();
		Thread obj2 = new Hello();

		obj1.start();
		obj2.start();
	}
}
```



When you call the `.start()` method, it will execute the `run()` method in a seperate thread.

Well becuase java doesnt support mulitple inheritanc, if you ever have to inherit from another class, you wont be able to do it. To overcome this limitation/feature  we can implement the `Runnable` interface

```java
class Hi implements Runnable {
	public void run() {
		for (int i = 1; i < 5; i++) {
			System.out.println("hi");

			try { Thread.sleep(400); } catch (Exception e) {}
		}
	}
}

class Hello implements Runnable {
	public void run() {
		for (int i = 1; i < 5; i++) {
			System.out.println("hello");
            
			try { Thread.sleep(400); } catch (Exception e) {}
		}
	}
}

public class App {
	public static void main(String[] args) {
		Runnable obj1 = new Hi();
		Runnable obj2 = new Hello();

		Thread t1 = new Thread(obj1);
		Thread t2 = new Thread(obj2);

		t1.start();
		t2.start();
	}
}
```

The only difference here is you implement the Runnable interface, create objects of the Thread class passing the objects implementing Runnable, and call the start method on the thread object, which in--return executes the `.run()` methods.

### Properties of a Thread Class

- `getName() : String`  gets you the name of the thread
  - `setName(String name)` : set name of a thread
- `isAlive() : Boolean` whether the thread is active or not
- `getId() : Long` get the execution id of the thread
- `getPriority()` : get the priority of thread 
  - `setPriority(int num)` : on a scale of 1-10
  - Predefined ENUMS
    - `Thread.MAX_PRIORITY` = 10
    - `Thread.MIN_PRIORITY` = 1
    - `Thread.NORM_PRIORITY` = 5

For more, refer to documentation : https://download.java.net/java/early_access/loom/docs/api/java.base/java/lang/Thread.html

```java
class Hi extends Thread {
	public void run() {
		for (int i = 1; i < 5; i++) {
			try { Thread.sleep(400); } catch (Exception e) { }
		}
	}
}

public class App {
	public static void main(String[] args) throws InterruptedException {
		Hi obj1 = new Hi();
		obj1.start();

		System.out.println("Name : " + obj1.getName());
		var threadName = obj1.getName();
		System.out.println(threadName + " isAlive  : " + obj1.isAlive());
		System.out.println(threadName + " threadId : " + obj1.getId());
		System.out.println(threadName + " priority : " + obj1.getPriority());
		

		System.out.println(Thread.MAX_PRIORITY);
		System.out.println(Thread.MIN_PRIORITY);
		System.out.println(Thread.NORM_PRIORITY);

		obj1.join();
	}
}
```



### Joining Threads

When you spin off a thread, you also have to wait for it to finish its task before moving on to avoid errors/exception

```java
class App {
	public static void main(String[] args) {

		Thread obj1 = new Thread(() -> {
			for (int i = 1; i < 5; i++) {
				System.out.println(Thread.currentThread().getName());
				try { Thread.sleep(400); } catch (Exception e) {}
			}
		}, "Thread 1");

		Thread obj2 = new Thread(() -> {
			for (int i = 1; i < 5; i++) {
				System.out.println(Thread.currentThread().getName());
				try { Thread.sleep(400); } catch (Exception e) {}
			}
		}, "Thread 2");

		obj1.start();
		obj2.start();

		try {
			obj1.join();
			obj2.join();
		} catch (Exception e) {
			System.out.println("execption");
		}

		System.out.println("bye");
	}
}
```

### Synchronizing Threads

```java
class DataClass {
    int num;
    boolean valueSet = false;

    public synchronized void getNum() {
        while (!valueSet) {
            try {
                wait();
            } catch (Exception e) {
            }
        }
        System.out.println("get : " + num);
        valueSet = false;
        notify();
    }

    public synchronized void setNum(int num) {
        while (valueSet) {
            try {
                wait();
            } catch (Exception e) {
            }
        }

        this.num = num;
        System.out.println("set : " + num);
        valueSet = true;
        notify();
    }
}

class Producer implements Runnable {

    DataClass dataClass;

    public Producer(DataClass data) {
        this.dataClass = data;
    }

    @Override
    public void run() {
        int i = 0;
        while (true) {
            dataClass.setNum(i++);
            try {
                Thread.sleep(800);
            } catch (Exception e) {
                System.out.println("exception");
            }
        }
    }
}

class Consumer implements Runnable {

    DataClass dataClass;

    public Consumer(DataClass data) {
        this.dataClass = data;
    }

    @Override
    public void run() {
        while (true) {
            dataClass.getNum();
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
                System.out.println("exception");
            }
        }
    }
}

public class App {
    public static void main(String[] args) {
        DataClass dataClass = new DataClass();
        var producer = new Producer(dataClass);
        var consumer = new Consumer(dataClass);

        Thread t1 = new Thread(producer);
        Thread t2 = new Thread(consumer);

        t1.start();
        t2.start();
    }
}
```

