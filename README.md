## About

Just an experimental project using the Twitter Source connector to produce tweets as messages.

## Getting started

Fill out the values in `twitter.properties.example` with your Twitter API keys/values
```bash
name=TwitterSourceConnector
tasks.max=1
connector.class=com.github.jcustenborder.kafka.connect.twitter.TwitterSourceConnector

# Set these required values
twitter.oauth.accessTokenSecret=
process.deletes=
filter.keywords=
kafka.status.topic=
kafka.delete.topic=
twitter.oauth.consumerSecret=
twitter.oauth.accessToken=
twitter.oauth.consumerKey=
```

Move the file over 

Start the kafka server & zookeeper in (separate windows)

```bash
# Kafka server
❯ kafka-server-start config/server.properties

# Zookeeper
❯ zookeeper-server-start.sh config/zookeeper.properties
```

```bash
❯ mv twitter.properies.example twitter.properies
```

Create status & delete topics

```bash
# Status topic
❯ kafka-topics --zookeeper localhost:2181 --create --topic <your-topic-status-name> --partitions 3 --replication-factor 1
Created topic <your-topic-status-name>.

# Delete topic
❯ kafka-topics --zookeeper localhost:2181 --create --topic <your-topic-delete-name> --partitions 3 --replication-factor 1
Created topic <your-topic-delete-name>.
```

Optional: Check your topic was created with the right partitions & replication factor

```bash
❯ kafka-topics --zookeeper localhost:2181 --topic <your-topic-name> --describe
Topic: <your-topic-name>	TopicId: KfCandFCTbCaGEwraEPq3Q	PartitionCount: 3	ReplicationFactor: 1	Configs:
	Topic: <your-topic-name>	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
	Topic: <your-topic-name>	Partition: 1	Leader: 0	Replicas: 0	Isr: 0
	Topic: <your-topic-name>	Partition: 2	Leader: 0	Replicas: 0	Isr: 0
```