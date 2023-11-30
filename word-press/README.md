## Deploying a HA WordPress Website using AWS

In this project, I will show my step-by-step process of deploying a fully functioning WordPress Website that is hosted on Amazon EC2, connected with Amazon RDS database, and is publicly accessible over the internet. This projects uses the following services: RDS, EC2, VPC, EBS, ASG, Load Balancers, Security Groups, and CloudFormation.

------
First and foremost, WordPress needs to be installed in the directory.  Using the terminal, we can achieve this easily by using the following commands:
```bash
~$curl https://wordpress.org/wordpress-6.2.tar.gz -o wordpress.tar.gz

~$ wget https://github.com/aws-samples/eb-php-wordpress/releases/download/v1.1/eb-php-wordpress-v1.zip

~$ tar -xvf wordpress.tar.gz

~$ mv wordpress wordpress-beanstalk

~$ cd wordpress-beanstalk

~/wordpress-beanstalk$ unzip ../eb-php-wordpress-v1.zip
```
Now for the first part of this project, I needed to establish a relation database and connect it. This was done using RDS and intializing it with MySQL. By enabling Multi-AZ deployment, I am able to make this database highly available and scalable. Security groups were intialized here to match the security groups used for the EBS later and allow inbound traffic.

![alt text](https://github.com/Seanhui2000/AWS-Projects/blob/main/word-press/Screenshots/Creation%20of%20MySQL%20Database.png) 

Following this, I created an Elastic Beanstalk environment to automate the configuration. The environment will create an EC2, load balancer, ASG, and S3 as well as using CloudFormation which greatly expedites the process allowing for time and performance efficiency. For high availability, I set up an auto scaling group with our created environment and to support uploads across multiple instances, this projects uses an EFS. The previous security groups were configured with this EBS to allow access to the MySQL database and the environment was created.

![alt test](https://github.com/Seanhui2000/AWS-Projects/blob/main/word-press/Screenshots/EBS%20Creation.png)

To upload the WordPress application to our new environment, I simply zipped the WordPress folders and uploaded it to our configured EBS. 

![alt test](https://github.com/Seanhui2000/AWS-Projects/blob/main/word-press/Screenshots/sucessfully%20uploaded%20word%20press.png)

From here, all that's left is the configuration for the WordPress website and the application is successfully deployed! 

![alt test](https://github.com/Seanhui2000/AWS-Projects/blob/main/word-press/Screenshots/wordpress%20website%20success.png)

------

What I've learned from this project is how to use the available services such as EBS to quick start web applications and manage environments as well as configure services such as RDS to connect with each other. Along with this, I was able to learn how to efficiently use services to follow best policy such as making the environment highly available using auto scaling groups and enabling the database to be multi-AZ. This allows me to implement best practice at all times.

-----

**References**

https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hawordpress-tutorial.html#php-hawordpress-tutorial-database

