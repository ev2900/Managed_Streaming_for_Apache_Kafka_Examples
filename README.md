1. Create a key pair
2. Deploy CloudFormation stack

[![Launch CloudFormation Stack](https://sharkech-public.s3.amazonaws.com/misc-public/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=msk-cluster&templateURL=https://sharkech-public.s3.amazonaws.com/misc-public/msk_cluster.yaml)

3. SSH into the Ec2 instance created by the CloudFormation stack. You may need to update the security group associated with the Ec2 instance to allow inbound traffic from your laptop
