# Virtual Environment


## Setting up Ansible Tower Preparation:

After the installation of Red Hat Ansible Tower (Controller), you will need to finalize a number of tasks in order to be ready for the workshop.

### Register Red Hat Ansible Tower (Controller) .

![Ansible_Tower](/images/ansible-workshop-illustration-04.png)


1- Ansible Tower (Controller) has to be registered to Red Hat Network. For that you'll need : Red Hat Account credentials or Red Hat Subscription Manifest a manifest file.

2- Configure Ansible Tower (Controller) to download Ansible content collections from :

  a- Ansible Automation Hub : It is the official location to discover and download supported collections, included as part of an Ansible Automation Platform subscription. These content collections contain modules, plugins, roles, and playbooks in a downloadable package.

Ansible Automation Hub gives you direct access to trusted content collections from Red Hat and Certified Partners. You can find content by topic or Ansible Partner organizations.


  b- Red Hat Galaxy:
  

## Virtual Environment

The Ansible virtualenv is very easy to manage. You will need access to the machine where Ansible Tower is installed. It is also recommended that you configure permissions properly before making any changes to the virtualenv.

An example of the recommended permissions can be found below and in the docs here:

```
# source /var/lib/awx/venv/ansible/bin/activate
# umask 0022
```
 ## Create a new Virtual Environment

Create a virtual environment using the python -m venv &lt;environment-name> command. You can give any name to your Python virtual environment.

```
# cd /var/lib/awx/venv/
# python3 -m venv hpe_venv
```

## Activate a Python virtual environment

After creating a virtual environment, you must enter the environment manually. This changes your active environment variables from your current shell to those required for Python to create a virtual environment:

```
# source hpe_venv/bin/activate
(hpe_venv) # python3 -V
Python 3.6.8
```


## Install HPE required SDKs in a virtual environment

With your virtual environment set up and active, you can install the HPE sdks required for this use-case. In this workshop we will focus on :
  - HPE OneView
  - HPE ILO

To Install HPE OneView SDK, there are three ways to do so. In this workshop we will rely on pip to do so. Further details can be found on HPE official github repository in the link below :

[https://github.com/HewlettPackard/oneview-python](https://github.com/HewlettPackard/oneview-python)


```
(hpe_venv) $ pip install hpOneView hpICsp python-ilorest-library
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

Further details about Virtual Environment for Red Hat Ansible Tower (Controller) can be found in this link :
https://docs.ansible.com/ansible-tower/latest/html/upgrade-migration-guide/virtualenv.html
