In order for the bastion host to be prepared it needs:
* RHEL installed (see document prerequisite)
* http server prepared
* ansible user prepared
* cmd genisoimage available

For ease of preparation we created a playbook, which is expected to be run from cmd-line at the Tower server: 

'''
su - awx
git clone <your git repository>
cd <your git repository name>
ansible-playbook -i invetory/hosts --ask-pass prep_bastion_host.yml
'''
This will change your current user to awx, as we do mislike running as root
clone your repository 
change into the directoy the cloning created
and run the playbook by asking for the root password of the basiton host

On th host itself we assure the following (via the playbook):

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

