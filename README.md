## Components of Kafka
### Producer
- Producers are applications or services that publish (write) messages into Kafka topics.
- They decide which topic and partition the message goes to, either randomly, in round-robin fashion or based on key.

### Consumer
- Consumers are applications that subscribe (read) messages from Kafka topics.
- Consumers exist in consumer groups to share the load of message consumption.

### Topic
- A topic is like a logical channel or category where messages are stored.
- Producers write messages into topics, and consumers read from them.

### Partition
- Topics are split into partitions to allow parallelism and scalability.
- Each partition is an ordered, immutable log of records.
- Messages inside partitions are identified by a unique offset.

### Broker
- A broker is a Kafka server that stores and serves messages.
- Acting as a central hub, the broker accepts messages from producers, assigns them unique offsets, and stores them securely on disk.

### Cluster
- A Kafka cluster is a group of brokers working together.
- It ensures data replication, fault tolerance, and high availability.

### Offset
- An offset is a unique ID assigned to each message in a partition.
- It helps consumers keep track of which messages have been read.

#### Kafka Architecture Diagrams
![kafka_architecture.png](docs/images/kafka_architecture.png)

#### Kafka Partitions Diagram
![kafka_partitions.png](docs/images/kafka_partitions.png)

#### Kafka IN-SYNC replicas
![kafka_in_sync_replicas.png](docs/images/kafka_in_sync_replicas.png)


## Comsumer Groups
A consumer group is a set of consumers that work together to process a topic.
- Each partition is consumed by exactly one consumer in the group.
- If you have more consumers than partitions, some consumers will stay idle.
- If you have fewer consumers than partitions, some consumers will handle multiple partitions.
  This design provides automatic load balancing. Kafka ensures that partitions are evenly distributed among available consumers in the group.
  ![img.png](docs/images/comsumer_groups.png)
### Number of Consumers < Number of Partitions
- In the diagram, the green consumer is connected to both partitions (p0 and p1).
- This happens because there are fewer consumers than partitions, so one consumer must handle multiple partitions.
- For example, if we have 2 partitions but only 1 consumer, that single consumer will read from both p0 and p1.
### Number of Consumers = Number of Partitions
- In the bottom-right blue box, we see two consumers.
- One consumer is assigned p0, and the other is assigned p1.
- Kafka ensures that each partition is consumed by exactly one consumer in the group.
- The producer decides which partition a message goes to:
    - If a key is provided, Kafka uses hashing of the key to determine the partition.
    - If the key is null, messages are distributed in a round-robin fashion.
### Number of Consumers > Number of Partitions
- At the top, the orange box shows multiple consumers, but only two of them are actually consuming.
- The rest are marked as idle.
- This happens because a partition cannot be consumed by more than one consumer in the same group.
- So, extra consumers simply remain idle and do not get any data.