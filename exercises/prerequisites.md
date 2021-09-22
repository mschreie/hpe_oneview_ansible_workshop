# Prerequisites

This workshop will need following prerequisites:

## VMs needed
Following virtual machines need to be available
- OneView VM 
  I'd love to see this installed already.

- Ansible Tower VM
  10 GB Mem
  250 GB HDD
  4 vCPUs
  RHEL ISO image 
  Networking
  We will install RHEL 8.2 or newer jointly, if not done already

- Bastion Host VM
  4 GB Mem (2 würden auch reichen)
  100 GB HDD
  2 vCPUs
  RHEL ISO image 
  Networking
  We will install RHEL 8.2 or newer jointly, if not done already

## Installation target needed
Additionaly we need a HPE Gen 10 Server which can be deployed onto.


## RHEL ISO image needed
We did not automate the RHEL installation process so please choose the following when installing manualy:

Fetch an RHEL installation image from:
https://access.redhat.com/products/red-hat-enterprise-linux => Login => Downloard RHEL 8.4 => 
Red Hat Enterprise Linux 8.4 Binary DVD

Hint: you can copy the Link from the "Downlad Now" button and paste it directly into the download cmd / system which needs to have the ISO at the end.
E.g. download directly onto your ESXi server or a repository server in your location.

## RHEL installation
Boot from ISO image downloaded previously

Localization
* Language: English      << i prefere English environment to be compatible when talking to peers or reading English documentation
* KEYBOARD: German (MAC) << please choose th keyboard which fits best to your environment
* Timezone: Europe/Berlin 

Installation Destination
* leave VMware Virtual Disk selected - it connects to the RHEL iso image
* Storage Configuration: Automatic

Network & Hostname
* switch Ethernet ON    << this gets easily forgotten!!!!!
* set HOSTNAME          << this also gets easily forgotten, rendering all systems to be named "localhost"
    * controller.example.com

Connect To Red Hat
* credentials to your RHN account needed

SW Selection:
* Minimal Install (for Ansible Tower)
* Server          (for the Bastion host)
Root Password:
* xxxxxxxx        (if you choose the standard learning password it is called weak and you need to press done twice)

User Creation
  Add a user as you wish
* Make this user administrator
For the Bastion host we will later create a user "ansible" as well.

Beginn Installation
