#### Thread类中的方法的介绍



①start（）：启动当前线程

②run（）：需要重写此方法，线程中的执行代码

③currentThread（）：静态方法，返回当前代码的线程

```java
Thread.currentThread()
```

④getName（）：获取当前线程的名字

```java
Thread.currentThread().getName()
```

⑤setName（）：设置当前线程的名字

```java
     threandMeathod threandMeathod = new threandMeathod();
        threandMeathod.setName("线程一");
```

⑥yield（）：释放当前线程的执行权（不是放弃执行权，有可能再次执行此线程）

⑦jion（）：在主线程中，跳转到该线程执行，此时主线程处于堵塞状态，执行完才可以再次执行主线程

```java
if(i == 20){

    try {
        threandMeathod.join();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

}
```

⑧sleep（long millitime）：让当前线程阻塞（单位毫秒）

```java
try {
    sleep(100);
} catch (InterruptedException e) {
    e.printStackTrace();
```

⑨isAlive():判断线程是否还存活