
### Cluster Creation

1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster.yaml)

### Create a Topic

1. SSH into the Ec2 instance created by the CloudFormation stack. *You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop*
2. Install required dependencies

```sudo apt-get update```

```sudo apt install default-jdk```

```wget https://archive.apache.org/dist/kafka/2.6.2/kafka_2.12-2.6.2.tgz```

```tar -xzf kafka_2.12-2.6.2.tgz```

```cd kafka_2.12-2.6.2```

3. Create a kafka topic

```bin/kafka-topics.sh --bootstrap-server $MYBROKERS --create --topic test10 --partitions 10 --replication-factor 3```

### Add Brokers to MSK Cluster


