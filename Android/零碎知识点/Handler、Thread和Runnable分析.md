```
	//Represents a command that can be executed. Often used to run code in a different Thread.

	public interface Runnable {

    public void run();
 }
```


####Handler

`A Handler allows you to send and process Message and Runnable objects associated with a thread's MessageQueue.  Each Handler instance is associated with a single thread and that thread's message queue.  When you create a new Handler, it is bound to the thread message queue of the thread that is creating it -- from that point on, it will deliver messages and runnables to that message queue and execute them as they come out of the message queue.`

* Handler可以在线程的帮助下用来发送和处理Message和Runnable实例；

* 每个Handler实例都是和单个线程和此线程的消息队列绑定的；

* Handler负责的工作是传送message到消息队列中且在它们从队列中出来的时候对它们进行处理。
