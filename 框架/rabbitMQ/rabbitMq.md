## 1.work queue

工作队列(又称任务队列)的主要思想是避免立即执行资源密集型任务，而不得不等待它完成。 相反我们安排任务在之后执行。我们把任务封装为消息并将其发送到队列。在后台运行的工作进 程将弹出任务并最终执行作业。当有多个工作线程时，这些工作线程将一起处理这些任务。

### 1.1 发送消息

**生产者**

```java
public static void main(String[] args) throws IOException, TimeoutException {

    Channel channel = RabbitmqUtil.getChannel();

    //获取队列声明
    /** * 生成一个队列
     *  * 1.队列名称
     *  * 2.队列里面的消息是否持久化 默认消息存储在内存中
     *  * 3.该队列是否只供一个消费者进行消费 是否进行共享 true可以多个消费者消费
     *  * 4.是否自动删除 最后一个消费者断开连接以后 该队列是否自动删除 true 自动删除
     *  * 5.其他参数
     *  */
    channel.queueDeclare(TASK_QUEUE_NAME,false,false,false,null);


    Scanner scanner = new Scanner(System.in);

    while (scanner.hasNext()){
        String message = scanner.next();

        /**
         * 发送一个消息
         * 1.发送到那个交换机
         * 2.路由的 key 是哪个
         * 3.其他的参数信息
         * 4.发送消息的消息体
         */
        channel.basicPublish("",TASK_QUEUE_NAME,null,message.getBytes("UTF-8"));
        System.out.println("执行完成" + message);
    }

}
```

### 1.2 消息应答

> 消费者在接 收到消息并且处理该消息之后，告诉 rabbitmq 它已经处理了，rabbitmq 可以把该消息删除了。

消息应答主要分为两种：

#### 1.2.1 自动消息应答

> 消息发送后立即被认为已经传送成功，这种模式需要在**高吞吐量和数据传输安全性方面做权衡,**因为这种模式如果消息在接收到之前，消费者那边出现连接或者 channel 关闭，那么消息就丢失了,当然另一方面这种模式消费者那边可以传递过载的消息，没有对传递的消息数量进行限制， 当然这样有可能使得消费者这边由于接收太多还来不及处理的消息，导致这些消息的积压，最终 使得内存耗尽，最终这些消费者线程被操作系统杀死，所以**这种模式仅适用在消费者可以高效并 以某种速率能够处理这些消息的情况下使用**

#### 1.2.2手动消息应答

**手动应答的方式：**

先在信道的处理上将autoAck设置为false，之后在消息处理函数中，channel.basicAck返回tag 和是否批量应答。

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210707191355078.png" alt="image-20210707191355078" style="zoom: 67%;" />

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210707192026351.png" alt="image-20210707192026351" style="zoom:50%;" />

### 1.3  消息重新入队

> 如果消费者**由于某些原因失去连接(其通道已关闭，连接已关闭或 TCP 连接丢失)，导致消息 未发送 ACK 确认，RabbitMQ 将了解到消息未完全处理，并将对其重新排队**。如果此时其他消费者 可以处理，它将很快将其重新分发给另一个消费者。这样，即使某个消费者偶尔死亡，也可以**确保不会丢失任何消息**

### 1.4 代码实现手动应答

**消费者**

```java
public static void main(String[] args) throws IOException, TimeoutException {

    Channel channel = RabbitmqUtil.getChannel();


    //执行消息处理
    DeliverCallback deliverCallback = (consumerTag,message)->{

        SleepUtil.sleep(1);
        System.out.println("这是c1执行较快" + new String(message.getBody(),"UTF-8"));

        //手动应答
        /**
            1. 消息的tag位置
        	2.是否批量返回
        */
        channel.basicAck(message.getEnvelope().getDeliveryTag(),false);
    };


    //开启手动应答
    boolean autoAck = false;
    /**
     * 消费者消费消息
     * 1.消费哪个队列
     * 2.消费成功之后是否要自动应答 true 代表自动应答 false 手动应答
     * 3.消费者未成功消费的回调
     */
    channel.basicConsume(TASK_QUEUE_NAME,autoAck,deliverCallback,consumerTag -> {
        System.out.println("消息处理发生错误");
    });


}
```

**工具类**

