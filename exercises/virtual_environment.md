# Setting up Virtual Environment


## Setting up Ansible Tower Preparation:

After the installation of Red Hat Ansible Tower (Controller), you will need to finalize a number of tasks in order to be ready for the workshop.

### Register Red Hat Ansible Tower (Controller) .

1. Ansible Tower (Controller) has to be registered to Red Hat Network. For that you'll need : **Red Hat Account credentials** or Red Hat Subscription Manifest a manifest file. Nagivate to **Settings** Then **LICENSE**. Use the mean of registration as follows (Type in the credentials or upload the manifest).

![Ansible_Tower Registration](/images/register-tower.png)

2. Configure Ansible Tower (Controller) to download Ansible content collections:

In the illustration below you will see a typical reference design to download Ansible Content Collections from Red Hat. There a Private Automation Hub that will help to publish Content collection locally.

![Ansible_Tower](/images/ansible-workshop-illustration-01.png)


For the sake of the simplicity of this workshop, will not install Private Automation Hub. We will connect Ansible Tower directly to the Cloud Services of Red Hat  called : Automation Hub.

Below additiona Terminology to understand further the concepts:

   **a. Certified Content**: In the portal of Automation Hub, users have direct access to certified content collections from Red Hat and Partners. Certified collections are developed, tested, built, delivered, and supported by Red Hat and its Partners. To find more details about the scope of support, check the [Ansible Certified Content FAQ](https://access.redhat.com/articles/4916901).

   **b. Supported Automation**: Automation Hub is a one-stop-shop for Ansible content that is backed by support from Red Hat to deliver additional reassurance for customers. Additional supportability claims for these collections may be provided under the “Maintained and Supported By” one of Red Hat Partners. A list of currently supported content can be found in the Knowledge base.
   
   **c. Ansible Galaxy**: Ansible Galaxy is the upstream location for the Ansible community that initially started to provide pre-packaged units of work known as Ansible roles. Roles can be used from Ansible Playbooks and immediately put to work. in a recent version of Galaxy started to provide Ansible content collections as well.
Ansible Galaxy resides on https://galaxy.ansible.com/

   **d. Accessing collections from Automation Hub**: Ansible collections can be used and downloaded from multiple locations. They can either be downloaded using a requirement file, statically included in the git repository or eventually installed separately in the virtual environment.

## Step 1:Authenticate Ansible Tower to Automation Hub
Creating a token
Authenticating Ansible Tower requires a token. It can be achieved using the steps below:

 1) Navigate to [https://cloud.redhat.com/ansible/automation-hub/token/](https://cloud.redhat.com/ansible/automation-hub/token/)
 
![Create_Token](/images/create-token.png)
          
 2) Click Load Token.
 3) Click copy icon to copy the API token to the clipboard.


![Copy_Token](/images/copy-token.png)


Using authentication token as user admin, navigate to the **Credentials > New Credentials** as follows <br>

![Use_Token](/images/save-token.png)


As soon as you create the Automation Hub credentials, it is time to attached these credentials to the right organization
Navigate to the **Organizations > Default** as follows <br>

**/!\ Please make sure to attache Automation Hub Credentials, then Ansible Galaxy credentials to set the precedence where to look for Ansible Content Collections.**


![Attach_Token](/images/attach-token.png)

## Step 2: Virtual Environment

The Ansible virtualenv is very easy to manage. You will need access to the machine where Ansible Tower is installed. It is also recommended that you configure permissions properly before making any changes to the virtualenv.

An example of the recommended permissions can be found below and in the docs here:

```
# source /var/lib/awx/venv/ansible/bin/activate
# umask 0022
```
 ## Step 3: Create a new Virtual Environment

Create a virtual environment using the python -m venv &lt;environment-name> command. You can give any name to your Python virtual environment.

```
# cd /var/lib/awx/venv/
# python3 -m venv hpe_venv
```

## Step 4: Activate a Python virtual environment

After creating a virtual environment, you must enter the environment manually. This changes your active environment variables from your current shell to those required for Python to create a virtual environment:

```
# source hpe_venv/bin/activate
(hpe_venv) # python3 -V
Python 3.6.8
```


## Step 5: Install HPE required SDKs in a virtual environment

