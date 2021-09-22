# Getting Started with HPE OneView

## Introduction

This exercise is going to assume the prerequisites below are all met:

* You already have a good understanding of Ansible and Red Hat Ansible Tower (Controller)
* You have access to an lab environment which includes:
    * at least 1 x physical HPE Server with ILO 
    * 1 x HPE OneView connected to the network.
    * 1 x Red Hat Ansible Tower (Controller) instance with valid subscription to manage the environment.
    * 1 x bastion host that has an nginx/httpd running

![ans-wksp-01](/images/ansible-workshop-illustration-04.png)


In this exercise, we will integrate for the first time with HPE OneView. As a first playbook we will start by gathering some facts from the Enclosure.
In order to achieve this we will need work from an existing repository that will host all the playbooks needed over the course of this workshop.


### Step 1: Create Github Credentials

These credentials are needed to work on your Github account in order to download the repository. If your project is public, you won't need to specify any credentials.

![Create Github Credentials](/images/create-github-creds.png)


### Step 2: Create Project

Navigate to **Projects** in Tower UI, create a **New Project** :

![Create-Prj](/images/create-prj.png)

* NAME : HPE OneView Workshop
* Organization : Default
* SCM TYPE : Git
* SCM URL : https://github.com/<YOUR_NICKNAME>/hpe_oneview_ansible_workshop.git
* Tick : CLEAN, DELETE ON UPDATE, UPDATE REVISION ON LAUNCH
* SCM Credentials : YOUR GITHUB CREDENTIALS
* Ansible Environment : /var/lib/awx/venv/testoneview/  (Creating this virtual enviroment please refer to [Excercice 3](/exercises/virtual_environment.md)


In this section, The playbook needed is called :

```
hpe_oneview_get_enclosures_facts.yml
```


### Step 3 : Create Credentials Type:

In order to authenticate to the enclosure, we will need to create a new credential specifically for HPE OneView. Since HPE OneView is not listed in the Credential types available by default in Ansible Tower (Controller), we will need to create a new type first.

1. Navigate to Credentials Types
2. Create a New Credential Type

![Create-Cred-Type](/images/create-creds-type.png)

* NAME : HPE Oneview Credentials
* INPUT CONFIGURATION :
```
fields:
  - id: username
    type: string
    label: HPE Oneview username
  - id: password
    type: string
    label: HPE Oneview password
    secret: true
  - id: domain
    type: string
    label: HPE Oneview Domain
  - id: api_version
    type: string
    label: HPE Oneview Api version
required:
  - username
  - password
  - domain
  - api_version
```
* INJECTOR CONFIGURATION :
```
extra_vars:
  oneview_apiversion: '{{ api_version }}'
  oneview_domain: '{{ domain }}'
  oneview_password: '{{ password }}'
  oneview_username: '{{ username }}'
```

### Step 4 : Create HPE OneView Credential

As soon as the new Credential type is added, we can create the HPE OneView Credential to be used later.

![Create_One_View Credentials](/images/create-oneview-creds.png)


* NAME : HPE Oneview creds
* CREDENTIAL TYPE : HPE Oneview Credentials
* HPE ONEVIEW USERNAME : YOUR_HPE_ONEVIEW_ADMIN
* HPE ONEVIEW PASSWORD : YOUR_HPE_ONEVIEW_ADMIN_PASSWD
* HPE ONEVIEW DOMAIN : your_domain or local
* HPE ONEVIEW API VERSION : 2800 or later


### Step 5 : Create HPE OneView Inventory I

Every Job Template will require an inventory of managed hosts on which it will run. In this case, we are looking to automate HPE servers but the automation will run locally on Ansible Tower (Controller) and will only reach out to OneView via API calls. So, for simple "hello world" like testing we create an inventory with oneview host as the one and only server.

![Create HPE OneView](/images/create-inv.png)

1. Navigate to **Inventories**
2. Create **New Inventory**
* Name : Oneview Server
* Hosts: LIST_ALL_HP_ONEVIEW_IP


### Step 6 : Create Job Template : Gather Enclosures Facts
   
Now it's time to create the **Job Template** that will help to gather Facts from Enclosures. You can consider this job template as a "Hello World" example. It will help validate the integration points between Ansible Tower (Controller) and HPE OneView.
 
![Create Job Template](/images/create-enclo-job-template.png)

1. Navigate to Templates
2. Create **New Job Template** using the parameters below:

* NAME: "OneView :  Gather Enclosures Facts"
* Inventory: Oneview servers
* Credentials : HPE OneView Credentials
* Projects : HPE OneView Workshop
* PLAYBOOK : hpe_oneview_get_enclosures_facts.yml
* VIRTUAL ENVIRONMENT : /var/lib/awx/venv/testoneview/  (Creating this virtual enviroment please refer to [Excercice 3](/excerices/virtual_environment.md))

Then Add a survey that will store the defaut value :

* oneview_hostname: IP ADDR OF HPE ONEVIEW


### Step 7 : Launch the job template

* Click on the little rocket
* Provide OneView Variable as a survey : IP_ADDRESS_ONEVIEW


## Conclusion

With this, we managed to run our first playbook on Ansible Tower (Controller) and integrate with HPE Oneview.