```java
public class RabbitmqUtil {

      /**
     *  获取信道
     * @return
     * @throws IOException
     * @throws TimeoutException
     */
    public static Channel getChannel() throws IOException, TimeoutException {
        ConnectionFactory factory = new ConnectionFactory();
        factory.setUsername("admin");
        factory.setPassword("123");
        factory.setHost("192.168.2.4");
        Connection connection = factory.newConnection();
        Channel channel = connection.createChannel();

        return channel;
    }

}
```

### 1.5 rabbitMQ的持久化

> 当rabbitmq服务器宕机的时候，其消息是默认存储在缓存中的。没有持久化，可能造成消息的丢失。这时就需要rabbitmq实现其持久化到磁盘。

#### 1.5.1 队列持久化

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210707193129891.png" alt="image-20210707193129891" style="zoom:50%;" />

**注意点：**

是如果之前声明的队列不是持久化的，需要把原先队列先删除，或者重新 创建一个持久化的队列，不然就会出现错误

![image-20210707193240130](C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210707193240130.png)

#### 1.5.2 消息持久化

设置第二个参数为 :

> MessageProperties.PERSISTENT_TEXT_PLAIN 

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210707193530152.png" alt="image-20210707193530152" style="zoom:50%;" />

### 1.6不公平分发

> 如果这个任务我还没有处理完或者我还没有应答你，你先别分配给我，我目前只能处理一个 任务，然后 rabbitmq 就会把该任务分配给没有那么忙的那个空闲消费者，当然如果所有的消费者都没有完 成手上任务，队列还在不停的添加新任务，队列有可能就会遇到队列被撑满的情况，这个时候就只能添加 新的 worker 或者改变其他存储任务的策略。
>
> 类似于dubbo中的最小时间策略。

**在发布消息之前设置消息分发的策略**

```java
channel.basicQos(1);
```

### 1.7 预取值

> 就是在rabbitmq和消费者之间设置一个缓冲区，让消息能在这里等待

## 2. 发布确认

### 2.1 发布确认原理

生产者将信道设置成confirm模式，一旦信道进入confirm模式，**所有在该信道上面发布的消息都将会被指派一个唯一的ID(从1开始)，**一旦消息被投递到所有匹配的队列之后，broker就会发送一个确认给生产者(包含消息的唯一ID)，这就使得生产者知道消息已经正确到达目的队列了，如果消息和队列是可持久化的，那么确认消息会在将消息写入磁盘之后发出，broker回传给生产者的确认消息中delivery-tag域包含了确认消息的序列号，此外broker也可以设置basic.ack的multiple域，表示到这个序列号之前的所有消息都已经得到了处理

confirm模式最大的好处在于他是异步的，一旦发布一条消息，生产者应用程序就可以在等信道返回确认的同时继续发送下一条消息，当消息最终得到确认之后，生产者应用便可以通过回调方法来处理该确认消息，如果RabbitMQ因为自身内部错误导致消息丢失，就会发送一条nack消息，生产者应用程序同样可以在回调方法中处理该nack消息。

###    2.2  单个发布确认

>  waitForConfirmsOrDie(long)这个方法只有在消息被确认的时候才返回，如果在指定时间范围内这个消息没有被确认那么它将抛出异常。

```java
/**
 *  单个发布
 *
 * @throws Exception
 */
public static void single() throws Exception {
    Channel channel = RabbitmqUtil.getChannel();
    String name = UUID.randomUUID().toString();
    channel.queueDeclare(name,true,false,false,null);

    //开启发布确认
    channel.confirmSelect();

    long begin = System.currentTimeMillis();

    for(int i = 0; i < MESSAGE_COUNT; i++){

        String message = i + "";

        channel.basicPublish("",name,null,message.getBytes());

        //发布确认,开启单个确认
        boolean flag = channel.waitForConfirms();
        if(flag){
            System.out.println(message);
        }
    }

    long end = System.currentTimeMillis();
    System.out.println("单个发布时间：" + (end -  begin) + "ms");


}
```

### 2.3 异步发布确认

>  将发布一个**消息就存储到一个线程安全的跳跃表**。发布成功回调在成功的回调函数中删除，剩下的就是没有发布.
>
> **删除和查询是通过targetId来进行的**

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210720015324829.png" alt="image-20210720015324829" style="zoom: 50%;" />

