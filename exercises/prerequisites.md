# Prerequisites

This workshop will need the following prerequisites:

## Virtual Machines needed

You will need to prepare the following virtual machines: 
- **OneView Virtual Machine:** Please make sure this virtual machine is already installed.

- **Ansible Tower Virtual Machine** : This workshop will guide you to install a  RHEL 8.2 or later and help installing Red Hat Ansible Tower if not yet done.   
    
```Hardware Specifications:
    - 4 vCPUs
    - 10 GB Mem
    - 250 GB HDD
    - RHEL ISO image 
    - Networking
```
- **Bastion Host Virtual Machine** :  This workshop will guide you to install a  RHEL 8.2 or later and help configure this machine as bastion, if not yet done. 

```Hardware Specifications:
    - 2 vCPUs
    - 4 GB Mem (2 würden auch reichen)
    - 100 GB HDD
    - RHEL ISO image 
    - Networking
```

## Target Physical Server: 

You will need a HPE Gen 10 physical  Server on which we will automated ESXi installation. Ultimately, it should be connected to the management network and pre-configured on HPE OneView.
 

## RHEL ISO image needed

We will this ISO to deploy all the needed RHEL instances. In the Scope of this workshop we didn't plan to automate the installation of RHEL. Please choose the following guide when installing manualy:

- Fetch an RHEL installation image from: [HERE](https://access.redhat.com/products/red-hat-enterprise-linux)
- Login with Red Hat Customer Portal Credentials
- Download RHEL 8.4
- Click on Red Hat Enterprise Linux 8.4 Binary DVD

**Hint**: you can copy the Link from the "Downlad Now" button and paste it directly into the download cmd / system which needs to have the ISO at the end.
          e.g. download directly onto your ESXi server or a repository server in your location.

## RHEL Installation
Boot from ISO image downloaded previously

- **Localization**
   * Language: English      << We recommend English environment to be compatible when talking to peers or reading English documentation
   * KEYBOARD: German (MAC) << please choose th keyboard which fits best to your environment
   * Timezone: Europe/Berlin 

- **Installation Destination**
   * leave VMware Virtual Disk selected - it connects to the RHEL iso image
   * Storage Configuration: Automatic

- **Network & Hostname**
   * Switch Ethernet ON    << this gets easily forgotten!!!!!
   * set HOSTNAME          << this also gets easily forgotten, rendering all systems to be named "localhost"
   * controller.example.com

- **Connect To Red Hat**
   * credentials to your RHN account needed

- **Software Selection**
   * Minimal Install (for Ansible Tower)
   * Server          (for the Bastion host)

- **Root Password**
  * xxxxxxxx     (if you choose the standard learning password it is called weak and you need to press done twice)

- **User Creation**
  * Add a user as you wish
  * Make this user administrator

For the Bastion host we will later create a user "ansible" as well.

Begin Installation
