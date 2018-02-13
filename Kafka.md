# Design
[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KAFKA-Design.png]]

# Kafka Ecosystem
## Producers
Producers publish data to the topics of their choice. The producer is responsible for choosing which record to assign to which partition within the topic.[Kafka](http://kafka.apache.org/documentation.html#producerapi) HELK currently works with machines (producers) running Winlogbeat as their log shipper. Most labs might have **Winlogbeat** installed on all the endpoints and sending data to the HELK directly. However, it is recommended, for scalability reasons, to use WEF servers to collect your windows logs first. Then, you can have Winlogbeat or NXlog installed on your WEF servers to ship the logs to your HELK. **Using WEF servers has not been tested yet with the HELK. This is coming soon..!**

The following Winlogbeat config is recommended:
```
winlogbeat.event_logs:
  - name: Application
    ignore_older: 30m
  - name: Security
    ignore_older: 30m
  - name: System
    ignore_older: 30m
  - name: Microsoft-windows-sysmon/operational
    ignore_older: 30m
  - name: Microsoft-windows-PowerShell/Operational
    ignore_older: 30m
    event_id: 4103, 4104
  - name: Windows PowerShell
    event_id: 400,600
    ignore_older: 30m
  - name: Microsoft-Windows-WMI-Activity/Operational
    event_id: 5857,5858,5859,5860,5861

output.kafka:
  hosts: ["<HELK-IP>:9092","<HELK-IP>:9093","<HELK-IP>:9094"]
  topic: "winlogbeat"
  max_retries: 2
  max_message_bytes: 1000000
```
You can check how your logs are being sent to the HELK by running the following command in your systems (producers):
```
winlogbeat.exe -e
```
[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KAFKA-producer1.png]]
[[https://github.com/Cyb3rWard0g/HELK/raw/master/resources/images/KAFKA-producer2.png]]

Producers send data directly to the broker that is the leader for the partition without any intervening routing tier. To help the producer do this all Kafka nodes can answer a request for metadata about which servers are alive and where the leaders for the partitions of a topic are at any given time to allow the producer to appropriately direct its requests.

## Kafka Cluster (Brokers)
HELK uses a kafka cluster conformed of 3 brokers. Each broker has its own ID number and topic log partitions. Connecting to one broker bootstraps a client to the entire Kafka cluster.

Each HELK broker has the following custom configurations:

| Name | Description | Type | Value |
|--------|---------|-------|-------|
| broker.id | The broker id for this server. If unset, a unique broker id will be generated.To avoid conflicts between zookeeper generated broker id's and user configured broker id's, generated broker ids start from reserved.broker.max.id + 1. | int | 0,1,2 |
| listeners | Listener List - Comma-separated list of URIs we will listen on and the listener names. If the listener name is not a security protocol, listener.security.protocol.map must also be set. Specify hostname as 0.0.0.0 to bind to all interfaces. Leave hostname empty to bind to default interface | string | PLAINTEXT://:9092 PLAINTEXT://:9093 PLAINTEXT://:9094 |
| advertised.listeners | Listeners to publish to ZooKeeper for clients to use, if different than the `listeners` config property. In IaaS environments, this may need to be different from the interface to which the broker binds. | string | PLAINTEXT://HELKIP:9092 PLAINTEXT://HELKIP:9093 PLAINTEXT://HELKIP:9094 |
| log.dirs | The directories in which the log data is kept. If not set, the value in log.dir is used | string | /tmp/kafka-logs /tmp/kafka-logs-1 /tmp/kafka-logs-2|
| auto.create.topics.enable | Enable auto creation of topic on the server | boolean | false |
| num.replica.fetchers | Number of fetcher threads used to replicate messages from a source broker. Increasing this value can increase the degree of I/O parallelism in the follower broker. | int | 2 |
| unclean.leader.election.enable | Indicates whether to enable replicas not in the ISR set to be elected as leader as a last resort, even though doing so may result in data loss | boolean | true |
| num.recovery.threads.per.data.dir | The number of threads per data directory to be used for log recovery at startup and flushing at shutdown | int | 1 |

## Zookeeper
Kafka needs ZooKeeper to work efficiently in the cluster. Kafka uses Zookeeper to do leadership election of Kafka Broker and Topic Partition pairs. Kafka uses Zookeeper to manage service discovery for Kafka Brokers that form the cluster. Zookeeper sends changes of the topology to Kafka, so each node in the cluster knows when a new broker joined, a Broker died, a topic was removed or a topic was added, etc. Zookeeper provides an in-sync view of Kafka Cluster configuration.[Cloudurable](http://cloudurable.com/blog/kafka-architecture/index.html)