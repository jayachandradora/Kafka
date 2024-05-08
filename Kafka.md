
Kafka is a distrbuted message streaming platform that uses publish subscribe mechanism to stream the records(data flow).

Message streaming, also known as event streaming or stream processing, refers to the practice of continuously processing and transmitting data records (messages) in real-time. 
Message streaming is commonly used in various applications and industries, including:

-  **IoT (Internet of Things)**: Collecting and processing sensor data from IoT devices to monitor and control physical environments.
-  **Finance**: Analyzing real-time market data for trading, risk management, and fraud detection.
-  **Social Media**: Processing user interactions and content updates on social media platforms.
-  **E-commerce**: Analyzing user behavior and transactions to personalize recommendations and detect anomalies.
-  **Telecommunications**: Processing network data for monitoring, troubleshooting, and optimizing network performance.
-  **Healthcare**: Analyzing patient data for remote monitoring, disease surveillance, and predictive analytics.

Message streaming systems often use distributed architectures and technologies to handle large volumes of data and ensure scalability, fault tolerance, and low latency. Examples of popular message streaming platforms include Apache Kafka, Amazon Kinesis, Apache Pulsar, and Google Cloud Pub/Sub.

Point-to-point (P2P) and publish-subscribe (pub-sub) are two fundamental messaging paradigms used in distributed systems for communication between components. Here's an overview of each:

###  Point-to-Point Messaging System (P2P):
In a point-to-point messaging system, also known as queuing, messages are sent from one producer to exactly one consumer. The messaging system typically involves the following components:

*  Queue: messages are persisted in queue. A message queue acts as an intermediary storage mechanism where messages are placed by the producer and consumed by the consumer(s). Each message is consumed by only one consumer.
*  Producer: A producer is responsible for sending messages to the message queue.
*  Consumer: A consumer retrieves messages from the message queue and processes them. Once a message is consumed, it is typically removed from the queue.
P2P messaging systems are often used in scenarios where each message must be processed by a single recipient, such as task distribution, workload balancing, or job scheduling.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/2663a2ff-b566-4051-a814-c88d2ac101e4)


###  Publish-Subscribe Messaging System (Pub-Sub):
In a publish-subscribe messaging system, messages are broadcasted from a producer (publisher) to multiple consumers (subscribers) who have subscribed to receive specific types of messages. The messaging system typically involves the following components:

*  Topic: A topic is a channel or category to which messages are published. Publishers send messages to specific topics, and subscribers receive messages from topics they have subscribed to.
*  Publisher (Producer): A publisher is responsible for sending messages to one or more topics.
*  Subscriber (Consumer): A subscriber expresses interest in one or more topics and receives messages published to those topics.

Pub-sub messaging systems allow for decoupling between publishers and subscribers, enabling a more flexible and scalable communication model. Subscribers can dynamically subscribe to topics of interest without needing to know about the publishers, and publishers can broadcast messages without needing to know about the subscribers.

Pub-sub messaging systems are commonly used in scenarios such as event-driven architectures, real-time data processing, and message broadcasting.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/18025bfa-44bf-4eaf-a556-971e32c1c517)


In summary, point-to-point messaging systems are designed for one-to-one communication, while publish-subscribe messaging systems are designed for one-to-many communication.

##  Kafka Architecture


![image](https://github.com/jayachandradora/Kafka/assets/115500959/778eba44-985a-4655-b540-90feda1d2935)

###  Topics:

*  A stream of messages belonging to a particular category is called a topic.
*  it is a logical feed name to which messages are published.
*  similar to table in a database.
*  unique identifier of a topic is its name.
*  we can create as many topic as we want.
*  Decoupling: Topics facilitate decoupling between publishers and subscribers. Publishers can publish messages to topics without needing to know the identities or locations of subscribers, and subscribers can receive messages from topics without needing to know the identities or locations of publishers.
*  Scalability: Topics enable scalability by allowing multiple publishers to send messages to the same topic and multiple subscribers to receive messages from the same topic concurrently.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/30f36f0f-c2a8-4d0e-a244-165a29d69412)

###	partition:
*	Each topic in Kafka is divided into one or more partitions.
*	Each partition is an ordered, immutable sequence of messages.
*	Partitions enable Kafka to horizontally scale both storage and throughput by distributing data across multiple brokers
*	Each message within the partition has a unique id associated known as offset

####  Here are some key characteristics of partitions in Kafka:

*	Ordered Sequence: Messages within a partition are strictly ordered by their offset, which is a unique identifier assigned to each message as it is appended to the partition. The offset starts from zero for the first message in the partition and increments monotonically for each subsequent message.
*	Replication: Partitions can be replicated across multiple brokers for fault tolerance and high availability. Each partition has one leader and zero or more replicas. The leader is responsible for handling all read and write requests for the partition, while replicas serve as backups that can take over as leaders in case of leader failure.
*	Scalability: By partitioning topics into multiple partitions, Kafka can distribute the data and processing load across multiple brokers and consumers, allowing for horizontal scalability.
*	Consumer Group Parallelism: Consumers within a consumer group can parallelize message processing by assigning different partitions to different consumer instances within the group. Each message in a partition is consumed by only one consumer instance, ensuring that messages within the same partition are processed in order.
*	Retention Policy: Each partition has its own configurable retention policy, which determines how long Kafka retains messages in the partition before they are eligible for deletion. Retention policies can be based on time or size, allowing for flexible data retention strategies.
*	Leader Election: Kafka uses leader election mechanisms to dynamically assign leaders for each partition, ensuring that leadership is evenly distributed across brokers and partitions.

