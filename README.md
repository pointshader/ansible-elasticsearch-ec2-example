ansible-elasticsearch-ec2-example
=================================

Ansible playbook to spin up an Elasticsearch cluster on Amazon's EC2 platform

---
##Setup

#### 1. Create a new EC2 Security Group

- [Amazon has a great walkthrough][1]
- Port 9300 should only be open to rest of the security group (Elasticsearch Transport Layer)
- Port 9200 and/or Port 80 should be carefully restricted (Elasticsearch Command Layer)
- Port 22 should be open (SSH Access)

#### 2. Create and download an EC2 keypair
	
- You can only download this once, and instances are locked to a key, be careful!

#### 3. Install Ansible on your local machine

- Use your package manager or [download directly from Ansible][2]

## Configure

#### 1. Edit the Ansible playbook

>deploy_elasticsearch.yml

- Insert your keypair name, instance type, security group, AMI image, and instance count
>      keypair: EC2KEYPAIRNAME
>      instance_type: t1.micro
>      security_group: EC2SECURITYGROUPNAME
>      image: AMINAME
>      instance_count: 2
- I used ami-c30360aa (Ubuntu 13.04 64-bit), but other Ubuntu images should work.

#### 2. Edit the Elasticsearch configuration file
	
>elasticsearch/elasticsearch.yml

- Insert your AWS access key and secret key, and add any additional settings

##Run

#### 1. Export AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY as variables to your shell
	
- Calling source on a shell script is great for this
>       #!/bin/bash
>       export AWS_ACCESS_KEY_ID='AKIA123456781234567'
>       export AWS_SECRET_ACCESS_KEY='123abcdefghijklmnopqrstuv'
    

#### 2. Run Ansible
	
- Execute the following: 
> ansible-playbook deploy_elasticsearch.yml -vv --private-key=/path/to/ec2/key
- Make sure the key has permissions for only the current user! 

#### 3. Relax	


- Or test your cluster by executing
>curl http://EC2PUBLICIPOFANODE:9200/_cluster/health?pretty=true

---

####Quick fixes
- If Ansible complains about the local host group not existing, you may need to add 127.0.0.1 under the [local] heading in your ansible hosts file.

[1]:[http://docs.aws.amazon.com/gettingstarted/latest/wah/getting-started-security-group.html]
[2]:[http://ansible.cc/]
