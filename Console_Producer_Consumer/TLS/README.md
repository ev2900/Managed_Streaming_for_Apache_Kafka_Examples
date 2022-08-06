## Cluster Creation

1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster-tls&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster_TLS.yaml)

The CloudFormation stack will deploy the architecture below

<img width="700" alt="OpenSearch_demo_Architecture" src="https://github.com/ev2900/Managed_Streaming_for_Apache_Kafka_Examples/blob/main/Cluster_Expansion/ReadMe_Architecture/architecture.png">

## EC2 post-Deploymnet Configuration

1. SSH into the Ec2 instance created by the CloudFormation stack. *You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop*

2. Install required dependencies
