# Console Producer, Consumer with TLS Encryption

Follow the high level steps below to create a MSK cluster, topic, send/receive data via. a TLS broker connection

## Cluster Creation

1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster-tls&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster_TLS.yaml)

## EC2 post-Deploymnet Configuration

1. SSH into the Ec2 instance created by the CloudFormation stack. *You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop*

2. Install required dependencies

```sudo apt-get update```

```sudo apt install default-jdk```

```wget https://archive.apache.org/dist/kafka/2.6.2/kafka_2.12-2.6.2.tgz```

```tar -xzf kafka_2.12-2.6.2.tgz```

```cd kafka_2.12-2.6.2```

3. View the MSK cluster's client information. Replace/run *<TLS-boot-strap-server>* with the Bootstrap servers, TLS, Private endpoint value

```export MYTLSBROKERS=<TLS-boot-strap-server>```

## Configure Client TLS
 
To send / receive data via. TLS we need to create a client.properties file and set up a SSL truststore 

 1. Set up a SSL truststore via. the clients JVM truststore
 
```mkdir /home/ubuntu/tmp/```
  
```cp /usr/lib/jvm/java-1.11.0-openjdk-amd64/lib/security/cacerts /home/ubuntu/tmp/kafka.client.truststore.jks```

 2. Create client.properties
 
Create a ```/home/ubuntu/tmp/client.properties``` file and place the code below inside

```
security.protocol=SSL
ssl.truststore.location=/tmp/kafka.client.truststore.jks
```
 
## Create a Topic
  
1. Create a kafka topic
  
```bin/kafka-topics.sh --bootstrap-server $MYTLSBROKERS --create --topic TLSTestTopic --partitions 3 --replication-factor 3```

## Console Consumer
  
```bin/kafka-console-consumer.sh --bootstrap-server $MYTLSBROKERS --consumer.config /home/ubuntu/tmp/client.properties --topic TLSTestTopic``` 
 
Leave this window open. When data is sent from the console producer it will appear in this window
 
## Console Producer

In a new SSH / EC2 terminal session 
 
```bin/kafka-console-producer.sh --broker-list $MYTLSBROKERS --producer.config /home/ubuntu/tmp/client.properties --topic TLSTestTopic```

Type messages and the [ENTER] to send them. You will see the messages appear in the console consumer window
