# eficode-assignment-weatherapp-ansible
This repo handles an Ansible part for the assingment

## How to use this repository for deployment

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