```java
/**
 * 异步发布确认
 * @throws Exception
 */
public static void Async() throws Exception{

    Channel channel = RabbitmqUtil.getChannel();

    String name = UUID.randomUUID().toString();

    //声明信道
    channel.queueDeclare(name,true,false,false,null);

    //开启发布确认
    channel.confirmSelect();

    //开始时间
    long begin = System.currentTimeMillis();

    //创建一个跳跃表
    ConcurrentSkipListMap<Long,String> outstandingConfirms = new ConcurrentSkipListMap<>();


    /**
     * 成功的回调函数
     *
     * 1. 消息的数据位置
     * 2. 是否是批量发送
     */
    ConfirmCallback ackCallback = (deliveryTag,multiple) -> {
            outstandingConfirms.remove(deliveryTag);
    };

    //发送未成功的消息
    ConfirmCallback nackCallback = (deliveryTag,multiple) -> {
        String s = outstandingConfirms.get(deliveryTag);

        System.out.println(s);
    };
    
    //通过监听器来监听消息的发送情况
    //跟异步的，所以需要一个线程安全有序的存储容器
    channel.addConfirmListener(ackCallback,nackCallback);

    for (int i = 0; i < MESSAGE_COUNT; i++) {

        String message = i + "";

        //将发布的数据存储到这个map中
        //通过信道获取targetId
        outstandingConfirms.put(channel.getNextPublishSeqNo(),message);

        channel.basicPublish("",name,null,message.getBytes());
    }

    long end = System.currentTimeMillis();
    System.out.println("单个发布时间：" + (end -  begin) + "ms");
}
```



## 3. 交换机

RabbitMQ消息传递模型的核心思想是: 生产者生产的消息从不会直接发送到队列。实际上，通常生产者甚至都不知道这些消息传递传递到了哪些队列中。

相反，生产者只能将消息发送到交换机(exchange)，交换机工作的内容非常简单，一方面它接收来自生产者的消息，另一方面将它们推入队列。交换机必须确切知道如何处理收到的消息。是应该把这些消息放到特定队列还是说把他们到许多队列中还是说应该丢弃它们。这就的由交换机的类型来决定。

### 3.1 Exchanges的类型

直接(direct), 主题(topic) ,标题(headers) , 扇出(fanout)

#### 3.1.1 扇出(fanout)

就是将交换机的名称自己定义，RoutingKey直接使用空串。系统会直接分配一个随机空串。

这样就是将消息发送到信道，每一个信道自己独立。

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210720220855439.png" alt="image-20210720220855439" style="zoom:67%;" />

**ReceiveLogs01接收消息**

```java
public class ReceiveLogs01 {
    //定义交换机的名称
    public static final String EXCHANGE_NAME = "log";

    public static void main(String[] args) throws IOException, TimeoutException {

        //获取队列
        Channel channel = RabbitmqUtil.getChannel();

        //声明交换机
        channel.exchangeDeclare(EXCHANGE_NAME,"fanout");

        //声明临时对列
        String queueName = channel.queueDeclare().getQueue();

        //把该临时队列绑定我们的exchange 其中routingkey(也称之为binding key)为空字符串
        channel.queueBind(queueName,EXCHANGE_NAME,"");

        System.out.println("ReceiveLogs01等待连接");
        DeliverCallback deliverCallback = (consumerTag,message) -> {
            System.out.println("dReceiveLogs01的消息" + new String(message.getBody(),"UTF-8"));
        };

        CancelCallback cancelCallback = (consumerTag)->{

        };
        channel.basicConsume(queueName,true, deliverCallback,cancelCallback);

    }
}
```

**ReceiveLogs02接收消息**

```java
public class ReceiveLogs02 {
    //定义交换机的名称
    public static final String EXCHANGE_NAME = "log";

    public static void main(String[] args) throws IOException, TimeoutException {

        //获取队列
        Channel channel = RabbitmqUtil.getChannel();

        //声明交换机
        channel.exchangeDeclare(EXCHANGE_NAME,"fanout");

        //声明信道
        String queueName = channel.queueDeclare().getQueue();

        //把该临时队列绑定我们的exchange 其中routingkey(也称之为binding key)为空字符串
        channel.queueBind(queueName,EXCHANGE_NAME,"");


        System.out.println("ReceiveLogs02等待连接");
        DeliverCallback deliverCallback = (consumerTag,message) -> {
            System.out.println("dReceiveLogs02的消息" + new String(message.getBody(),"UTF-8"));
        };

        CancelCallback cancelCallback = (consumerTag)->{

        };
        channel.basicConsume(queueName,true, deliverCallback,cancelCallback);

    }
}
```

