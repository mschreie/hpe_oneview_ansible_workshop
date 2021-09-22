# Automationg HPE OneView to manipulate Server Configuration

## Introduction

This exercise is going to assume you have a working OneView connection and credentials in place as listed in the exercise [Getting Started with HPE OneView](/exercises/getting_started_with_hpe_oneview.md)

![ans-wksp-01](/images/ansible-workshop-illustration-04.png)


In this exercise, we will automatically create a new server profile template and deploy the same to one or more servers.
In order to achieve this we will need work from an existing repository that will host all the playbooks needed over the course of this workshop.


We already have:
* a project using the correct github repository 
* all credentials in place

In this section, The playbooks needed are called :

```
hpe_oneview_create_srv_prof_templ.yml
hpe_oneview_deploy_srv_prof.yml
```

### Step 1 : Create HPE OneView Inventory II

In the previous exersice we used a very simple approach on inventories. Naming the Oneview Host as the target in the play-header does not allow to scale tasks, when multiple Servers should be deployed.
Furthermore we add additional data into the inventory:

```
localhost    ansible_connection=local
bastion_host ansible_host=10.6.80.25
oneview_host ansible_host=composer.synergy.hybridit.hpecic.net

[esx_hosts]
esx1    ansible_host="1.2.3.4"   ov_hardware="Demo1_Rack12_Frame3, bay 5" ilo_ip=2.3.4.4
esx2    ansible_host="1.2.3.5"   ov_hardware="Demo1_Rack12_Frame3, bay 6" ilo_ip=2.3.4.5
```

For ease of use this inventory resides within github and we create an Ansible Tower inventory which syncs data form there.


![Create HPE OneView](/images/create-inv.png)

1. Navigate to **Inventories**
2. Create **New Inventory** (don't choose *Smart Inventory*)
* NAME : Workshop Inventory from GitHub
* ORGANIZATION: Default

click *Save* *SOURCES* *+* 

This brings you to "CREATE SOURCE" page
* **NAME**                : Inventory file from Github
* **SOURCE**              : Sourced from a Project
* **ANSIBLE ENVIRONMENT** : Use Default Environment
* **PROJECT**             : HPE OneView Workshp
* **INVENTORY FILE**      : inventory/hosts
* **VERBOSITY**           : 1 
UPDATE OPTIONS: <br>
choose all update options: <br>
OVERWRITE; 
OVERWRITE VARIABLES; 
UPDATE ON LAUNCH; 
UPDATE OM PROJECT UPDATE; 


### Step 2 : Create Job Templates
   
Now it's time to create the **Job Templates**. 
 
![Create Job Template](/images/create-enclo-job-template.png)

1. Navigate to Templates
2. Create **New Job Template** using the parameters below:

#### OneView : Server Profile Template Creation
This Job Template will use a very blunt Playbook which creates a Server Profile Template.
In a real world we would work with a Jinja2-Template which gets customized with parameters instead of hardcoding everything into the playbook directly. But for this to be feasable we need to agree jointly on some predefined structure.
Take this playbooks as a "proof of concept" level.

* **NAME**: OneView : Server Profile Template Creation
* **Inventory**: Workshop Inventory from GitHub
* **Projects**: HPE OneView Workshop
* **PLAYBOOK** : hpe_oneview_create_srv_templ.yml
* **Credentials** : HPE OneView Credentials; HPE ILO Credential
* **VIRTUAL ENVIRONMENT** : /var/lib/awx/venv/testoneview/  (Creating this virtual enviroment please refer to [Excercice 3](/exercises/virtual_environment.md)

We do not need privilege escalation or any other option.

#### Step 3 : OneView : Deploy Server Template

* **NAME**: OneView : Deploy Server Template
* **Inventory**: Workshop Inventory from GitHub
* **Projects** : HPE OneView Workshop
* **PLAYBOOK** : hpe_oneview_deploy_srv_prof.yml
* **Credentials** : HPE OneView Credentials
* **VIRTUAL ENVIRONMENT** : /var/lib/awx/venv/testoneview/  (Creating this virtual enviroment please refer to [Excercice 3](/exercises/virtual_environment.md)

We do not need privilege escalation or any other option.


### Step 4 : Launch the job templates

* Click on the little rocket next to the Job templates
     * First Start with : **Server Profile Template Creation**
     * Once Successfull then you can launch : **Deploy Server Template**
* Review output in the log window
* Review in Oneview console what is happening 

## Conclusion

With this, we managed to define a oneview server profile template and push it onto a server effectively managing hw configuration.