Overall, partitions are a critical component of Kafka's distributed architecture, enabling high throughput, fault tolerance, scalability, and efficient message processing.

Distribution of the partions among brokers happens in round robin fashion
![image](https://github.com/jayachandradora/Kafka/assets/115500959/32b1093d-bd21-4ce4-93b1-6aaf0f2a8905)

![image](https://github.com/jayachandradora/Kafka/assets/115500959/ba007dd8-1149-476d-9642-acea50f624fb)

### Replica amd Replication

![image](https://github.com/jayachandradora/Kafka/assets/115500959/aa3cd22b-5a3d-47a6-998d-750ae643faec)

![image](https://github.com/jayachandradora/Kafka/assets/115500959/93a91146-7cbe-4e39-aaab-33255cdb2fe0)

Process of replicating the partition is called replication.

In Apache Kafka, a replica refers to a copy of a partition that is stored on a different broker than the partition's leader replica. Replication is a key feature of Kafka and provides fault tolerance, high availability, and durability of data.

Here are the main points about replicas in Kafka:

*	Leader and Follower Replicas: Each partition in Kafka has one leader replica and zero or more follower replicas. The leader replica is responsible for handling all read and write requests for the partition. Follower replicas are passive copies of the partition that replicate data from the leader.
*	Replication Factor: The replication factor is the number of replicas maintained for each partition. When a topic is created, you specify the replication factor, which determines how many copies of each partition are created and distributed across brokers. The minimum replication factor is 1, meaning that there is only one copy of each partition (no replication). However, it is common to have a replication factor of at least 2 or 3 for fault tolerance.
*	In-Sync Replicas (ISR): Follower replicas that are fully caught up with the leader replica and are in-sync with the latest messages are known as in-sync replicas (ISR). Kafka guarantees durability and consistency by ensuring that all writes are first committed to the leader replica and then replicated to all ISR replicas before acknowledging the write to the producer.
*	Leader Election: Kafka dynamically assigns leader replicas for each partition and handles leader election in case of leader failure. If the leader replica fails, one of the ISR replicas is elected as the new leader. This process ensures continuous availability and fault tolerance.
*	Replica Lag: Follower replicas may lag behind the leader replica due to factors such as network latency or broker overload. Replica lag refers to the difference in the position of the follower replica's log compared to the leader replica's log. Monitoring and managing replica lag is important for maintaining consistency and performance in Kafka clusters.
*	Data Distribution and Load Balancing: Kafka brokers use replicas to distribute data and load across the cluster. By replicating partitions across multiple brokers, Kafka ensures fault tolerance and scalability while balancing data and processing load.





![image](https://github.com/jayachandradora/Kafka/assets/115500959/7be37c8e-0e18-4fb5-8ebf-fb6b3c192070)


![image](https://github.com/jayachandradora/Kafka/assets/115500959/355573c2-b2e0-4a72-a589-4d7f2b06058d)

Note : Topic also type of queue with some extra features.
![image](https://github.com/jayachandradora/Kafka/assets/115500959/e3ca64b5-80fd-44ce-ba0d-60660d6652f3)


###  Centralized System:

- In a centralized system, all the data and processing are managed in one central location or node.

**Advantages:**

- Simplicity: Easier to manage and maintain since everything is in one place.
- Control: Centralized control makes it easier to enforce policies and monitor activities.

**Disadvantages:**

- Single Point of Failure: If the central node fails, the entire system could go down.
- Scalability Issues: Scaling a centralized system can be challenging, especially as the volume of data or the number of users grows.

###  Distributed System:

- In a distributed system, data and processing are spread across multiple nodes or locations.
- In the context of Kafka, a distributed system might involve multiple Kafka clusters spread across different geographical regions.

**Advantages:**

- Fault Tolerance: Distributed systems are more resilient to failures since data and processing are distributed across multiple nodes.
- Scalability: Easier to scale by adding more nodes or clusters as needed to handle increased load.
- Performance: Distributed processing can often be faster since tasks can be performed in parallel across multiple nodes.

**Disadvantages:**

- Complexity: Distributed systems are generally more complex to design, implement, and manage compared to centralized systems.
- Consistency Challenges: Ensuring consistency across distributed systems can be tricky and may require additional mechanisms.

In summary, while centralized systems offer simplicity and centralized control, distributed systems provide better fault tolerance, scalability, and performance at the cost of increased complexity. In the context of Kafka, choosing between a centralized or distributed setup depends on factors like the scale of data processing, fault tolerance requirements, and anticipated growth.

