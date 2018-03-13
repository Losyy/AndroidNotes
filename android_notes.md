# 一、Handler原理
> Handler、Looper、MessageQueue、Message四者的联系


## 1.1 Message


消息实体类，包含what、arg1、arg2、obj、target、callback、next等成员变量。
**next** 变量的存在允许Message行程一个单链表，MessageQueue的实现即基于该单链表。

`注意`：尽量使用obtain方法获取Message实例。obtain方法从一个全局消息池返回Message实例，可以避免每次都创建新实例，从而减少内存开销。


## 1.2 MessageQueue


消息队列，内部包含一个成员变量mMessages（类型为Message），以该变量作为表头，维护一个Message链表，作为消息队列的处理数据。

提供对消息队列的各种操作，入队、出队、删除等。


## 1.3 Looper


消息循环，内部维护一个静态成员变量sThreadLocal（类型为ThreadLocal<Looper>），通过该变量，在prepare方法中为当前线程创建一个Looper实例，用以处理MessageQueue里维护的消息队列（通过loop方法不断从MessageQueue取出Message进行处理）。

每个线程只能有一个Looper实例（通过sThreadLocal实现）。

loop方法中通过调用msg.target.dispatchMessage(msg)实际处理Message。msg.target为一个Handler实例。


## 1.4 Handler


消息处理类，作为消息处理机制的外部接口类，提供方法允许调用者发送消息（通过sendMessage或post等系列方法）、实际负责Message的处理（通过dispatchMessage和handleMessage等方法）。

dispatchMessage方法中消息处理优先级，msg.callback > mCallback > handleMessage()。


## 1.5 使用


```java
class LooperThread extends Thread {
    public Handler mHandler;

    public void run() {
        Looper.prepare();

        mHandler = new Handler() {
           public void handleMessage(Message msg) {
              // process incoming messages here
           }
        };

        Looper.loop();
   }
}
```


# 二、Binder原理


# 三、LruCache原理
