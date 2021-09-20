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

## Step 1 : Ansible Tower Preparation:

After the installation of Red Hat Ansible Tower (Controller), you will need to finalize a number of tasks in order to be ready for the workshop.

1- First, Tower (Controller) has to be registered to Red Hat Network and for that you'll need. Red Hat Account credentials or Red Hat Subscription Manifest a manifest file.


