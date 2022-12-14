# MSK Cluster Expansion

Follow the high level steps below to create a MSK cluster, add brokers, reassign topic partitions to new brokers, expand broker stroage

## Cluster Creation

1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster.yaml)

The CloudFormation stack will deploy the architecture below

<img width="700" alt="OpenSearch_demo_Architecture" src="https://github.com/ev2900/Managed_Streaming_for_Apache_Kafka_Examples/blob/main/Cluster_Expansion/ReadMe_Architecture/architecture.png">

## EC2 post-Deploymnet Configuration

1. SSH into the Ec2 instance created by the CloudFormation stack. *You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop*

2. Install required dependencies

```sudo apt-get update```

```sudo apt install default-jdk```

```wget https://archive.apache.org/dist/kafka/2.6.2/kafka_2.12-2.6.2.tgz```

```tar -xzf kafka_2.12-2.6.2.tgz```

```cd kafka_2.12-2.6.2```

3. View the MSK cluster's client information. Replace/run *<plain-text-boot-strap-server>* with the Bootstrap servers, Plaintext, Private endpoint value

```export MYBROKERS=<plain-text-boot-strap-server>```

## Create a Topic

1. Create a kafka topic

```bin/kafka-topics.sh --bootstrap-server $MYBROKERS --create --topic test10 --partitions 10 --replication-factor 3```

## Add Brokers to MSK Cluster & Re-assign Partitions

1. via. the AWS console edit the number of brokers. Adjust the number of brokers perzone to 2

2. View the current partition broker assignments
 
```bin/kafka-topics.sh --bootstrap-server $MYBROKERS --describe --topic test10```

Notice how all of the partitions are assigned to brokers 1, 2, 3. Given that the MSK cluster is now expanded to include 6 brokers, we will remap the topic's partitions to include broker 4, 5, 6

To reasign partitions to different brokers we need to create a json file that specifies which partitions, partition replicates should be mapped to which brokers. We can either create this mapping by hand or kafka can suggest a new mapping for us 

To have kafka suggest a new mapping

3. Create a file ```topics-to-move.json```
4. Populate the file with 

```{ "topics": [ { "topic" : "test10"}], "version":1}```

5. ```bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --topics-to-move-json-file topics-to-move.json --broker-list "1,2,3,4,5,6" --generate``` 
6. Copy the *Proposed partition reassignment configuration* part of the respond into a new json file ```expand-cluster-reassignment.json```
7. ```bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --reassignment-json-file expand-cluster-reassignment.json --execute```

To monitor the progress of the partition broker reassignment 

```bin/kafka-reassign-partitions.sh --bootstrap-server $MYBROKERS --reassignment-json-file expand-cluster-reassignment.json --verify```

You can verify that the reassignment worked by running ```bin/kafka-topics.sh --bootstrap-server $MYBROKERS --describe --topic test10``` and observing that the paritions in the topic are now mapped across all 6 brokers

## Expand Broker Storage

1. In the AWS console expand the EBS storage volume per broker to 1000 GiB   
