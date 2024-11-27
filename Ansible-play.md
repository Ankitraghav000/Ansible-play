![Ansible-Playbook](https://github.com/user-attachments/assets/9ec5c571-b9fd-497b-b584-bef205fa8a83)
# what is Ansible ?

Ansible is a simple tool that helps manage and automate tasks like software installation on multiple computers at once. Think of it like a remote control for servers or systems.
## simple breakdown of how Ansible works:

   ### Automation:
   Ansible allows you to automate repetitive tasks. Instead of manually configuring each computer or server, you can write a set of instructions (called a "playbook") that tells Ansible what to do.

   ### Playbooks: 
   These are files where you write your instructions in a simple, human-readable format. You can think of a playbook as a recipe that describes the steps needed to achieve a specific goal, like installing software or updating settings.

   ### Inventory:
   Ansible needs to know which computers or servers you want to manage. You create an inventory file that lists all the machines (like a contact list) that Ansible will work with.

   ### Agentless:
   One of the great things about Ansible is that it doesn’t require any special software to be installed on the computers you manage. It connects to them over the network using standard protocols, which makes it easier to use.

   ### Modules:
   Ansible has many built-in tools (called modules) that perform specific tasks, like copying files, installing software, or checking system status. You can use these modules in your playbooks to accomplish various tasks.

   ### Idempotent:
   This means that if you run the same playbook multiple times, it will only make changes if something needs to be changed. For example, if you already installed a program, running the playbook again won’t reinstall it.
   ![ansible-cover](https://github.com/user-attachments/assets/c2f02193-dd85-463a-9367-3e75592e8eb3)
## playbook 

A playbook in Ansible is a file written in YAML that automates tasks on remote computers (servers). It defines a set of instructions (called tasks) that Ansible will run on the specified systems (called hosts). Each task performs an action, such as installing software or configuring settings.
### How It Works:

**Hosts:**
   The playbook specifies which servers to target.
   
   **Tasks:**
   It lists the steps (like installing software) to execute on those servers.
   
**Execution:**
   When you run the playbook, Ansible connects to the remote servers and carries out each task one by one.
   
**Reporting:** 
   After each task, Ansible tells you if it was successful, failed, or skipped.

## **Objective**

- Use the specified inventory file.
- Login as the user rh-user.
- Set up a directory at /home/rh-user/stc-code.
- Define all necessary variables for paths.
- Backing up the existing JAR file on the RHBK servers to /backup/\<timestamp>.
- Copying the new JAR file to the /Downloads/rhbk/providers directory on both RHBK servers.
- Running the kc.sh build script on the RHBK servers.
- Implement a curl command to display the service status
- Restarting the RHBK servers.
- Implement a curl command to display the service status 
- Change the RHBK service to a notifier (using Ansible handlers).

 ## **I have a three server:-**

1. Ansible server

2. first-rhbk

3. second_rhbk


**Ansible server**

\*OS details:\*\
PRETTY\_NAME="Ubuntu 22.04.5 LTS"\
NAME="Ubuntu"\
VERSION\_ID="22.04"\
VERSION="22.04.5 LTS (Jammy Jellyfish)"\
VERSION\_CODENAME=jammy\
ID=ubuntu\
ID\_LIKE=debian\
HOME\_URL="https\://www\.ubuntu.com/"\
SUPPORT\_URL="https\://help.ubuntu.com/"\
BUG\_REPORT\_URL="https\://bugs.launchpad.net/ubuntu/"\
PRIVACY\_POLICY\_URL="https\://www\.ubuntu.com/legal/terms-and-policies/privacy-policy"\
UBUNTU\_CODENAME=jammy


- This command enables your Ubuntu system to install and update Ansible from the official Ansible PPA(Personal Package Archive) repository.

```
sudo apt-add-repository ppa:ansible/ansible
```
**Output:**
```
rh-user@s:~$ sudo apt-add-repository ppa:ansible/ansible
Repository: 'deb https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ jammy main'
Description:
Ansible is a radically simple IT automation platform that makes your applications and systems easier to deploy. Avoid writing scripts or custom code to deploy and update your applications— automate in a language that approaches plain English, using SSH, with no agents to install on remote systems.

http://ansible.com/

If you face any issues while installing Ansible PPA, file an issue here:
https://github.com/ansible-community/ppa/issues
More info: https://launchpad.net/~ansible/+archive/ubuntu/ansible
Adding repository.
Press [ENTER] to continue or Ctrl-c to cancel.
Found existing deb entry in /etc/apt/sources.list.d/ansible-ubuntu-ansible-jammy.list
Adding deb entry to /etc/apt/sources.list.d/ansible-ubuntu-ansible-jammy.list
Found existing deb-src entry in /etc/apt/sources.list.d/ansible-ubuntu-ansible-jammy.list
Adding disabled deb-src entry to /etc/apt/sources.list.d/ansible-ubuntu-ansible-jammy.list
Adding key to /etc/apt/trusted.gpg.d/ansible-ubuntu-ansible.gpg with fingerprint 6125E2A8C77F2818FB7BD15B93C4A3FD7BB9C367
Hit:1 https://download.docker.com/linux/ubuntu jammy InRelease
Hit:2 http://in.archive.ubuntu.com/ubuntu jammy InRelease                                                                                            
Hit:3 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease                                                                                    
Hit:4 http://security.ubuntu.com/ubuntu jammy-security InRelease                                       
Hit:5 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease 
Hit:6 https://ppa.launchpadcontent.net/ansible/ansible/ubuntu jammy InRelease
Reading package lists... Done
```

- Refreshes the package lists from repositories to get the latest available versions.

```
sudo apt update
```
**Output:**
```
rh-user@s:~$ sudo apt update
Hit:1 https://download.docker.com/linux/ubuntu jammy InRelease
Hit:2 http://security.ubuntu.com/ubuntu jammy-security InRelease                                            
Hit:3 http://in.archive.ubuntu.com/ubuntu jammy InRelease                                                   
Hit:4 https://ppa.launchpadcontent.net/ansible/ansible/ubuntu jammy InRelease     
Hit:5 http://in.archive.ubuntu.com/ubuntu jammy-updates InRelease
Hit:6 http://in.archive.ubuntu.com/ubuntu jammy-backports InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
7 packages can be upgraded. Run 'apt list --upgradable' to see them.
rh-user@s:~$ 
```
- Install the Ansible
```
sudo apt install ansible
```
**Output:**
```
rh-user@s:~$ sudo apt install ansible
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ansible is already the newest version (10.6.0-1ppa~jammy).
0 upgraded, 0 newly installed, 0 to remove and 7 not upgraded.
```

- Check the version of Ansible
```
ansible --version
```
**Output:**
```
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~/playbook$ ansible --version
ansible [core 2.17.6]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/rhuser/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/rhuser/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Nov  6 2024, 20:22:13) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

- Generates a new pair of SSH public and private keys for secure authentication
```
ssh-keygen
```
**Output:**
```
rh-user@s:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/rh-user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/rh-user/.ssh/id_rsa
Your public key has been saved in /home/rh-user/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:OkvnzCkoHaowqZ+AgNgFkjhjmqmzFYOef01/L6HSok8 rh-user@s
The key's randomart image is:
+---[RSA 3072]----+
|o..              |
|=o .             |
|o*  .            |
|B.o.             |
|* oo    S        |
|=+. .  o   .     |
|*+.o o*E+ . .    |
|+oooooo@ = o     |
|ooo...+oB . o.   |
+----[SHA256]-----+

```
- Copies your SSH public key to the remote server[change the server name and IP]
```
ssh-copy-id rh-user@192.168.122.37
```
**Output:**
```
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ ssh-copy-id rh-user@192.168.122.37
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/rh-user/.ssh/id_rsa.pub"
The authenticity of host '192.168.122.37 (192.168.122.37)' can't be established.
ED25519 key fingerprint is SHA256:amwCcbxX8qMMgavbCumvaYG3MT01fIK4rShGDMJLPME.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
rh-user@192.168.122.37's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'rh-user@192.168.122.37'"
and check to make sure that only the key(s) you wanted were added.
```
- Copies your SSH public key to the second remote server[change the server name and IP]

```
ssh-copy-id rh-user@192.168.1.242
```
- Installs the Python package manager pip, which is required for Ansible to manage and install Python dependencies 
```
sudo apt install python3-pip
```
**Output:**
```
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ sudo apt install python3-pip
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
python3-pip is already the newest version (22.0.2+dfsg-1ubuntu0.5).
0 upgraded, 0 newly installed, 0 to remove and 4 not upgraded.
```



