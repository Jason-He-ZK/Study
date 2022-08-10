### RabbitMQ

direct：一对一，routing key = binding key

fanout：一对所有绑定的，

topic：一对多匹配的，#匹配 0 个或多个，*匹配一个

**send**

创建模块时选择 RabbitMQ

```
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

```
@Configuration
public class RabbitMQConfig {
    // 发送方只需要交换机
    @Bean
    public DirectExchange directExchange() {
        return new DirectExchange("directExchange");
    }
    @Bean
    public FanoutExchange fanoutExchange(){
        return new FanoutExchange("fanoutExchange");
    }
    @Bean
    public TopicExchange topicExchange(){
        return new TopicExchange("topicExchange");
    }
}
```

```
@Service("sendService")
public class SendServiceImpl implements SendService {
    @Resource
    private AmqpTemplate amqpTemplate;

    public void sendDirectMessage(String message) {
        amqpTemplate.convertAndSend("directExchange","directRoutingKey",message);
    }
    public void sendFanoutMessage(String message) {
        amqpTemplate.convertAndSend("fanoutExchange","",message);
    }
    public void sendTopicMessage(String message) {
        amqpTemplate.convertAndSend("topicExchange","topic.routingKey",message);
    }
}
```

```
public static void main(String[] args) {
    ApplicationContext ac=SpringApplication.run(ReciverApplication.class, args);
    SendService service= (SendService) ac.getBean("sendService");
    service.sendDirectMessage("sendDirectMessage");
    service.sendFanoutMessage("sendFanoutMessage");
    service.sendTopicMessage("sendTopicMessage");
}
```

receiver

配置文件：同 send

配置类

```
@Configuration
public class RabbitMQConfig {
    @Bean
    public DirectExchange directExchange() {
        return new DirectExchange("directExchange");
    }
    @Bean
    public FanoutExchange fanoutExchange(){
        return new FanoutExchange("fanoutExchange");
    }
    @Bean
    public TopicExchange topicExchange(){
        return new TopicExchange("topicExchange");
    }

    @Bean
    public Queue directQueue() {
        return new Queue("directQueue");
    }
    @Bean
    public Queue fanoutQueue() {
        return new Queue("fanoutQueue");
    }
    @Bean
    public Queue topicQueue() {
        return new Queue("topicQueue");
    }

    @Bean
    public Binding directBinding(DirectExchange directExchange, Queue directQueue) {
        return BindingBuilder.bind(directQueue).to(directExchange).with("directRoutingKey");
    }
    @Bean
    public Binding fanoutBinding(FanoutExchange fanoutExchange, Queue fanoutQueue) {
        return BindingBuilder.bind(fanoutQueue).to(fanoutExchange);
    }
    @Bean
    public Binding topicBinding(TopicExchange topicExchange, Queue topicQueue) {
        return BindingBuilder.bind(topicQueue).to(topicExchange).with("topic.#");
    }
}
```

```
@Service("receiveService")
public class ReceiveServiceImpl implements ReceiveService {
    @Resource
    private AmqpTemplate amqpTemplate;

    @RabbitListener(queues = {"directQueue"})
    public void directReceive(String message){
        System.out.println("directReceive----"+message);
    }
    @RabbitListener(queues = {"fanoutQueue"})
    public void fanoutReceive(String message) {
        System.out.println("fanoutReceive----"+message);
    }
    @RabbitListener(queues = {"topicQueue"})
    public void topicReceive(String message) {
        System.out.println("topicReceive----"+message);
    }
}
```

```
public static void main(String[] args) {
    ApplicationContext ac=SpringApplication.run(SendApplication.class, args);
}
```
