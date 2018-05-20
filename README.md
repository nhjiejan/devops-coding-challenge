# Devops-coding-challenge

## Description
Based on the devops coding challenage as specified [here](https://github.com/adazzle/devops-coding-challenge). To be able to automate the creation of an nginx web server in AWS behind an Elastic Load Balancer using Ansible.

## Prepare your environment

### Install software
For Infrastructure automation, this project uses Ansible. For all platform installation instructions, see [Ansible](https://www.ansible.com). To install with [Homebrew](http://docs.brew.sh/Installation.html) run:

    brew install ansible

We also we need to install `boto` module to access AWS resources

    pip install boto

### configure secrets
To configure any secrets you can use [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html). Protected variables may include ```access_key_id```, ```secret_access_key``` and ```ssh_key```. Example usage can be found [here](https://gist.github.com/tristanfisher/e5a306144a637dc739e7).

Update the [/vars/aws-settings.yaml](./vars/aws-settings.yaml) with your AWS id credentials and an AWS ssh key-pair created from the AWS management console. Next, update the [ansible.cfg](./ansible.cfg) file with the location of the ```private_key_file``` and ```vault_password_file``` variables


### Deploy ec2 instance and ELB
This will:
* create EC2 instance
* configure EC2 instance
* deploy web-app to EC2 instance
* register EC2 instance to ELB

From the project root folder run:


    ./deploy_all.yaml


## Test deployment
This will:
1. Get a list of EC2 instances tagged by **devops-coding-challenge** ELB group
2. get request to EC2 instance public IP address `http://instance-ipv4-address/version.txt`
3. if result from step 2 not equal to **app_version** from  `vars/settings.yml` - add instance to configuration group **webservers-to-fix**
4. start or restart NGINX on each EC2 instance from configuration group **webservers-to-fix**

From the project root folder run:

    ./live-probe.yaml
