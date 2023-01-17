# eficode-assignment-weatherapp-ansible
This repo handles an Ansible part for the assingment.

## How to use this repository for deployment
This repository is for a control node to deploy production docker images to production servers.
I will assume the current conditon below.
- The control node is outside of AWS infrastructure.
- The target node is already provisied by AWS CloudFormation.
- The automatic deployment will be executed by Ansible tool.

### Prerequisite
1. Ansible should be installed on the control node.
2. Target nodes should be opened for SSH access.
3. Python should be installed on the target nodes.
4. `.prod.env` file should be created the root of working directory on target node

```Shell
$ cat .prod.env
# API key to access openweathermap for production weatherapp
APPID=<your api key>
# EXAMPLE
# APPID=12345678910
# You need to provide .prod.env at workdir for production server
ENDPOINT=http://0.0.0.0:9000/api
```
 
### How to use
1. Create working directory and git clone in the directory
```Shell
$ mkdir wk_ansible and cd wk_ansible
# Using one of commands depending on your condition
$ git clone https://github.com/dalpengholic/eficode-assignment-weatherapp-ansible.git
$ git clone git@github.com:dalpengholic/eficode-assignment-weatherapp-ansible.git

```
2. Create inventory file at the root of the cloned directory to Make connection from a control node to a target node
```Shell
$ cd eficode-assignment-weatherapp-ansible
$ pwd
wk_ansible/eficode-assignment-weatherapp-ansible
# Giving a dns or ip address of target node. After making inventory file
$ can inventory
[webserver]
ec2-<your target ip>.compute-1.amazonaws.com
```

3. Test connection first by using Ansible ping module
```Shell
$ ansible all -m ping -u ubuntu --private-key /home/youruser/Downloads/yourkey.pem
ec2-.compute-1.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```



## How I understand this assignment
- General purpose
This assignment is designed to measure candidates' knowledge and implementation skillsregarding the general Continuos Integration and Continuous Deployment(CI/CD) as well as Infrastructure as Code(IaC). 

- Target of Ansible automation
The main target of Ansible assignment is to install docker and docker-compose and deploy weatherapp on EC2 instance by using Ansible with official docker images from [the master branch of assignment repo](https://github.com/dalpengholic/eficode-assignment-weatherapp). This Ansible automation assumes that provisioning computing sources are handled by Infrastructure as Code(IaC) tools such as  AWS Cloudformation or Hasicorp Terraform.

give the same development environment to developers by using docker and docker-compose. The second target is using proper branch strategy to accelerate developement speed as well as keep the source codes in safe and clean.

- Virtual short 1 week sprint
I have followed the virtual 1 week sprint below.
  - Day1: Assignment analysis
  - Day2 to Day4: Plan and action and short review
  - Day5: Rest
  - Day6 and Day7: Polishing and main review

## How I approached this Ansible assignment
- First, I decided not to create my own playbook, but to use ready-made Ansible-Galaxy collection to install docker and docker-compose. The reason is using collection gives more stable provisioing because the collection has been updated and watched by others and saves time as well. 
- Second, I made a playbook for running weatherapp using docker-compose. The playbook's process consists of followings
    - Cleaning Environment 
    - Preparing directory and git cloning from the repo with only master branch 
    - Pulling new images and running docker-compose.prod.yml file

## My assumtions regarding this assignment
- Target node(s) is provided by AWS CloudFormation(CF).
- I assumed that the operation system on an EC2 instance uses Ubuntu 22.04.
- I used  my laptop as the main Ansible control node to control target nodes.

## What I have learned
- I realized that `.prod.env` having ENDPOINT variable is mandatory for production server during my check up.
- An Ansible community module like `docker_compose` is a bit tricky to controll docker-compose action compared to official builtin moudles. I had to use `cmd` built in module to handle docker-compose

## What need to be improved for my result
- Ansible playbooks shoule be handle not only Ubuntu or Debian, but also different type of OS in general for reusability
- There could be better way to handle APIKEY in secured manner such as using Ansible Vault to encrypt API key or Hashicorp Vault using dedicated service to handle sensitive data
- It would be better to organize the current Ansible directory structure such as using host_vars and roles/tasks folders for reusability and getting better maintainability 
- It would be better to have a bation host in AWS as a control node. 