With your virtual environment set up and active, you can install the HPE sdks required for this use-case. In this workshop we will focus on :
  - HPE OneView
  - HPE ILO

To Install HPE OneView SDK, there are three ways to do so. In this workshop we will rely on pip to do so. Further details can be found on HPE official github repository in the link below :

[https://github.com/HewlettPackard/oneview-python](https://github.com/HewlettPackard/oneview-python)


```
(hpe_venv) $ pip install hpeOneView hpICsp python-hpilo python-ilorest-library psutil
Collecting hpOneView
  Downloading https://files.pythonhosted.org/packages/75/0c/a932041b58827c04cf3a565e0ace692e75f731d368e532ec4d484c870030/hpOneView-5.3.0.tar.gz (81kB)
    100% |████████████████████████████████| 81kB 3.6MB/s 
Collecting hpICsp
  Cache entry deserialization failed, entry ignored
  Using cached https://files.pythonhosted.org/packages/16/7d/22dcc6291808384bcc3bae3d50100662c607456695841aa48dedd3d8e445/hpICsp-1.0.2.tar.gz
Collecting python-ilorest-library
  Downloading https://files.pythonhosted.org/packages/0c/28/1a1c54e23b70d1783d67ea3b4616ef87367a5af203c1bb849f5bb53018ff/python-ilorest-library-3.2.2.zip (86kB)
    100% |████████████████████████████████| 92kB 6.9MB/s 
Requirement already satisfied: future>=0.15.2 in ./hpe_venv/lib/python3.6/site-packages (from hpOneView)
Collecting jsonpatch (from python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/a3/55/f7c93bae36d869292aedfbcbae8b091386194874f16390d680136edd2b28/jsonpatch-1.32-py2.py3-none-any.whl
Collecting jsonpath_rw (from python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/71/7c/45001b1f19af8c4478489fbae4fc657b21c4c669d7a5a036a86882581d85/jsonpath-rw-1.4.0.tar.gz
Collecting jsonpointer (from python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/23/52/05f67532aa922e494c351344e0d9624a01f74f5dd8402fe0d1b563a6e6fc/jsonpointer-2.1-py2.py3-none-any.whl
Collecting urllib3 (from python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/5f/64/43575537846896abac0b15c3e5ac678d787a4021e906703f1766bfb8ea11/urllib3-1.26.6-py2.py3-none-any.whl (138kB)
    100% |████████████████████████████████| 143kB 5.5MB/s 
Collecting six (from python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl
Collecting ply (from jsonpath_rw->python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/a3/58/35da89ee790598a0700ea49b2a66594140f44dec458c07e8e3d4979137fc/ply-3.11-py2.py3-none-any.whl (49kB)
    100% |████████████████████████████████| 51kB 9.2MB/s 
Collecting decorator (from jsonpath_rw->python-ilorest-library)
  Downloading https://files.pythonhosted.org/packages/3d/cc/d7b758e54779f7e465179427de7e78c601d3330d6c411ea7ba9ae2f38102/decorator-5.1.0-py3-none-any.whl
Installing collected packages: hpOneView, hpICsp, jsonpointer, jsonpatch, ply, decorator, six, jsonpath-rw, urllib3, python-ilorest-library
  Running setup.py install for hpOneView ... done
  Running setup.py install for hpICsp ... done
  Running setup.py install for jsonpath-rw ... done
  Running setup.py install for python-ilorest-library ... done
Successfully installed decorator-5.1.0 hpICsp-1.0.2 hpOneView-5.3.0 jsonpatch-1.32 jsonpath-rw-1.4.0 jsonpointer-2.1 ply-3.11 python-ilorest-library-3.2.2 six-1.16.0 urllib3-1.26.6

```

To check the installed the installed libraries, you can run the following command below:

```
(hpe_venv) $ pip freeze 

```

To deactive the virtual environment, you can run the command deactivate:

```
(hpe_venv) $ deactivate 
[root@tower venv]# 

```

*Use Environment in Tower:* <br>
When creating a Job-Templte in Tower you can choose which virtual environment to use there.

Further details about Virtual Environment for Red Hat Ansible Tower (Controller) can be found in this link :
https://docs.ansible.com/ansible-tower/latest/html/upgrade-migration-guide/virtualenv.html
