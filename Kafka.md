
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

###	Producer

In Apache Kafka, a producer is an application or process that publishes messages to one or more topics in a Kafka cluster. Producers are responsible for generating data and sending it to Kafka brokers for storage and distribution to consumers.

producers can write data either on the topic level(write all the partitions of that topic in round robin fashion) or specific partitions of the topic.

###	Consumers

*  In Apache Kafka, a Consumers are applications which read/consume data from the topics within a cluster using consumer APIs.

*  Consumers can read data either on the topic level(all the partitions of that topic) or specific partitions of the topic.

*  Consumers are always associated with exactly one consumer group.

*  A consumer group is a group of related consumers that perform a task.
*  Consumer group is used to increase throughput

###	Broker

*	Brokers or kafka servers are simple software processes who maintain and manage the published messages.

*	Brokers are also manage the consumer-offsets and are responsible for the delivery of messages to the right consumers,
*	 A set of brokers who are communicating with each other to perform the menagement and maintenance task are collectively known as kafka cluster.
*	we can add more brokers in a already running kafka cluster without any downtime.

Here are the key points about Kafka brokers:

-	Storage: Brokers store messages that are produced by producers. Each broker hosts one or more partitions of Kafka topics. The actual data is stored on disk in the form of log segments, which are immutable sequences of messages.
-	Partition Leader: For each partition, one broker serves as the leader replica. The leader broker is responsible for handling all read and write requests for that partition, including message replication to follower replicas and serving consumer requests for data.
-	Replication: Kafka uses replication for fault tolerance and high availability. Each partition can have one leader replica and one or more follower replicas. Follower replicas replicate data from the leader replica, and they can take over as leaders if the current leader fails.
-	Communication: Brokers communicate with each other and with clients (producers and consumers) using the Kafka protocol over TCP/IP. Clients can connect to any broker in the cluster to produce or consume messages, and Kafka brokers coordinate to ensure data consistency and availability.
-	Partition Assignment: Kafka brokers participate in the process of partition assignment, which determines which broker serves as the leader for each partition and which brokers host the follower replicas. Partition assignment is dynamic and can change over time, especially in response to broker failures or cluster rebalancing.
-	Scalability: Kafka brokers can be horizontally scaled by adding more broker instances to the cluster. As more brokers are added, Kafka can distribute data and processing load across multiple nodes, enabling higher throughput and storage capacity.
-	Monitoring and Management: Kafka provides tools and APIs for monitoring and managing broker health, performance, and configuration. Administrators can monitor metrics such as disk usage, network throughput, and partition lag to ensure optimal cluster operation.

###	Zookeeper

*	Zookeeper is used to monitor kafka cluster and co-ordinate with each broker.
*	keeps all the metadate information related to kafka cluster in the form of key value pair.
metadata includes,
	*	configuration informations.
	*	Health status of each broker.
*	it is used for the controller election within kafka cluster.

###	Features of kafka:

Apache Kafka offers a rich set of features that make it a popular choice for building real-time streaming data pipelines, event-driven architectures, and distributed messaging systems. Here are some key features of Kafka:

1.	**Distributed Messaging**: Kafka is designed as a distributed system, allowing it to scale horizontally across multiple servers (brokers) to handle large volumes of data and high message throughput.
2.	**Fault Tolerance**: Kafka provides fault tolerance through data replication. Each message is replicated across multiple brokers, ensuring data durability and availability even in the event of broker failures.
3.	**High Throughput and Low Latency**: Kafka is optimized for high-throughput, low-latency message delivery. It can handle millions of messages per second and provides consistent performance even under heavy load.
4.	**Scalability**: Kafka scales easily by adding more brokers to the cluster. It supports dynamic partition rebalancing and leader election, allowing it to distribute data and processing load across the cluster efficiently.
5.	**Pub-Sub Messaging**: Kafka supports a publish-subscribe messaging model, allowing producers to publish messages to topics, and consumers to subscribe to topics and receive messages in real-time.
6.	**Streaming Data Processing**: Kafka includes a Streams API that enables real-time stream processing of data. Developers can build applications that consume, process, and transform data from Kafka topics in real-time, enabling a wide range of use cases such as real-time analytics, ETL (Extract, Transform, Load), and event-driven microservices.
7.	**Exactly-Once Semantics**: Kafka provides support for exactly-once message delivery semantics, ensuring that messages are processed exactly once, even in the presence of failures or retries. This feature is crucial for maintaining data consistency and integrity in mission-critical applications.
8.	**Retention and Compaction**: Kafka allows users to configure retention policies for topics, specifying how long messages should be retained in the log. It also supports log compaction, which retains only the latest value for each key in a topic, making it suitable for maintaining the current state of a dataset.
9.	**Security**: Kafka provides robust security features, including SSL/TLS encryption for data in transit, authentication and authorization mechanisms for controlling access to topics, and integration with external authentication providers such as LDAP or Kerberos.
10.	**Monitoring and Management**: Kafka includes tools and APIs for monitoring cluster health, performance, and resource utilization. It also provides administrative interfaces for managing topics, partitions, and configurations.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/4ac811a4-fedc-461d-bf20-4f0e9c610d5f)

