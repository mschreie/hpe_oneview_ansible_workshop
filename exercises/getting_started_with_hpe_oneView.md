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
In order to achieve this we will need work from an existing repository that will host all the playbooks needed over the course of this workshop.


### Step 1: Create Github Credentials

These credentials are needed to work on your Github account in order to download repository. If your project is public, you won't need to specify any credentials.

![Create Github Credentials](/images/create-github-creds.png)


### Step 2: Create Project

Navigate to **Projects** in Tower UI, create a **New Project** :

![Create-Prj](/images/create-prj.png)

* NAME : HPE OneView Workshop
* Organization : Default
* SCM TYPE : Git
* SCM URL :[https://github.com/mschreie/hpe_oneview_ansible_workshop.git](https://github.com/mschreie/hpe_oneview_ansible_workshop.git)
* Tick : CLEAN, DELETE ON UPDATE, UPDATE REVISION ON LAUNCH
* SCM Credentials : <YOUR GITHUB CREDENTIALS>


In this section, The playbook needed is called :  ```hpe_oneview_get_enclosures_facts.yml```

### Step 3: Create Credentials Type:

In order to authenticate to the enclosure, we will need Create a new credentials specifically for HPE OneView. Since HPE OneView is not listed in the Credentials types available by default in Ansible Tower (Controller), we will need to create a new type.

1. Navigate to Credentials Types
2. Create a New Credential Type

![Create-Cred-Type](/images/create-creds-type.png)

**NAME** : HPE Oneview Credentials

**INPUT CONFIGURATION** :

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

**INJECTOR CONFIGURATION**:

```
extra_vars:
  oneview_apiversion: '{{ api_version }}'
  oneview_domain: '{{ domain }}'
  oneview_password: '{{ password }}'
  oneview_username: '{{ username }}'
```
