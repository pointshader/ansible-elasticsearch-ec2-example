ansible-elasticsearch-ec2-example
=================================

Ansible playbook to spin up an Elasticsearch cluster on Amazon's EC2 platform

Setup
-----

### Step 1: Create an EC2 Security Group

-Port 9300 should only be open to rest of the security group (Elasticsearch Transport Layer)

-Port 9200 and/or Port 80 should be carefully restricted (Elasticsearch Command Layer)

-Port 22 should be open (For SSH)

### Step 2: Create and download an EC2 keypair
	
-You can only download this once, and instances are locked to a key, be careful!

### Step 3: Install Ansible on your local machine

Configure
-----

### Step 1: Edit the Ansible playbook

> deploy_elasticsearch.yml
	
-Insert your preferred keypair name, instance type, security group, AMI image, and instance count

### Step 2: Edit the Elasticsearch configuration file
	
> elasticsearch/elasticsearch.yml

-Insert your AWS access key and secret key, and add any additional settings

Run
-----

### Step 1: Export AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY to the shell
	
-Calling source on a shell script is great for this

### Step 2: Run Ansible
	
-Run the following: ansible-playbook deploy_elasticsearch.yml -vv --private-key=/path/to/ec2/key

-Make sure the key has private permissions!

### Step 3: Relax
	
-Add celebratory reward of choice

-Or test your cluster by executing: curl EC2PUBLICIP:9200/_cluster/health?pretty=true

Note: If Ansible complains about the local host group not existing, you may need to add 127.0.0.1 under the [local] heading in your ansible hosts file.
