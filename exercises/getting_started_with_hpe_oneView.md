# Automating HPE OneView

## Introduction

This exercise is going to assume the prerequisites below are all met:

* You have already have a good understand of Ansible and Red Hat Ansible Tower (Controller)
* You have access to an lab environment which includes:
    * 1 x HPE rack that is powered on with at least 1 x physical Server
    * 1 x HPE OneView connected to the management network.
    * 1 x Red Hat Ansible Tower (Controller) instance with valid subscription to manage the environment.
    * 1 x bastion host that has an nginx/httpd running

![ans-wksp-01](/images/ansible-workshop-illustration-04.png)


In this exercise, we will integrate for the first time with HPE OneView. As a first playbook we will start by gathering some facts from the Enclosure.

In order to achieve this we will need to:

Create Project from the git repository below, it will host all the playbooks we will need over the course of this workshop.
: [https://github.com/mschreie/hpe_oneview_ansible_workshop.git](https://github.com/mschreie/hpe_oneview_ansible_workshop.git)

The playbook needed in this section is called :  hpe_oneview_get_enclosures_facts.yml

In order to authenticate to the enclosure, we will need Create a new credentials specifically for HPE OneView.
Since HPE OneView is not listed in the Credentials types available by default in Ansible Tower (Controller), we will need to create a new type.


Step 1:
Navigate to Credentials Types
Create a New Credential Type 
