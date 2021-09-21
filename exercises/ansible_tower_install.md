# Setting up Ansible Tower

### Prerequisites:

Starting with Ansible Tower 3.8, you must have valid subscriptions attached before installing and running the Ansible Automation Platform. Even if you already have valid licenses from previous versions, you must still provide your credentials or a subscriptions manifest again upon upgrading to Tower 3.8. See Attaching Subscriptions for detail.

Supported Operating Systems:

* Red Hat Enterprise Linux 8.2 or later 64-bit (x86)
* Red Hat Enterprise Linux 7.7 or later 64-bit (x86)
* CentOS 7.7 or later 64-bit (x86)

Minimal Requirements:

* A currently supported version of Mozilla Firefox or Google Chrome
* 2 CPUs minimum for Automation Platform installations.
* 4 GB RAM minimum for Automation Platform installations

For specific RAM needs, refer to the capacity algorithm section of the Ansible Tower User Guide for determining capacity required based on the number of forks in your particular configuration

* 20 GB of dedicated hard disk space for Tower service nodes
* 10 GB of the 20 GB requirement must be dedicated to /var/, where Tower stores its files and working directories

The storage volume should be rated for a minimum baseline of 750 IOPS:

* 20 GB of dedicated hard disk space for nodes containing a database
* 64-bit support required (kernel and runtime)
* PostgreSQL version 10 required to run Ansible Tower
* Ansible version 2.9 required to run Ansible Tower


