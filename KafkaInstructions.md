# Learning Kafka with Oreilly
My work with OReillys *Apache Kafka Series - Learn Apache Kafka for Beginners*

The result is test code in this project, kafka-beginners-cource. I also made "Production code" for listening to Twitter and putting to Kafka - and reading Kafka and putting to MongoDB. Those is in separate projects;
- Twitter to Kafka
- TweetsKafkaToMongo

Below is used - useful commands that I used during the class.  

By the way, the class was a really good intro and exersises to Kafka, Stéphane Maarek is pleasent to listen to and its made in 2018.

# Kafka på Kubernetes / mini1

For å bruke kafka command linje så lages en pod/ciontainer m klienten på ; 
    kubectl run my-kafka-client --restart='Never' --image docker.io/bitnami/kafka:2.6.0-debian-10-r0 --namespace default --command -- sleep infinity
    kubectl exec --tty -i my-kafka-client --namespace default -- bash
	cd /opt/bitnami/kafka/bin

     kafka-topics my-kafka-0.my-kafka-headless.default.svc.cluster.local:9092 \
            --topic test

# Kafka on RHEL1
Kafka installed with homebrew
Som der er installert på RHEL1
	'zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties & kafka-server-start /usr/local/etc/kafka/server.properties'
	
eller

	systemctl start zookeeper 

###  Kafka dirs
	/usr/local/etc/kafka 

# Kafka kommandoer / CI Interface
### Topic commands
	kafka-topics --zookeeper 127.0.0.1:2181 --topic tweets-topic --create --partitions 6 --replication-factor 1
	kafka-topics --zookeeper 127.0.0.1:2181 --list
	kafka-topics --zookeeper 127.0.0.1:2181 --topic first_topic --describe

### Delete topics
	kafka-topics --zookeeper 127.0.0.1:2181 --topic first-topic --delete

### Producer commands
	kafka-console-producer --broker-list 127.0.0.1:9092 --topic first_topic --producer-property acks=all


### Consumer commands 
	kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic 
	kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic --from-beginning
	kafka-consumer-groups  --bootstrap-server 127.0.0.1:9092 --list

# Consumer groups
	kafka-console-consumer --bootstrap-server 127.0.0.1:9092 --topic first_topic -group my-first-application
from-beginning on groups means from the ones not read yet

# Offsets and reset
	kafka-consumer-groups --bootstrap-server localhost:9092 --group my-first-applicaiton --reset-offsets --to-earliest --topic first_topic --execute 

# Settings for safe producer. Kafka >= 1.0
enable.idempotence=true (producer level) + min.insync.replicas = 2 (broker / topic level)
Implies acks=all, retries=maxint, max.inflight.per.connections=5
While keeping ordering and reasonable performance

## Before 1.0
cks=all On producer level. Ensures that data is distributed to replicas before ack is received. (Tanke, holder det ikke med 2?)
min.insync.replicas=2 broker/topic level Enshures two brokers in ISR at least have the data after an ack
max.inflight.retries.