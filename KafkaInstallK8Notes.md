# Intro
https://argus-sec.com/external-communication-with-apache-kafka-deployed-in-kubernetes-cluster/
https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport

# Working with
For å bruke kafka command linje så lages en pod/ciontainer m klienten på ; 
    kubectl run my-kafka-client --restart='Never' --image docker.io/bitnami/kafka:2.6.0-debian-10-r0 --namespace default --command -- sleep infinity
    kubectl exec --tty -i my-kafka-client --namespace default -- bash
	cd /opt/bitnami/kafka/bin

     kafka-topics my-kafka-0.my-kafka-headless.default.svc.cluster.local:9092 --topic test


# Configure extarnal access?
Kjørt denne; 
kubectl create -f Kafka-NodePort.yaml 

Får ikke tilgang fra vscode plugin

# Installing
Installed on mini1 mminikube-docap kluster

´mini1-2:~ tores$ helm install my-kafka bitnami/kafka´

NAME: my-kafka
LAST DEPLOYED: Thu Aug 13 18:47:25 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
** Please be patient while the chart is being deployed **

Kafka can be accessed by consumers via port 9092 on the following DNS name from within your cluster:

    my-kafka.default.svc.cluster.local

Each Kafka broker can be accessed by producers via port 9092 on the following DNS name(s) from within your cluster:

    my-kafka-0.my-kafka-headless.default.svc.cluster.local:9092

To create a pod that you can use as a Kafka client run the following commands:

    kubectl run my-kafka-client --restart='Never' --image docker.io/bitnami/kafka:2.6.0-debian-10-r0 --namespace default --command -- sleep infinity
    kubectl exec --tty -i my-kafka-client --namespace default -- bash

    PRODUCER:
        kafka-console-producer.sh \
            
            --broker-list my-kafka-0.my-kafka-headless.default.svc.cluster.local:9092 \
            --topic test

    CONSUMER:
        kafka-console-consumer.sh \
            
            --bootstrap-server my-kafka.default.svc.cluster.local:9092 \
            --topic test \
            --from-beginning