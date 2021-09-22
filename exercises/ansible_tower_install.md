# Ansible Tower Deployment

### Assumptions:

We assume that you have already installed **Red Hat Enterprise Linux 8.3 or later** on the virtual machine requested in the previous exercice : Prerequisites.
We assume also that you have valid Red Hat Ansible Automation Platform valid subscriptions that will be used in this workshop to install (Ansible Tower 3.8) 

### Download Ansible Tower:

Access the latest version of the bundled Ansible Automation Platform installation program (ansible-automation-platform-setup-bundle-1.2.5-1.tar.gz) directly from [Downloads](https://access.redhat.com/downloads/content/480) : Please note that you must have a Red Hat customer account to access the downloads.

Make sure to pick the bundle installer :

```
ansible-automation-platform-setup-bundle-1.2.5-1.tar.gz
root@localhost:~$ tar xvzf ansible-automation-platform-setup-bundle-1.2.5-1.tar.gz
```
Make sure to copy this installer under /root of the Ansible Tower Instance.

### Configure Inventory to setup Ansible Tower

* **Single Machine **:

- This setup will be standalone Tower with database on the same node as Tower. This is a single machine install of Tower.
- The web frontend, REST API backend, and database are all on a single machine. This is the standard installation of Tower. It also installs PostgreSQL from your OS vendor repository, and configures the Tower service to use that as its database. 

Please configure you inventory as the exemple of the inventory file below:

```
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='password'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='password'
```
### Run Ansible Tower Installer

The Tower setup playbook script uses the inventory file and is invoked as ./setup.sh from the path where you unpacked the Tower installer tarball.

```
root@localhost:~$ ./setup.sh
```
