# Console Producer, Consumer

Follow the high level steps below to create a MSK cluster, topic, send/receive data

## Cluster Creation

1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster-plaintext&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster_Plaintext.yaml)

The CloudFormation stack will deploy the architecture below with the EncryptionInTransit, ClientBroker property set to PLAINTEXT

<img width="700" alt="OpenSearch_demo_Architecture" src="https://github.com/ev2900/Managed_Streaming_for_Apache_Kafka_Examples/blob/main/Cluster_Expansion/ReadMe_Architecture/architecture.png">

## EC2 post-Deploymnet Configuration

1. SSH into the Ec2 instance created by the CloudFormation stack. *You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop*

2. Install required dependencies

```sudo apt-get update```

```sudo apt install default-jdk```

```wget https://archive.apache.org/dist/kafka/2.6.2/kafka_2.12-2.6.2.tgz```

```tar -xzf kafka_2.12-2.6.2.tgz```

```cd kafka_2.12-2.6.2```

3. View the MSK cluster's client information. Replace/run *<plaintext-boot-strap-server>* with the Bootstrap servers, Plaintext, Private endpoint value

  ```export MYPLAINTEXTBROKERS=<plaintext-boot-strap-server>```
  
## Create a Topic
  
1. Create a kafka topic
  
```bin/kafka-topics.sh --bootstrap-server $MYPLAINTEXTBROKERS --create --topic PlainTextTestTopic --partitions 3 --replication-factor 3```
  
## Console Consumer
  
```bin/kafka-console-consumer.sh --bootstrap-server $MYPLAINTEXTBROKERS --topic PlainTextTestTopic``` 
 
Leave this window open. When data is sent from the console producer it will appear in this window
 
## Console Producer
In a new SSH / EC2 terminal session 
 
```bin/kafka-console-producer.sh --broker-list $MYPLAINTEXTBROKERS --topic PlainTextTestTopic```
Type messages and the [ENTER] to send them. You will see the messages appear in the console consumer window
