# Prepare Bastion Host

As it is best practice to not mess arround with the Tower Server itself, we set up an additional server where we can run things as root, can install additiona SW easily and can run a web-server without interfearing with Tower web interface.

In order for the bastion host to be prepared it needs:
1* RHEL installed (see document prerequisite)
2* http server prepared
3* ansible user prepared
4* cmd genisoimage available
5* VMware ESXi installer image available

For ease of preparation we created a playbook, which is expected to be run from cmd-line at the Tower server. It automates steps 2, 3 and 4.

## Install RHEL
This is explained in the prerequisite.md

## Prepare Bastion Host (steps 2 to 4)

Auf dem Tower:
```
cd /var/lib/awx
git clone <your git repository>
cd <your git repository name>
```
Bitte das inventory anpassen (also die IP-Adressen) in
./inventory/hosts

```
ansible-playbook -e ansible_user=root -i invetory/hosts --ask-pass prep_bastion_host.yml
```
This will change your current user to awx, as we do mislike running as root
clone your repository 
change into the directoy the cloning created
and run the playbook by asking for the root password of the basiton host

On the host itself we assure the following (via the playbook):
Hint: Do not run the following commands, when you have already executed the playbook.

```
# yum install -y httpd
# systemctl enable httpd
# systemctl start httpd
# firewall-cmd --add-service=http
# firewall-cmd --add-service=https
# useradd -m ansible
# passwd ansible
# usermod -a -G wheel ansible
# yum install -y genisoimage
# visudo    <<< we change the suduers file to look similar to this:
# grep wheel /etc/sudoers
## Allows people in group wheel to run all commands
#%wheel	ALL=(ALL)	ALL
%wheel	ALL=(ALL)	NOPASSWD: ALL
```

## VMware ESXi installer image available
The ESXi installer images need to be put onto the bastion host at 
/var/www/html/isos/
you can use wget on the bastion host itself to download
```
cd /var/www/html/isos
wget https://someplace/to/fetch/iso/from
```

or you can scp the file to the bastion host 

```
scp file.iso root@ipofbastionhost:/var/www/html/isos/
```

## Check result
browse to
http://bastion-hostsip-address/isos/
