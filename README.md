First create a client machine from which we will connect to the MSK cluster
To create a client machine:

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
2. Choose Launch instances.
Choose Select to create an instance of Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type.
3. Choose the t3.medium instance type by selecting the check box next to it.
4. Choose Next: Configure Instance Details.
5. In the Network list, choose the VPC we created with Terraform
6. In the Auto-assign Public IP list, choose Enable.
7. Choose Add Tag.
8. Enter Name for the Key and MSKClient for the Value.
9. Choose Review and Launch, and then choose Launch.
10. Choose Create a new key pair, enter MSKKeyPair for the Key pair name, and then choose Download Key Pair. Alternatively, you can use an existing key pair if you prefer.
11. Read the acknowledgement, select the check box next to it, and choose Launch Instances.

Note. On the security group configuration page add 22 TCP port for SSH connection.

To create topic
1. ssh to instances
2. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
3. In the navigation pane, choose Instances, and then choose MSKClient by selecting the check box next to it.
4. Choose Actions, and then choose Connect. Follow the instructions to connect to the client machine MSKClient.
5. Install Java on the client machine by running the following command:
sudo yum install java-1.8.0
6. Run the following command to download Apache Kafka
wget https://archive.apache.org/dist/kafka/2.6.2/kafka_2.12-2.6.2.tgz
7. Run the following command in the directory where you downloaded the TAR file in the previous step.
tar -xzf kafka_2.12-2.6.2.tgz
8. Go to the kafka_2.12-2.6.2 directory.
9. Open the Amazon MSK console at https://console.aws.amazon.com/msk/.
10. Choose View client information.
11. Copy the private endpoint for plaintext authentication and the Apache ZooKeeper connection string (also for plaintext communication).
12. Run the following command, replacing ZookeeperConnectString with the string that you obtained in the previous instruction.
13. bin/kafka-topics.sh --create --zookeeper ZookeeperConnectString --replication-factor 3 --partitions 1 --topic MSKTutorialTopic

Publish a message
1. Go to the bin folder of the Apache Kafka installation on the client machine, and create a text file named client.properties with the following contents.
security.protocol=PLAINTEXT
2. Run the following command in the bin folder, replacing BootstrapBrokerString with the private endpoint string that you obtained in the previous step(Create a Topic)
./kafka-console-producer.sh --broker-list BootstrapBrokerString --producer.config client.properties --topic Example

3. Enter any message that you want, and press Enter. Repeat this step two or three times. Every time you enter a line and press Enter, that line is sent to your Apache Kafka cluster as a separate message.

Finally check if lambda source mapping is configured to work with the cluster you sending messages to.