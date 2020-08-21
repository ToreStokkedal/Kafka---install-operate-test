# Kafka---install-operate-test
Repository to install, operate and test Kafka, on Kubenetes. Will also contain sample code, references etc


# Install on Kubernetes
- Kafka Installert uten feilmelding
- Nås fra klient internt i clusteret på gitt adresse
- Nås IKKE utenfra, må eksponere listener på alle pods ut

## Instllasjon
´mini1-2:~ tores$ helm install my-kafka bitnami/kafka´

## 


# Expose Kafka outside cluster.

Article found here https://argus-sec.com/external-communication-with-apache-kafka-deployed-in-kubernetes-cluster/

It points at LoadBalancer as soluiton. We may use Ingress for HTTP/REST, but ....
We may use NodePort, but ....

So we will test a ading a load *balancer* in mini1