**EmitLog发消息**

```java
public class EmitLog {

    public static final String EXCHANGE_NAME = "log";
    public static void main(String[] args) throws Exception {
        Channel channel = RabbitmqUtil.getChannel();

        //发布订阅模式
        channel.exchangeDeclare(EXCHANGE_NAME,"fanout");

        Scanner scanner = new Scanner(System.in);

        while (scanner.hasNext()){

            String s = scanner.next();
            //这里就直接将消息发送到交换机上，不需要发送到信道上了
            channel.basicPublish(EXCHANGE_NAME,"",null,s.getBytes("UTF-8"));
            System.out.println("EmitLog发送消息" + s);
        }
    }

}
```

#### 3.1.2 直接(direct)

就是在扇出的情况上定义一个RoutingKey消息发送到指定的地方，但是其只能发送到一个RoutingKey的信道。可以对一个信道进行多重绑定。

这里就不做代码演示了

#### 3.1.3 主题(topic)

其类似于模糊查询，给到一个RoutingKey的范围，使这个范围内的信道都可以。、

**注意点**

> 发送到类型是topic交换机的消息的routing_key不能随意写，必须满足一定的要求，它必须是一个单词列表，以点号分隔开。这些单词可以是任意单词，比如说："stock.usd.nyse", "nyse.vmw", "quick.orange.rabbit".这种类型的。
>
> 当然这个单词列表最多不能超过255个字节。
>
> 在这个规则列表中，其中有两个替换符是大家需要注意的
>
>  	***(星号**)可以代替一个单词
>      	  #(井号)可以替代零个或多个单词

**接收方1**

```java
public class ReceiveLogsTopic01 {

    public static final String EXCHANGE_NAME = "topic_logs";

    public static void main(String[] args) throws Exception {

        Channel channel = RabbitmqUtil.getChannel();
        java.lang.String queueName = "Q1";

        //声明队列和绑定交换机
        channel.exchangeDeclare(EXCHANGE_NAME,"topic");
        channel.queueDeclare(queueName,false,false,false,null);
        channel.queueBind(queueName,EXCHANGE_NAME,"*.orange.*");

        System.out.println("Q1等待发送。。。。。。。。。。。");
        DeliverCallback deliverCallback = (consumerTag, message)->{
            System.out.println(new String(message.getBody(),"UTF-8"));

        };

        //接收消息
        channel. basicConsume(queueName,true,deliverCallback,(consumerTag) ->{});

    }
}
```

**接收方2**

```java
public class ReceiveLogsTopic02 {

    public static final String EXCHANGE_NAME = "topic_logs";

    public static void main(String[] args) throws Exception {

        Channel channel = RabbitmqUtil.getChannel();
        String queueName = "Q2";

        //声明队列和绑定交换机
        channel.exchangeDeclare(EXCHANGE_NAME,"topic");
        channel.queueDeclare(queueName,false,false,false,null);
        channel.queueBind(queueName,EXCHANGE_NAME,"*.*.rabbit");
        channel.queueBind(queueName,EXCHANGE_NAME,"lazy.#");

        System.out.println("Q2等待发送。。。。。。。。。。。");
        DeliverCallback deliverCallback = (consumerTag, message)->{
            System.out.println(new String(message.getBody(),"UTF-8"));

        };

        //接收消息
        channel. basicConsume(queueName,true,deliverCallback,(consumerTag) ->{});

    }
}
```

**发送方**

