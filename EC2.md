# Question:

### 1. __An IP address of EC2 instance gets changed after the restart__

Actually, When you `stop/start` your `instance`, `the IP address will change`. If you `reboot` the instance, it `will keep the same IP addresses`. Unfortunately, it is not possible for us to reassign the address to your instance as that address would have been released back into the pool used by other EC2 instances.

If you want to avoid this issue in the future, depending on your needs:

- If you only need a fixed public IP address, you can assign an Elastic IP address to your instance.

- If you need both public and private IP addresses to remain the same throughout the lifetime of the instance, you can launch your instance in VPC instead. The private IP address assigned to an instance in VPC remains with the instance through to termination.

`Public IP`: try to use an Elastic Ip, then you will not have this problem anymore. You can allocate an new one to your instance directly on AWS Console or programmatically. But if your are using an autoscaling-group you will have to do it on your user-data or cloud-init process.

`Private IP`: Unfortunately you cannot fix a private Ip address to an instance. The only way is to use DNS and in that case a private DNS zone for you VPC

To learn more, see the aws documentation to [assign elastic ip][aws-ec2].

<!-- Reference -->

[aws-ec2]:https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html