# Synchronous vs. asynchronous processes
**Synchronous processes** run one after the other, and each task must be 100% complete before the next operation begins. 

**Asynchronous (or async) processes** can work independently and in parallel. In other words, you don't have to wait for task A to be 100% complete before beginning task B. 
# How it works
## Batch processing
Commonly used in applications where large amounts of data are processed regularly for periodic review. Corporate finance and accounting software would be well-suited to batch processing.

Perhaps the most popular implementation of this is **MapReduce,** a model for batch processing introduced by Google in 2004. It's a simple algorithm:

![](<../Attachments/Pasted image 20261104125505.png>)

The reduced input is automatically sorted and stored for easy access.

Note that MapReduce won't work as well for data that's not repetitive, as it won't condense down (reduce) as much.

## Stream processing
With stream processing, data flows into your system as **events** occur.

Stream processing will get you close-to-real time results which is valuable in applications where accuracy is critical such as a trading platform. These benefits are offset by complexity and fragility, however. **Check-pointing** can help alleviate problems with system outages, but you'll have to give careful consideration to checkpoint frequency. Frequent checkpoints mean faster recovery, but lower performance.

## Lambda architecture
Lambda architecture can bridge the gap between durable-yet-delayed batch processing and fresh-yet-brittle stream processing by creating a "fast lane" for processing priority events, but this introduces operational complexity.

# Asynchronous queues
Queues make asynchronous processes more reliable and less brittle as events are captured and processed in an orderly way. **Task queues** are a type of **message queue**; sometimes they're built on top of message queues.

Generally:

- **Message queues**, which receive, log, and deliver messages, can be used to update users that jobs are being processed in the background, thus unblocking them and making for a better user experience. Redis and RabbitMQ are both popular choices.
- **Task queues** execute in addition to passing information. They schedule jobs, complete tasks, and report results. Celery is a popular choice.

**Publish/Subscribe (or pub/sub)** messaging is another async communication method to know. In pub/sub messaging, you have a _subscriber_ who receives a message sent by a _publisher_ via a _broker_. Because communication is decoupled, messages can be automatically pushed to all subscribers rather than pulled individually via a message queue. This method is popular in event-driven architecture because event-driven services can be delivered quickly and easily. Additionally, because publishers are isolated from subscribers, the system is easier to maintain and secure.

![](<../Attachments/Pasted image 20261104130113.png>)

# When to bring it up in an interview

Batch processing is useful when you need to process chunks of data in predictable intervals, as in accounting software. Stream processing is a good choice for applications like anomaly detection or sentiment analysis when timeliness is critical, and lambda architecture can help you capture the benefits of both if you're willing to deal with added complexity. Synchronous processing is still the best choice for simple applications where processing times are short and well-defined, and when errors must be dealt with immediately like in payment tools.

Take a look at the below table to help guide your decisions when prepping for your interview:

![](<../Attachments/Pasted image 20261104130344.png>)

### Further reading

- Read [this article](https://www.twilio.com/blog/2017/11/chaos-engineering-ratequeue-ha.html) from Twilio's engineering blog, which covers how engineering chaos tested an internally-developed queueing system called Ratequeue.