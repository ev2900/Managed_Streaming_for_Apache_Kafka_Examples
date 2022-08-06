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

3. View the MSK cluster's client information. Replace/run *<TLS-boot-strap-server>* with the Bootstrap servers, TLS, Private endpoint value

```export MYTLSBROKERS=<TLS-boot-strap-server>```
 
## Create a Topic
  
1. Create a kafka topic
  
``` ```
  
## Configure Client TLS
  
```mkdir /home/ubuntu/tmp/```
  
```cp /usr/lib/jvm/java-1.11.0-openjdk-amd64/lib/security/cacerts /home/ubuntu/tmp/kafka.client.truststore.jks```

```client.properties```
  
```
security.protocol=SSL
ssl.truststore.location=/tmp/kafka.client.truststore.jks
```

## Console Producer
  
```kafka_2.12-2.6.2/bin/kafka-console-producer.sh --broker-list $MYTLSBROKERS --producer.config /home/ubuntu/tmp/client.properties --topic TLSTestTopic```

## Console Consumer
  
In a new SSH / EC2 terminal session 
  
```kafka_2.12-2.6.2/bin/kafka-console-consumer.sh --bootstrap-server $MYTLSBROKERS --consumer.config /home/ubuntu/tmp/client.properties --topic TLSTestTopic```