![image](https://github.com/jayachandradora/Kafka/assets/115500959/ad646393-97c2-409c-a883-85d4745433e1)


In above Broker 1,2 and 3 are connected to zookeeper and broker id 1 is a controller node. There is only one controller node in any kafka cluster

![image](https://github.com/jayachandradora/Kafka/assets/115500959/eefb8207-5197-45b3-b98d-abb143f055da)


Note : once producer publish a message it produce in a partition which is in leader node and the follower node(broker) where replica of main partition is there send a fetch request and copy the message. this is how replicas are in sync(ISR).

###	States of partition and Replica

![image](https://github.com/jayachandradora/Kafka/assets/115500959/aea80336-6315-482a-9fb5-6ffacf54e2fa)

*	when the partition is in Newpartition state then read and write operation can not be happened because the leader is not yet assigned and client can not know which broker to be connected to write or read the message.
*	onlinepartition is the working state of the partition. in this state read and write operation can be happened.
*	once broker 1 down then partition 0 moves to offlinepartition state.

###	Kafka controller Noad

In kafka cluster , one of the brokers serves as the controller node
*	The controller manages the state of partitions and replicas.
*	The controller manages the state transition.
*	It perform administrative tasks like reassigning partitions.

**There is one controller node in a kafka cluster and we can not decrease the partition because of data loss**
### Partition Reassignment
1.	move the partition across the brokers.
2.	selectively move replicas of a partition to a specific set of brokers.
3.	increase the replication factor.

###	Internals of Kafka Producer and Offsets in Kafka 

####	Offsets

![image](https://github.com/jayachandradora/Kafka/assets/115500959/7889e7d3-2e62-4b58-93fb-44d0349aabec)


*	The record in the partitions are each assigned a sequential id number called the offset that uniquely identifies each record within the partition.
  Three variations of offsets.
  1.	Log-end offsets: offset of the last message written to a log/partition. Or how many messages present in a partition. in above diagram log-end offset of partition p0 is 3
  2.	Current offset: pointer to the last record that kafka has already sent to a consumer in the most recent poll.
  3.	committed offset: Marking an offset as consumed is called committed.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/73f8ede8-073d-4f90-aa1c-73699386c4db)

*	When the broker store the messages then it store in file system and while store the messages it store the metadata(information about the data/message) of the message
*	If key is null then the messages sent in round robin fashion between the partitions.
*	Timestamp will the time when the broker saved the message in filesystem.
 
![image](https://github.com/jayachandradora/Kafka/assets/115500959/0407e90f-c8d5-436c-b462-b25c26ca1fda)
 
**Current offset**: pointer to the last record that kafka has already sent to a consumer in the most recent poll. kafka cluster store how may messages sent to the consumer. it store the information of consumer group not consumer.EX: the current offset of cg1 is 3.

**committed offset**: Marking an offset as consumed is called committed. consumer send the commit request to kafka and kafka store the commiteed offset i.e how many message or offset processed by consumer.EX. if consumer processed the message a then committed offset will be 0.

**committed offset should not be more then Current offset.**

**autocommit: automatically sent to broker that messages are committed**

### Internals of Consumer Group

![image](https://github.com/jayachandradora/Kafka/assets/115500959/ddb38700-30e9-4a6f-9194-a8829e7c684e)

*	Kafka maintain that one partition can not assigned to two consumer in the same consumer group so that duplication will not be happend.
*	When another one consumer wants to subscribe the topic the kafka store the information in **consumer_offset** topic like, which consumer in the consumer group consume how many messages from which partition,  and assign the partition to that consumer accordingly not like the consumer consumes the messages from 0 or starting.
*	when new consumer wants to subscribe or join the topic then kafka block the message consumption. and rebalancing/redistribution happenes and assign the partition accordingly.

### Consumer Group Rebalancing

The process of re-distributing partitions within a consumer group is known as Consumer Group Rebalancing.
Rebalancing of Consumer group happens in below cases.
1.	A Consumer joining the consumer group.
2.	A Consumer leaving the consumer group.
3.	If partitions are added to the topics which these consumers are interested in.
4.	If a partition goes in offline state.(when broker is down)

###	How Consumer Group Rebalancing will happens.

For this two entities are involved
1.	 Group coordinator.
2.	 Group Leader

If there are 10 consumer groups and 3 Brokers in kafka cluster and we devide 3,3 and 4 consumer groups and assigned to these 3 brokers. then these consumer groups are devided into 3 subset and it is managed by 3 brokers.

![image](https://github.com/jayachandradora/Kafka/assets/115500959/70d79602-a21f-4a9c-b339-fe380c16a74b)

![image](https://github.com/jayachandradora/Kafka/assets/115500959/19dc9816-50f3-439d-a3e3-b8d5399250ac)


![image](https://github.com/jayachandradora/Kafka/assets/115500959/45cc952d-abf8-4494-b87f-1583deb90dd0)

**FindCoordinator, joinGroup etc are API calls.**

###	Internals of Topics, Partitions and Replications. 



![image](https://github.com/jayachandradora/Kafka/assets/115500959/198e2854-553a-4486-8f8b-3ebc4ab4b79f)

  
![image](https://github.com/jayachandradora/Kafka/assets/115500959/7be37c8e-0e18-4fb5-8ebf-fb6b3c192070)


![image](https://github.com/jayachandradora/Kafka/assets/115500959/355573c2-b2e0-4a72-a589-4d7f2b06058d)

Note : Topic also type of queue with some extra features.One node cluster and one node Zookeepers
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

