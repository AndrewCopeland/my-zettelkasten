# 1596728190 rabbitmq-consumer-acknowledgements-publisher-confirms
#queue #rabbitmq #data #safety
https://www.rabbitmq.com/confirms.htm
This is related to data safety.

This makes sure that consumers and publishers actually are sending, recieveing and processing the message correctly.

Delivery processing acknowledgements from consumers to RabbitMQ are known as `acknowledgements` in messaging protocols.
Broker acknowledgements to publishers are a protocol extensions called `publisher confirms`.

Both of these are essential for reliable delivery both from the publishers to RabbitMQ nodes and from the RabbitMQ notes to consumers. They are essential for data safety.

## Delivery Acknowledgements (Consumer)




## Links