```java
public class EmitLogTopic {

    public static final String EXCHANGE_NAME = "topic_logs";

    public static void main(String[] args) throws IOException, TimeoutException {
        Channel channel = RabbitmqUtil.getChannel();

        //声明交换机
        channel.exchangeDeclare(EXCHANGE_NAME, BuiltinExchangeType.TOPIC);

        Map<String,String> bindingKeyMap = new HashMap<>();

        bindingKeyMap.put("quick.orange.rabbit","被队列Q1Q2接收到");
        bindingKeyMap.put("lazy.orange.elephant","被队列Q1Q2接收到");
        bindingKeyMap.put("quick.orange.fox","被队列Q1接收到");
        bindingKeyMap.put("lazy.brown.fox","被队列Q2接收到");
        bindingKeyMap.put("lazy.pink.rabbit","虽然满足两个绑定但只被队列Q2接收一次");
        bindingKeyMap.put("quick.brown.fox","不匹配任何绑定不会被任何队列接收到会被丢弃");
        bindingKeyMap.put("quick.orange.male.rabbit","是四个单词不匹配任何绑定会被丢弃");
        bindingKeyMap.put("lazy.orange.male.rabbit","是四个单词但匹配Q2");

        for(Map.Entry<String, String> bindingKeyEntry: bindingKeyMap.entrySet()){
            channel.basicPublish(EXCHANGE_NAME,bindingKeyEntry.getKey(),null,bindingKeyEntry.getValue().getBytes("UTF-8"));
        }

    }

}
```



## 4. 死信队列

### 4.1  定义

死信，顾名思义就是无法被消费的消息，字面意思可以这样理解，一般来说，producer将消息投递到broker或者直接到queue里了，consumer从queue取出消息进行消费，但**某些时候由于特定的原因导致queue中的某些消息无法被消费**，这样的消息如果没有后续的处理，就变成了死信，有死信自然就有了死信队列。

应用场景:为了保证订单业务的消息数据不丢失，需要使用到RabbitMQ的死信队列机制，当消息消费发生异常时，将消息投入死信队列中.还有比如说: 用户在商城下单成功并点击去支付后在指定时间未支付时自动失效

### 4.2  死信队列的来源

**消息TTL过期**

**队列达到最大长度**(队列满了，无法再添加数据到mq中)
     

**消息被拒绝**(basic.reject或basic.nack)并且requeue=false.

### 4.3 实现

#### 4.3.1 架构图

<img src="C:\Users\大梦\AppData\Roaming\Typora\typora-user-images\image-20210721021156465.png" alt="image-20210721021156465" style="zoom: 67%;" />

#### 4.3.2 代码实现



**c1实现**

```java
public class Consumer01 {

    public static final String NORMAL_EXCHANGE = "normal_exchange";
    public static final String DEAD_EXCHANGE = "dead_exchange";

    public static void main(String[] args) throws IOException, TimeoutException {

        Channel channel = RabbitmqUtil.getChannel();

        //声明交换机
        channel.exchangeDeclare(NORMAL_EXCHANGE, BuiltinExchangeType.DIRECT);
        channel.exchangeDeclare(DEAD_EXCHANGE, BuiltinExchangeType.DIRECT);

        //绑定普通队列的信息传输到那个死信队列
        Map<String, Object> map = new HashMap<>();
        map.put("x-dead-letter-exchange", DEAD_EXCHANGE);
        map.put("x-dead-letter-routing-key", "lisi");
        //声明普通队列
        String normalQueue = "normal-queue";
        channel.queueDeclare(normalQueue, false, false, false, map);

        //声明死信队列
        String deadQueue = "dead-queue";
        channel.queueDeclare(deadQueue, false, false, false, null);

        //绑定普通交换机
        channel.queueBind(normalQueue, NORMAL_EXCHANGE, "zhangsan");

        //绑定死信交换机
        channel.queueBind(deadQueue, DEAD_EXCHANGE, "lisi");

        DeliverCallback deliverCallback = (targetId, message) -> {
            System.out.println("接收到消息" + new String(message.getBody(), "UTF-8"));
        };
        //接收消息
        channel.basicConsume(normalQueue, true, deliverCallback, (cal) -> {
        });
    }

}
```

**producer的实现**

```java
public class Producer {

    public static final String NORMAL_EXCHANGE = "normal_exchange";

    public static void main(String[] args) throws IOException, TimeoutException {

        Channel channel = RabbitmqUtil.getChannel();
        //设置消息过期时间
        AMQP.BasicProperties properties = new AMQP
                .BasicProperties()
                .builder().expiration("10000").build();

        for (int i = 0; i < 11; i++) {
            String s = "info" + i;
            channel.basicPublish(NORMAL_EXCHANGE, "zhangsan", properties, s.getBytes("UTF-8"));
        }
    }
}
```

