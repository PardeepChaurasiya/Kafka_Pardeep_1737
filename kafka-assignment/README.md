## Producer.java

### To produce message as object
Add your message in ProducerRecord
```java
for (int i = 1; i <= 10; i++){
    User user=new User(i,"pardeep",24,"mca");
    kafkaProducer.send(new ProducerRecord("pardeep-topic",String.valueOf(user.getId()),user));
}
```
Here `pardeep-topic` is name of ***topic*** & `user` is object of **User** class

## Consumer.java
### To see consuming messages
Make sure you subscribed to the topic `pardeep-topic`
```java
List topics = new ArrayList();
topics.add("pardeep-topic");
kafkaConsumer.subscribe(topics);
```
### Writing Data into a file when consuming it
```java

while (true) {
        FileWriter fileWriter = new FileWriter("pardeep-json-kafka-data.txt", true);
        ConsumerRecords<String, User> consumerRecords = kafkaConsumer.poll(Duration.ofSeconds(1));
        for (ConsumerRecord<String, User> consumerRecord : consumerRecords) {
            System.out.printf(
                "Topic: %s, Partition: %d, Value: %s%n",
                consumerRecord.topic(),
                consumerRecord.partition(),
                consumerRecord.value().toString());
            fileWriter.write(consumerRecord.value().toString() + "\n");
        }
        fileWriter.flush();
        fileWriter.close();
}
```
### Then
Just run the `Consumer.java` file. A terminal will open and there you can see your all messages from beginning produced by Producer.

### In pardeep-json-kafka-data.txt
> {"id":1, "name":"pardeep", "age":24, "course":"mca"}
> 
> {"id":2, "name":"pardeep", "age":24, "course":"mca"}
> 
> {"id":3, "name":"pardeep", "age":24, "course":"mca"}
> 
> {"id":4, "name":"pardeep", "age":24, "course":"mca"}
> 
> {"id":5, "name":"pardeep", "age":24, "course":"mca"}
> 

