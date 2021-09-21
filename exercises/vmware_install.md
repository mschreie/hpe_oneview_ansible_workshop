# Automated installation of VMWare ESXi

In this exercise we will automate the installation of VMware ESXi. 

## Prerequisits - General
In order to achieve this we need:
* a bastion host with a http-server 
* ansible access to that http-server
* root priviledges to that http-server as we want to loop mount iso-images
* a VMware installation iso residing on that http-server
* A server to install onto
* This server needs to have ILO in place

## Prerequisits in Tower
We already prepared somewhere else:
* inventory
* project


## Available playbooks
You find follwoing playbooks which need to be integrated into Ansible Tower:
* vmware_iso_prep.yml
  This playbook takes the original ISO image unpacks it, creates a additional, tailored kickstart file, assures that this kickstart file is used by adding certain bootparameters to boot.cfg and repacks all this into a new iso-file which get's placed into the webserver directory.

* vmware_iso_boot.yml
  This playbook boots from the customized iso image by connecting it as CDrom via ILO.
* vmware_iso_cleanup.yml
  This playbook cleans up some temporary files and directories created by vmware_iso_prep.yml. It helps assure you don't have any leftovers from older attempts in your newly created iso.

## Adjusting configuration files
All the prepared playbooks rely on 
* inventory/hosts file, which needs to be adopted to your environment. Don't change the names for bastion_host, oneview_host or esxi_hosts. the playbooks rely on these names.
* group_vars/all/vars.yml file, which also nees to be adopted to your setup.
After local change of these files do not forget to swicht to the root-directory of the repository and:
'''
git add .
git commit -m "some meaningful explanation"
git push
'''
see git documentation elsewhere in this repo

## Credentials Needed

### Machine Credential
We execute the iso preparation on the bastion host, therefore we need to asure to have machine credentials which work for the bastion host and also allow to become root.

### ESXi root user credential
We inject the later root password into the kickstart file. As this injection use case is not related to "machine credentials" we once again need a custom credential type.

### ILO credential
We need to reach out to ILO and need a ILO Credential. Similar to the HPE Oneview credential this will be a custom credential type


## Creating custom credential types

As outlined we need 2 custom credential types. They are created in a similar way:

In Tower (Controller) UI 
1. Navigate to Credentials Types
2. Create a New Credential Type

![Create-Cred-Type](/images/create-creds-type.png)

* NAME : HPE ILO Credentials
* INPUT CONFIGURATION :
```
fields:
  - id: username
    type: string
    label: HPE ILO username
  - id: password
    type: string
    label: HPE ILO password
    secret: true
```
* INJECTOR CONFIGURATION :
```
extra_vars:
  ilo_password: '{{ password }}'
  ilo_username: '{{ username }}'
```
   
And as a second credential type:

* NAME : ESXi root user credential type
* INPUT CONFIGURATION :
```
fields:
  - id: username
    type: string
    label: ESXi root username
  - id: password
    type: string
    label: ESXi password
    secret: true
```
* INJECTOR CONFIGURATION :
```
extra_vars:
  esxi_password: '{{ password }}'
  esxi_username: '{{ username }}'
```

## Creating the credentials
As outlined above we need three different credentials we will now create:

In Tower (Controller) UI 
1. Navigate to Credentials 
2. Create a New Credential by pressing + button

This will look similar to:

![Create_One_View Credentials](/images/create-oneview-creds.png)

### Creating Machine credential to connect to bastion host
* NAME : Ansible User - Machine credential
* CREDENTIAL TYPE : Machine
* USERNAME : ansible
* PASSWORD : We injected a password via bastion host preparation which need to nbe put here


### Creating ESXi root credential to inject into kickstart configuration
* NAME : ESXI root user
* CREDENTIAL TYPE : ESXi root user credential type
* USERNAME : root
* PASSWORD : a password to your choosing

### Creating ILO credential to connect to ILO with
* NAME : HPE ILO Credential
* CREDENTIAL TYPE : HPE ILO Credentials
* USERNAME : Administrator
* HPE ILO USERNAME : YOUR_HPE_ILO_ADMIN
* HPE ILO PASSWORD : YOUR_HPE_ILO_ADMIN_PASSWD

## Create Job Templates
We will now create 3 job-templates. One for each of the playbooks mentioned earler in this text:

In Tower (Controller) UI 
1. Navigate to Templates 
2. Create a New Job Template by pressing + button and choosing Job Template

## ESXi : Customize boot ISO
* NAME        : ESXi : Customize boot ISO
* JOB TYPE    : Run
* INVENTORY   : Workshop Inventory from GitHub
* PROJECT     : HPE OneView Workshop
* PLAYBOOK    : vmware_iso_prep.yml    # Hint: If this does not show up, sync your inventory source first
* CREDENTIALS : Ansible User - Machine credential; ESXi root user
* VERBOSITY   : 0
* ANSIBLE ENVIRONMENT : /var/lib/awx/venv/ansible
* ENABLE PRIVILEGE ESCALATION

## ESXi : Cleanup ISO creation
* NAME        : ESXi : Cleanup ISO creation
* JOB TYPE    : Run
* INVENTORY   : Workshop Inventory from GitHub
* PROJECT     : HPE OneView Workshop
* PLAYBOOK    : vmware_iso_cleanup.yml    # Hint: If this does not show up, sync your inventory source first
* CREDENTIALS : Ansible User - Machine credential
* VERBOSITY   : 0
* ANSIBLE ENVIRONMENT : /var/lib/awx/venv/ansible
* ENABLE PRIVILEGE ESCALATION

## ESXi : Boot from ISO via ILO
* NAME        : ESXi : Boot from ISO via ILO
* JOB TYPE    : Run
* INVENTORY   : Workshop Inventory from GitHub
* PROJECT     : HPE OneView Workshop
* PLAYBOOK    : vmware_iso_boot.yml    # Hint: If this does not show up, sync your inventory source first
* CREDENTIALS : HPE ILO Credential
* VERBOSITY   : 0
* ANSIBLE ENVIRONMENT : /var/lib/awx/venv/<your created venv>
* ENABLE PRIVILEGE ESCALATION
