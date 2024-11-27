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
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ sudo apt-add-repository ppa:ansible/ansible
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
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ sudo apt update
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
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ sudo apt install ansible -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following NEW packages will be installed:
  ansible
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 17.6 MB of archives.
After this operation, 199 MB of additional disk space will be used.
Get:1 https://ppa.launchpadcontent.net/ansible/ansible/ubuntu jammy/main amd64 ansible all 10.6.0-1ppa~jammy [17.6 MB]
Fetched 17.6 MB in 41s (424 kB/s)                                                                                                                    
Selecting previously unselected package ansible.
(Reading database ... 255252 files and directories currently installed.)
Preparing to unpack .../ansible_10.6.0-1ppa~jammy_all.deb ...
Unpacking ansible (10.6.0-1ppa~jammy) ...
Setting up ansible (10.6.0-1ppa~jammy) ...

```

- Check the version of Ansible
```
ansible --version
```
**Output:**
```
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ ansible --version
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
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ ssh-keygen
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
The following additional packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu build-essential dpkg-dev
  fakeroot g++ g++-11 gcc gcc-11 javascript-common libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libasan6 libbinutils
  libc-dev-bin libc-devtools libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0
  libctf0 libdpkg-perl libexpat1-dev libfakeroot libfile-fcntllock-perl
  libgcc-11-dev libitm1 libjs-jquery libjs-sphinxdoc libjs-underscore liblsan0
  libnsl-dev libpython3-dev libpython3.10-dev libquadmath0 libstdc++-11-dev
  libtirpc-dev libtsan0 libubsan1 linux-libc-dev lto-disabled-list make
  manpages-dev python3-dev python3-distutils python3-setuptools python3-wheel
  python3.10-dev rpcsvc-proto zlib1g-dev
Suggested packages:
  binutils-doc debian-keyring g++-multilib g++-11-multilib gcc-11-doc
  gcc-multilib autoconf automake libtool flex bison gcc-doc gcc-11-multilib
  gcc-11-locales glibc-doc git bzr libstdc++-11-doc make-doc
  python-setuptools-doc
The following NEW packages will be installed:
  binutils binutils-common binutils-x86-64-linux-gnu build-essential dpkg-dev
  fakeroot g++ g++-11 gcc gcc-11 javascript-common libalgorithm-diff-perl
  libalgorithm-diff-xs-perl libalgorithm-merge-perl libasan6 libbinutils
  libc-dev-bin libc-devtools libc6-dev libcc1-0 libcrypt-dev libctf-nobfd0
  libctf0 libdpkg-perl libexpat1-dev libfakeroot libfile-fcntllock-perl
  libgcc-11-dev libitm1 libjs-jquery libjs-sphinxdoc libjs-underscore liblsan0
  libnsl-dev libpython3-dev libpython3.10-dev libquadmath0 libstdc++-11-dev
  libtirpc-dev libtsan0 libubsan1 linux-libc-dev lto-disabled-list make
  manpages-dev python3-dev python3-distutils python3-pip python3-setuptools
  python3-wheel python3.10-dev rpcsvc-proto zlib1g-dev
0 upgraded, 53 newly installed, 0 to remove and 3 not upgraded.
Need to get 62.2 MB of archives.
After this operation, 221 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 binutils-common amd64 2.38-4ubuntu2.6 [222 kB]
Get:2 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libbinutils amd64 2.38-4ubuntu2.6 [662 kB]
Get:3 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libctf-nobfd0 amd64 2.38-4ubuntu2.6 [108 kB]
Get:4 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libctf0 amd64 2.38-4ubuntu2.6 [103 kB]
Get:5 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 binutils-x86-64-linux-gnu amd64 2.38-4ubuntu2.6 [2,326 kB]
Get:6 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 binutils amd64 2.38-4ubuntu2.6 [3,200 B]
Get:7 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libc-dev-bin amd64 2.35-0ubuntu3.8 [20.3 kB]
Get:8 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 linux-libc-dev amd64 5.15.0-126.136 [1,334 kB]
Get:9 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libcrypt-dev amd64 1:4.4.27-1 [112 kB]
Get:10 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 rpcsvc-proto amd64 1.4.2-0ubuntu6 [68.5 kB]
Get:11 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libtirpc-dev amd64 1.3.2-2ubuntu0.1 [192 kB]
Get:12 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libnsl-dev amd64 1.3.0-2build2 [71.3 kB]
Get:13 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libc6-dev amd64 2.35-0ubuntu3.8 [2,100 kB]
Get:14 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libcc1-0 amd64 12.3.0-1ubuntu1~22.04 [48.3 kB]
Get:15 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libitm1 amd64 12.3.0-1ubuntu1~22.04 [30.2 kB]
Get:16 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libasan6 amd64 11.4.0-1ubuntu1~22.04 [2,282 kB]
Get:17 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 liblsan0 amd64 12.3.0-1ubuntu1~22.04 [1,069 kB]
Get:18 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libtsan0 amd64 11.4.0-1ubuntu1~22.04 [2,260 kB]
Get:19 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libubsan1 amd64 12.3.0-1ubuntu1~22.04 [976 kB]
Get:20 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libquadmath0 amd64 12.3.0-1ubuntu1~22.04 [154 kB]
Get:21 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libgcc-11-dev amd64 11.4.0-1ubuntu1~22.04 [2,517 kB]
Get:22 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 gcc-11 amd64 11.4.0-1ubuntu1~22.04 [20.1 MB]
Get:23 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 gcc amd64 4:11.2.0-1ubuntu1 [5,112 B]
Get:24 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libstdc++-11-dev amd64 11.4.0-1ubuntu1~22.04 [2,101 kB]
Get:25 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 g++-11 amd64 11.4.0-1ubuntu1~22.04 [11.4 MB]
Get:26 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 g++ amd64 4:11.2.0-1ubuntu1 [1,412 B]
Get:27 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 make amd64 4.3-4.1build1 [180 kB]
Get:28 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libdpkg-perl all 1.21.1ubuntu2.3 [237 kB]
Get:29 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 lto-disabled-list all 24 [12.5 kB]
Get:30 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 dpkg-dev all 1.21.1ubuntu2.3 [922 kB]
Get:31 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 build-essential amd64 12.9ubuntu3 [4,744 B]
Get:32 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libfakeroot amd64 1.28-1ubuntu1 [31.5 kB]
Get:33 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 fakeroot amd64 1.28-1ubuntu1 [60.4 kB]
Get:34 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 javascript-common all 11+nmu1 [5,936 B]
Get:35 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libalgorithm-diff-perl all 1.201-1 [41.8 kB]
Get:36 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libalgorithm-diff-xs-perl amd64 0.04-6build3 [11.9 kB]
Get:37 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libalgorithm-merge-perl all 0.08-3 [12.0 kB]
Get:38 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libc-devtools amd64 2.35-0ubuntu3.8 [28.9 kB]
Get:39 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libexpat1-dev amd64 2.4.7-1ubuntu0.4 [147 kB]
Get:40 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libfile-fcntllock-perl amd64 0.22-3build7 [33.9 kB]
Get:41 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libjs-jquery all 3.6.0+dfsg+~3.5.13-1 [321 kB]
Get:42 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libjs-underscore all 1.13.2~dfsg-2 [118 kB]
Get:43 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 libjs-sphinxdoc all 4.3.2-1 [139 kB]
Get:44 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 zlib1g-dev amd64 1:1.2.11.dfsg-2ubuntu9.2 [164 kB]
Get:45 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpython3.10-dev amd64 3.10.12-1~22.04.7 [4,762 kB]
Get:46 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpython3-dev amd64 3.10.6-1~22.04.1 [7,064 B]
Get:47 http://in.archive.ubuntu.com/ubuntu jammy/main amd64 manpages-dev all 5.10-1ubuntu1 [2,309 kB]
Get:48 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3.10-dev amd64 3.10.12-1~22.04.7 [508 kB]
Get:49 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3-distutils all 3.10.8-1~22.04 [139 kB]
Get:50 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3-dev amd64 3.10.6-1~22.04.1 [26.0 kB]
Get:51 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 python3-setuptools all 59.6.0-1.2ubuntu0.22.04.2 [340 kB]
Get:52 http://in.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 python3-wheel all 0.37.1-2ubuntu0.22.04.1 [32.0 kB]
Get:53 http://in.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 python3-pip all 22.0.2+dfsg-1ubuntu0.5 [1,306 kB]
Fetched 62.2 MB in 12s (5,336 kB/s)                                            
Extracting templates from packages: 100%
Selecting previously unselected package binutils-common:amd64.
(Reading database ... 208331 files and directories currently installed.)
Preparing to unpack .../00-binutils-common_2.38-4ubuntu2.6_amd64.deb ...
Unpacking binutils-common:amd64 (2.38-4ubuntu2.6) ...
Selecting previously unselected package libbinutils:amd64.
Preparing to unpack .../01-libbinutils_2.38-4ubuntu2.6_amd64.deb ...
Unpacking libbinutils:amd64 (2.38-4ubuntu2.6) ...
Selecting previously unselected package libctf-nobfd0:amd64.
Preparing to unpack .../02-libctf-nobfd0_2.38-4ubuntu2.6_amd64.deb ...
Unpacking libctf-nobfd0:amd64 (2.38-4ubuntu2.6) ...
Selecting previously unselected package libctf0:amd64.
Preparing to unpack .../03-libctf0_2.38-4ubuntu2.6_amd64.deb ...
Unpacking libctf0:amd64 (2.38-4ubuntu2.6) ...
Selecting previously unselected package binutils-x86-64-linux-gnu.
Preparing to unpack .../04-binutils-x86-64-linux-gnu_2.38-4ubuntu2.6_amd64.deb .
..
Unpacking binutils-x86-64-linux-gnu (2.38-4ubuntu2.6) ...
Selecting previously unselected package binutils.
Preparing to unpack .../05-binutils_2.38-4ubuntu2.6_amd64.deb ...
Unpacking binutils (2.38-4ubuntu2.6) ...
Selecting previously unselected package libc-dev-bin.
Preparing to unpack .../06-libc-dev-bin_2.35-0ubuntu3.8_amd64.deb ...
Unpacking libc-dev-bin (2.35-0ubuntu3.8) ...
Selecting previously unselected package linux-libc-dev:amd64.
Preparing to unpack .../07-linux-libc-dev_5.15.0-126.136_amd64.deb ...
Unpacking linux-libc-dev:amd64 (5.15.0-126.136) ...
Selecting previously unselected package libcrypt-dev:amd64.
Preparing to unpack .../08-libcrypt-dev_1%3a4.4.27-1_amd64.deb ...
Unpacking libcrypt-dev:amd64 (1:4.4.27-1) ...
Selecting previously unselected package rpcsvc-proto.
Preparing to unpack .../09-rpcsvc-proto_1.4.2-0ubuntu6_amd64.deb ...
Unpacking rpcsvc-proto (1.4.2-0ubuntu6) ...
Selecting previously unselected package libtirpc-dev:amd64.
Preparing to unpack .../10-libtirpc-dev_1.3.2-2ubuntu0.1_amd64.deb ...
Unpacking libtirpc-dev:amd64 (1.3.2-2ubuntu0.1) ...
Selecting previously unselected package libnsl-dev:amd64.
Preparing to unpack .../11-libnsl-dev_1.3.0-2build2_amd64.deb ...
Unpacking libnsl-dev:amd64 (1.3.0-2build2) ...
Selecting previously unselected package libc6-dev:amd64.
Preparing to unpack .../12-libc6-dev_2.35-0ubuntu3.8_amd64.deb ...
Unpacking libc6-dev:amd64 (2.35-0ubuntu3.8) ...
Selecting previously unselected package libcc1-0:amd64.
Preparing to unpack .../13-libcc1-0_12.3.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libcc1-0:amd64 (12.3.0-1ubuntu1~22.04) ...
Selecting previously unselected package libitm1:amd64.
Preparing to unpack .../14-libitm1_12.3.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libitm1:amd64 (12.3.0-1ubuntu1~22.04) ...
Selecting previously unselected package libasan6:amd64.
Preparing to unpack .../15-libasan6_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libasan6:amd64 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package liblsan0:amd64.
Preparing to unpack .../16-liblsan0_12.3.0-1ubuntu1~22.04_amd64.deb ...
Unpacking liblsan0:amd64 (12.3.0-1ubuntu1~22.04) ...
Selecting previously unselected package libtsan0:amd64.
Preparing to unpack .../17-libtsan0_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libtsan0:amd64 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package libubsan1:amd64.
Preparing to unpack .../18-libubsan1_12.3.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libubsan1:amd64 (12.3.0-1ubuntu1~22.04) ...
Selecting previously unselected package libquadmath0:amd64.
Preparing to unpack .../19-libquadmath0_12.3.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libquadmath0:amd64 (12.3.0-1ubuntu1~22.04) ...
Selecting previously unselected package libgcc-11-dev:amd64.
Preparing to unpack .../20-libgcc-11-dev_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libgcc-11-dev:amd64 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package gcc-11.
Preparing to unpack .../21-gcc-11_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking gcc-11 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package gcc.
Preparing to unpack .../22-gcc_4%3a11.2.0-1ubuntu1_amd64.deb ...
Unpacking gcc (4:11.2.0-1ubuntu1) ...
Selecting previously unselected package libstdc++-11-dev:amd64.
Preparing to unpack .../23-libstdc++-11-dev_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking libstdc++-11-dev:amd64 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package g++-11.
Preparing to unpack .../24-g++-11_11.4.0-1ubuntu1~22.04_amd64.deb ...
Unpacking g++-11 (11.4.0-1ubuntu1~22.04) ...
Selecting previously unselected package g++.
Preparing to unpack .../25-g++_4%3a11.2.0-1ubuntu1_amd64.deb ...
Unpacking g++ (4:11.2.0-1ubuntu1) ...
Selecting previously unselected package make.
Preparing to unpack .../26-make_4.3-4.1build1_amd64.deb ...
Unpacking make (4.3-4.1build1) ...
Selecting previously unselected package libdpkg-perl.
Preparing to unpack .../27-libdpkg-perl_1.21.1ubuntu2.3_all.deb ...
Unpacking libdpkg-perl (1.21.1ubuntu2.3) ...
Selecting previously unselected package lto-disabled-list.
Preparing to unpack .../28-lto-disabled-list_24_all.deb ...
Unpacking lto-disabled-list (24) ...
Selecting previously unselected package dpkg-dev.
Preparing to unpack .../29-dpkg-dev_1.21.1ubuntu2.3_all.deb ...
Unpacking dpkg-dev (1.21.1ubuntu2.3) ...
Selecting previously unselected package build-essential.
Preparing to unpack .../30-build-essential_12.9ubuntu3_amd64.deb ...
Unpacking build-essential (12.9ubuntu3) ...
Selecting previously unselected package libfakeroot:amd64.
Preparing to unpack .../31-libfakeroot_1.28-1ubuntu1_amd64.deb ...
Unpacking libfakeroot:amd64 (1.28-1ubuntu1) ...
Selecting previously unselected package fakeroot.
Preparing to unpack .../32-fakeroot_1.28-1ubuntu1_amd64.deb ...
Unpacking fakeroot (1.28-1ubuntu1) ...
Selecting previously unselected package javascript-common.
Preparing to unpack .../33-javascript-common_11+nmu1_all.deb ...
Unpacking javascript-common (11+nmu1) ...
Selecting previously unselected package libalgorithm-diff-perl.
Preparing to unpack .../34-libalgorithm-diff-perl_1.201-1_all.deb ...
Unpacking libalgorithm-diff-perl (1.201-1) ...
Selecting previously unselected package libalgorithm-diff-xs-perl.
Preparing to unpack .../35-libalgorithm-diff-xs-perl_0.04-6build3_amd64.deb ...
Unpacking libalgorithm-diff-xs-perl (0.04-6build3) ...
Selecting previously unselected package libalgorithm-merge-perl.
Preparing to unpack .../36-libalgorithm-merge-perl_0.08-3_all.deb ...
Unpacking libalgorithm-merge-perl (0.08-3) ...
Selecting previously unselected package libc-devtools.
Preparing to unpack .../37-libc-devtools_2.35-0ubuntu3.8_amd64.deb ...
Unpacking libc-devtools (2.35-0ubuntu3.8) ...
Selecting previously unselected package libexpat1-dev:amd64.
Preparing to unpack .../38-libexpat1-dev_2.4.7-1ubuntu0.4_amd64.deb ...
Unpacking libexpat1-dev:amd64 (2.4.7-1ubuntu0.4) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../39-libfile-fcntllock-perl_0.22-3build7_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-3build7) ...
Selecting previously unselected package libjs-jquery.
Preparing to unpack .../40-libjs-jquery_3.6.0+dfsg+~3.5.13-1_all.deb ...
Unpacking libjs-jquery (3.6.0+dfsg+~3.5.13-1) ...
Selecting previously unselected package libjs-underscore.
Preparing to unpack .../41-libjs-underscore_1.13.2~dfsg-2_all.deb ...
Unpacking libjs-underscore (1.13.2~dfsg-2) ...
Selecting previously unselected package libjs-sphinxdoc.
Preparing to unpack .../42-libjs-sphinxdoc_4.3.2-1_all.deb ...
Unpacking libjs-sphinxdoc (4.3.2-1) ...
Selecting previously unselected package zlib1g-dev:amd64.
Preparing to unpack .../43-zlib1g-dev_1%3a1.2.11.dfsg-2ubuntu9.2_amd64.deb ...
Unpacking zlib1g-dev:amd64 (1:1.2.11.dfsg-2ubuntu9.2) ...
Selecting previously unselected package libpython3.10-dev:amd64.
Preparing to unpack .../44-libpython3.10-dev_3.10.12-1~22.04.7_amd64.deb ...
Unpacking libpython3.10-dev:amd64 (3.10.12-1~22.04.7) ...
Selecting previously unselected package libpython3-dev:amd64.
Preparing to unpack .../45-libpython3-dev_3.10.6-1~22.04.1_amd64.deb ...
Unpacking libpython3-dev:amd64 (3.10.6-1~22.04.1) ...
Selecting previously unselected package manpages-dev.
Preparing to unpack .../46-manpages-dev_5.10-1ubuntu1_all.deb ...
Unpacking manpages-dev (5.10-1ubuntu1) ...
Selecting previously unselected package python3.10-dev.
Preparing to unpack .../47-python3.10-dev_3.10.12-1~22.04.7_amd64.deb ...
Unpacking python3.10-dev (3.10.12-1~22.04.7) ...
Selecting previously unselected package python3-distutils.
Preparing to unpack .../48-python3-distutils_3.10.8-1~22.04_all.deb ...
Unpacking python3-distutils (3.10.8-1~22.04) ...
Selecting previously unselected package python3-dev.
Preparing to unpack .../49-python3-dev_3.10.6-1~22.04.1_amd64.deb ...
Unpacking python3-dev (3.10.6-1~22.04.1) ...
Selecting previously unselected package python3-setuptools.
Preparing to unpack .../50-python3-setuptools_59.6.0-1.2ubuntu0.22.04.2_all.deb 
...
Unpacking python3-setuptools (59.6.0-1.2ubuntu0.22.04.2) ...
Selecting previously unselected package python3-wheel.
Preparing to unpack .../51-python3-wheel_0.37.1-2ubuntu0.22.04.1_all.deb ...
Unpacking python3-wheel (0.37.1-2ubuntu0.22.04.1) ...
Selecting previously unselected package python3-pip.
Preparing to unpack .../52-python3-pip_22.0.2+dfsg-1ubuntu0.5_all.deb ...
Unpacking python3-pip (22.0.2+dfsg-1ubuntu0.5) ...
Setting up python3-distutils (3.10.8-1~22.04) ...
Setting up javascript-common (11+nmu1) ...
Setting up manpages-dev (5.10-1ubuntu1) ...
Setting up lto-disabled-list (24) ...
Setting up python3-setuptools (59.6.0-1.2ubuntu0.22.04.2) ...
Setting up libfile-fcntllock-perl (0.22-3build7) ...
Setting up libalgorithm-diff-perl (1.201-1) ...
Setting up binutils-common:amd64 (2.38-4ubuntu2.6) ...
Setting up linux-libc-dev:amd64 (5.15.0-126.136) ...
Setting up libctf-nobfd0:amd64 (2.38-4ubuntu2.6) ...
Setting up python3-wheel (0.37.1-2ubuntu0.22.04.1) ...
Setting up libfakeroot:amd64 (1.28-1ubuntu1) ...
Setting up libasan6:amd64 (11.4.0-1ubuntu1~22.04) ...
Setting up fakeroot (1.28-1ubuntu1) ...
update-alternatives: using /usr/bin/fakeroot-sysv to provide /usr/bin/fakeroot (
fakeroot) in auto mode
Setting up libtirpc-dev:amd64 (1.3.2-2ubuntu0.1) ...
Setting up rpcsvc-proto (1.4.2-0ubuntu6) ...
Setting up make (4.3-4.1build1) ...
Setting up libquadmath0:amd64 (12.3.0-1ubuntu1~22.04) ...
Setting up python3-pip (22.0.2+dfsg-1ubuntu0.5) ...
Setting up libdpkg-perl (1.21.1ubuntu2.3) ...
Setting up libubsan1:amd64 (12.3.0-1ubuntu1~22.04) ...
Setting up libnsl-dev:amd64 (1.3.0-2build2) ...
Setting up libcrypt-dev:amd64 (1:4.4.27-1) ...
Setting up libjs-jquery (3.6.0+dfsg+~3.5.13-1) ...
Setting up libbinutils:amd64 (2.38-4ubuntu2.6) ...
Setting up libc-dev-bin (2.35-0ubuntu3.8) ...
Setting up libalgorithm-diff-xs-perl (0.04-6build3) ...
Setting up libcc1-0:amd64 (12.3.0-1ubuntu1~22.04) ...
Setting up liblsan0:amd64 (12.3.0-1ubuntu1~22.04) ...
Setting up libitm1:amd64 (12.3.0-1ubuntu1~22.04) ...
Setting up libc-devtools (2.35-0ubuntu3.8) ...
Setting up libjs-underscore (1.13.2~dfsg-2) ...
Setting up libalgorithm-merge-perl (0.08-3) ...
Setting up libtsan0:amd64 (11.4.0-1ubuntu1~22.04) ...
Setting up libctf0:amd64 (2.38-4ubuntu2.6) ...
Setting up libjs-sphinxdoc (4.3.2-1) ...
Setting up libgcc-11-dev:amd64 (11.4.0-1ubuntu1~22.04) ...
Setting up libc6-dev:amd64 (2.35-0ubuntu3.8) ...
Setting up binutils-x86-64-linux-gnu (2.38-4ubuntu2.6) ...
Setting up binutils (2.38-4ubuntu2.6) ...
Setting up dpkg-dev (1.21.1ubuntu2.3) ...
Setting up libexpat1-dev:amd64 (2.4.7-1ubuntu0.4) ...
Setting up libstdc++-11-dev:amd64 (11.4.0-1ubuntu1~22.04) ...
Setting up zlib1g-dev:amd64 (1:1.2.11.dfsg-2ubuntu9.2) ...
Setting up gcc-11 (11.4.0-1ubuntu1~22.04) ...
Setting up g++-11 (11.4.0-1ubuntu1~22.04) ...
Setting up gcc (4:11.2.0-1ubuntu1) ...
Setting up libpython3.10-dev:amd64 (3.10.12-1~22.04.7) ...
Setting up python3.10-dev (3.10.12-1~22.04.7) ...
Setting up g++ (4:11.2.0-1ubuntu1) ...
update-alternatives: using /usr/bin/g++ to provide /usr/bin/c++ (c++) in auto mo
de
Setting up build-essential (12.9ubuntu3) ...
Setting up libpython3-dev:amd64 (3.10.6-1~22.04.1) ...
Setting up python3-dev (3.10.6-1~22.04.1) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.8) ...
```
- Upgrades the paramiko library, which is essential for Ansible to handle SSH connections for managing remote systems.
```
pip install --upgrade paramiko
```
**Output**
```
rhuser@ankit-Standard-PC-Q35-ICH9-2009:~$ pip install --upgrade param
Defaulting to user installation because normal site-packages is not writeable
Collecting param
  Downloading param-2.1.1-py3-none-any.whl (116 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 116.8/116.8 KB 1.1 MB/s eta 0:00:00
Installing collected packages: param
Successfully installed param-2.1.1

```
- Upgrades the cryptography library, which is required for secure communication, encryption, and decryption tasks in Ansible, especially for handling SSH keys and SSL/TLS connections.
```
pip install --upgrade cryptography
```
**Output**
```
rh-user@amkit-Standard-PC-Q35-ICH9-2009:~$ pip install --upgrade cryptography
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: cryptography in /usr/lib/python3/dist-packages (3.4.8)
Collecting cryptography
  Downloading cryptography-43.0.3-cp39-abi3-manylinux_2_28_x86_64.whl (4.0 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.0/4.0 MB 8.4 MB/s eta 0:00:00
Collecting cffi>=1.12
  Downloading cffi-1.17.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (446 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 446.2/446.2 KB 3.6 MB/s eta 0:00:00
Collecting pycparser
  Downloading pycparser-2.22-py3-none-any.whl (117 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 117.6/117.6 KB 952.7 kB/s eta 0:00:00
Installing collected packages: pycparser, cffi, cryptography
Successfully installed cffi-1.17.1 cryptography-43.0.3 pycparser-2.22
```

- Opens the sudoers file
```
sudo visudo
```

- Add configuration in the end[Change your server name]

```
rhuser ALL=(ALL) NOPASSWD: ALL
```

- Create a folder for playbooks
```
sudo mkdir playbook
```
- change directory to playbook
```
cd playbook
```
Create a folder aman inside the Downloads
```
sudo mkdir -p Downloads/aman
```
- Create a custom inventory file: 
```
[webservers]

rh-user@192.168.122.37

 [webservers1]

rh-user@192.168.1.242
```

- Create a file and write the playbook here
```
sudo vim playbook1.yml
```
- Change all the Path according to your server name
```
---
#Play 1: Tasks to handle localhost (extracting and creating .jar file)
- name: Zip file & copy it (Localhost)
  hosts: localhost
  become: yes
  vars:
    extract_src: "/home/rhuser/Downloads/file.zip"
    extract_dest: "/home/rhuser/Downloads/aman/"
    extract_jar: "/home/rhuser/Downloads/aman/rhbk.jar"
  tasks:
    - name: Ensure Downloads directory exists
      file:
        path: /home/ankit/Downloads
        state: directory
        mode: '0755'

    - name: Extract the zip file
      unarchive:
        src: "{{extract_src}}"
        dest: "{{extract_dest}}"
        remote_src: yes

    - name: Change extracted files into a jar file
      command: jar cvf "{{extract_jar}}" -C /home/rhuser/Downloads/aman/ .

- name: Manage JAR file on First server
  hosts: webservers
  become: yes
  vars:
    backup_dir: "/home/rh-user/Downloads/backup"
    backup_existing_dir: "/home/rh-user/Downloads/backup/rhbk.jar.{{ ansible_date_time.iso8601 }}"
    existing_jar_file: "/home/rh-user/Downloads/rhbk.jar"
    new_jar_file: "/home/rhuser/Downloads/aman/rhbk.jar"
    stc_code_dir: "/home/rh-user/stc-code"

  tasks:
    - name: Ensure rh-user is logged in
      user:
        name: rh-user
        state: present

    - name: Ensure the stc-code directory exists
      file:
        path: "{{stc_code_dir}}"
        state: directory
        mode: '0755'

  tasks:
    - name: Check if the existing jar file exists on first server
      stat:
        path: "{{existing_jar_file}}"
      register: rhbk_file

    - name: Backup the existing jar file on first server
      copy:
        src: "{{existing_jar_file}}"
        dest: "{{backup_existing_dir}}"
        remote_src: yes
      when: rhbk_file.stat.exists

    - name: Copy jar file from Ansible server to first server
      copy:
        src: /home/rhuser/Downloads/aman/rhbk.jar
        dest: "{{backup_dir}}"

    - name: Build file in first RHBK
      command: /home/rh-user/Downloads/kc.sh
      notify:  # Notify added here
        - Restart Nginx  # Trigger the handler to restart Nginx

    - name: Check nginx service status using curl
      command: curl http://localhost
      register: nginx_status
      ignore_errors: yes
    - name: Display nginx service status
      debug:
        msg: "Nginx service status: {{ nginx_status.stdout }}"

  handlers:
    - name: Restart Nginx  
      service:
        name: nginx
        state: restarted

- name: Manage JAR file on Second server
  hosts: webservers1
  become: yes
  vars:
    backup_dir: "/home/rh-user/Downloads/backup"
    backup_existing_dir: "/home/rh-user/Downloads/backup/rhbk.jar.{{ ansible_date_time.iso8601 }}"
    existing_jar_file: "/home/rh-user/Downloads/rhbk.jar"
    new_jar_file: "/home/rhuser/Downloads/aman/rhbk.jar"
    stc_code_dir: "/home/rh-user/stc-code"

  tasks:
    - name: Ensure rh-user is logged in
      user:
        name: rh-user
        state: present

    - name: Ensure the stc-code directory exists
      file:
        path: "{{stc_code_dir}}"
        state: directory
        mode: '0755'

  tasks:
    - name: Check if the existing jar file exists on second server
      stat:
        path: "{{existing_jar_file}}"
      register: rhbk_file

    - name: Backup the existing jar file on second server
      copy:
        src: "{{existing_jar_file}}"
        dest: "{{backup_existing_dir}}"
        remote_src: yes
      when: rhbk_file.stat.exists

    - name: Copy jar file from Ansible server to second server
      copy:
        src: /home/rhuser/Downloads/aman/rhbk.jar
        dest: "{{backup_dir}}"

    - name: Build file in second RHBK
      command: /home/rh-user/Downloads/kc.sh
      notify:  # Notify added here
        - Restart Nginx  # Trigger the handler to restart Nginx

    - name: Check nginx service status using curl
      command: curl http://localhost
      register: nginx_status
      ignore_errors: yes
    - name: Display nginx service status
      debug:
        msg: "Nginx service status: {{ nginx_status.stdout }}"

  handlers:
    - name: Restart Nginx  
      service:
        name: nginx
        state: restarted
```
 ### **first-rhbk**

\*OS details:\*NAME="Ubuntu"\
VERSION="20.04.6 LTS (Focal Fossa)"\
ID=ubuntu\
ID\_LIKE=debian\
PRETTY\_NAME="Ubuntu 20.04.6 LTS"\
VERSION\_ID="20.04"\
HOME\_URL="https\://www\.ubuntu.com/"\
SUPPORT\_URL="https\://help.ubuntu.com/"\
BUG\_REPORT\_URL="https\://bugs.launchpad.net/ubuntu/"\
PRIVACY\_POLICY\_URL="https\://www\.ubuntu.com/legal/terms-and-policies/privacy-policy"\
VERSION\_CODENAME=focal\
UBUNTU\_CODENAME=focal

- Download rhbk-24.0.3.zip

- Change the Directory
```
 cd  Download

```
- Unzip the file

```
 unzip rhbk-24.0.3.zip

```

- Searches the package cache for available OpenJDK packages on your system, showing a list of OpenJDK versions and related packages that can be installed.
```
sudo apt-cache search openjdk
```
**Output**
```
rh-user@amkit-Standard-PC-Q35-ICH9-2009:~$ sudo apt-cache search openjdk
default-jdk - Standard Java or Java compatible Development Kit
default-jdk-doc - Standard Java or Java compatible Development Kit (documentation)
default-jdk-headless - Standard Java or Java compatible Development Kit (headless)
default-jre - Standard Java or Java compatible Runtime
default-jre-headless - Standard Java or Java compatible Runtime (headless)
openjdk-11-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-11-doc - OpenJDK Development Kit (JDK) documentation
openjdk-11-jdk - OpenJDK Development Kit (JDK)
openjdk-11-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-11-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-11-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-11-source - OpenJDK Development Kit (JDK) source files
crypto-policies - unify the crypto policies used by different applications and libraries
jtreg - Regression Test Harness for the OpenJDK platform
jtreg6 - Regression Test Harness for the OpenJDK platform
libeclipse-collections-java - Eclipse Collections - comprehensive collections library for Java
libhsdis0-fcml - HotSpot disassembler plugin using FCML
libjax-maven-plugin - Using the xjc goal with OpenJDK 11+
libreoffice - office productivity suite (metapackage)
openjdk-11-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-11-jre-dcevm - Alternative VM for OpenJDK 11 with enhanced class redefinition
openjdk-11-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-17-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-17-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-17-doc - OpenJDK Development Kit (JDK) documentation
openjdk-17-jdk - OpenJDK Development Kit (JDK)
openjdk-17-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-17-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-17-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-17-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-17-source - OpenJDK Development Kit (JDK) source files
openjdk-18-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-18-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-18-doc - OpenJDK Development Kit (JDK) documentation
openjdk-18-jdk - OpenJDK Development Kit (JDK)
openjdk-18-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-18-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-18-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-18-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-18-source - OpenJDK Development Kit (JDK) source files
openjdk-8-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-8-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-8-doc - OpenJDK Development Kit (JDK) documentation
openjdk-8-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-8-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-8-source - OpenJDK Development Kit (JDK) source files
uwsgi-app-integration-plugins - plugins for integration of uWSGI and application
uwsgi-plugin-jvm-openjdk-11 - Java plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-jwsgi-openjdk-11 - JWSGI plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-ring-openjdk-11 - Closure/Ring plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-servlet-openjdk-11 - JWSGI plugin for uWSGI (OpenJDK 11)
java-package - Utility for creating Java Debian packages
jtreg7 - Regression Test Harness for the OpenJDK platform
libasmtools-java - OpenJDK AsmTools
openjdk-19-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-19-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-19-doc - OpenJDK Development Kit (JDK) documentation
openjdk-19-jdk - OpenJDK Development Kit (JDK)
openjdk-19-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-19-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-19-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-19-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-19-source - OpenJDK Development Kit (JDK) source files
openjdk-21-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-21-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-21-doc - OpenJDK Development Kit (JDK) documentation
openjdk-21-jdk - OpenJDK Development Kit (JDK)
openjdk-21-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-21-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-21-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-21-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-21-source - OpenJDK Development Kit (JDK) source files
openjdk-21-testsupport - Java runtime based on OpenJDK (regression test support)
openjdk-8-jdk - OpenJDK Development Kit (JDK)
openjdk-8-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-8-jre-zero - Alternative JVM for OpenJDK, using Zero
```

- Install java 17 version
```
sudo apt install openjdk-17-jdk -y
```
**Output**
```
rh-user@amkit-Standard-PC-Q35-ICH9-2009:~$ sudo apt install openjdk-17-jdk -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
  openjdk-17-demo openjdk-17-source visualvm
The following NEW packages will be installed:
  openjdk-17-jdk
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 2,366 kB of archives.
After this operation, 2,457 kB of additional disk space will be used.
Get:1 http://in.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 openjdk-17-jdk amd64 17.0.13+11-2ubuntu1~22.04 [2,366 kB]
Fetched 2,366 kB in 3s (772 kB/s)         
Selecting previously unselected package openjdk-17-jdk:amd64.
(Reading database ... 277936 files and directories currently installed.)
Preparing to unpack .../openjdk-17-jdk_17.0.13+11-2ubuntu1~22.04_amd64.deb ...
Unpacking openjdk-17-jdk:amd64 (17.0.13+11-2ubuntu1~22.04) ...
Setting up openjdk-17-jdk:amd64 (17.0.13+11-2ubuntu1~22.04) ...
update-alternatives: using /usr/lib/jvm/java-17-openjdk-amd64/bin/jconsole to provide /usr/bin/jconsole (jconsole) in auto mode
```

- Check the java version
```
java -version
```
```
rh-user@amkit-Standard-PC-Q35-ICH9-2009:~$ java -version
openjdk version "17.0.13" 2024-10-15
OpenJDK Runtime Environment (build 17.0.13+11-Ubuntu-2ubuntu122.04)
OpenJDK 64-Bit Server VM (build 17.0.13+11-Ubuntu-2ubuntu122.04, mixed mode, sharing)
```

- Install the Openssh-server
```
sudo apt install openssh-server
```
**Output**
```
s@s:~$ sudo apt install openssh-server -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  openssh-server
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 435 kB of archives.
After this operation, 1,537 kB of additional disk space will be used.
Get:1 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 openssh-server amd64 1:8.9p1-3ubuntu0.10 [435 kB]
Fetched 435 kB in 2s (224 kB/s)         
Preconfiguring packages ...
Selecting previously unselected package openssh-server.
(Reading database ... 277930 files and directories currently installed.)
Preparing to unpack .../openssh-server_1%3a8.9p1-3ubuntu0.10_amd64.deb ...
Unpacking openssh-server (1:8.9p1-3ubuntu0.10) ...
Setting up openssh-server (1:8.9p1-3ubuntu0.10) ...
rescue-ssh.target is a disabled or a static unit not running, not starting it.
ssh.socket is a disabled or a static unit not running, not starting it.
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for ufw (0.36.1-4ubuntu0.1) ...
Rules updated for profile 'Nginx Full'
Firewall reloaded
```

- Restart the ssh
```
sudo systemctl restart ssh

```
- Check the status of SSH
```
sudo systemctl status ssh 
```
**Output**
```
s@s:~$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2024-11-27 15:41:09 IST; 1min 29s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 114958 (sshd)
      Tasks: 1 (limit: 38289)
     Memory: 1.7M
        CPU: 21ms
     CGroup: /system.slice/ssh.service
             └─114958 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Nov 27 15:41:09 s systemd[1]: Stopping OpenBSD Secure Shell server...
Nov 27 15:41:09 s systemd[1]: ssh.service: Deactivated successfully.
Nov 27 15:41:09 s sshd[114958]: Server listening on 0.0.0.0 port 22.
Nov 27 15:41:09 s systemd[1]: Stopped OpenBSD Secure Shell server.
Nov 27 15:41:09 s sshd[114958]: Server listening on :: port 22.
Nov 27 15:41:09 s systemd[1]: Starting OpenBSD Secure Shell server...
Nov 27 15:41:09 s systemd[1]: Started OpenBSD Secure Shell server.
```
- Make a foledr backup inside the aman
```
sudo mkdir -p Downloads/aman/backup
```

- Opens the sudoers file
```
sudo visudo
```

- Add configuration in the end[Change your server name]

```
rh-user ALL=(ALL) NOPASSWD: ALL
```
### **second\_rhbk**

\*OS details:\*

NAME="Ubuntu"\
VERSION="20.04.6 LTS (Focal Fossa)"\
ID=ubuntu\
ID\_LIKE=debian\
PRETTY\_NAME="Ubuntu 20.04.6 LTS"\
VERSION\_ID="20.04"\
HOME\_URL="https\://www\.ubuntu.com/"\
SUPPORT\_URL="https\://help.ubuntu.com/"\
BUG\_REPORT\_URL="https\://bugs.launchpad.net/ubuntu/"\
PRIVACY\_POLICY\_URL="https\://www\.ubuntu.com/legal/terms-and-policies/privacy-policy"\
VERSION\_CODENAME=focal\
UBUNTU\_CODENAME=focal

- Download rhbk-24.0.3.zip

- Change the Directory
```
 cd  Download

```
- Unzip the file

```
 unzip rhbk-24.0.3.zip
```
- Searches the package cache for available OpenJDK packages on your system, showing a list of OpenJDK versions and related packages that can be installed.
```
sudo apt-cache search openjdk
```
**Output**
```
rh-user@s:~$ sudo apt-cache search openjdk
default-jdk - Standard Java or Java compatible Development Kit
default-jdk-doc - Standard Java or Java compatible Development Kit (documentation)
default-jdk-headless - Standard Java or Java compatible Development Kit (headless)
default-jre - Standard Java or Java compatible Runtime
default-jre-headless - Standard Java or Java compatible Runtime (headless)
openjdk-11-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-11-doc - OpenJDK Development Kit (JDK) documentation
openjdk-11-jdk - OpenJDK Development Kit (JDK)
openjdk-11-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-11-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-11-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-11-source - OpenJDK Development Kit (JDK) source files
crypto-policies - unify the crypto policies used by different applications and libraries
jtreg - Regression Test Harness for the OpenJDK platform
jtreg6 - Regression Test Harness for the OpenJDK platform
libeclipse-collections-java - Eclipse Collections - comprehensive collections library for Java
libhsdis0-fcml - HotSpot disassembler plugin using FCML
libjax-maven-plugin - Using the xjc goal with OpenJDK 11+
libreoffice - office productivity suite (metapackage)
openjdk-11-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-11-jre-dcevm - Alternative VM for OpenJDK 11 with enhanced class redefinition
openjdk-11-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-17-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-17-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-17-doc - OpenJDK Development Kit (JDK) documentation
openjdk-17-jdk - OpenJDK Development Kit (JDK)
openjdk-17-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-17-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-17-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-17-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-17-source - OpenJDK Development Kit (JDK) source files
openjdk-18-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-18-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-18-doc - OpenJDK Development Kit (JDK) documentation
openjdk-18-jdk - OpenJDK Development Kit (JDK)
openjdk-18-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-18-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-18-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-18-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-18-source - OpenJDK Development Kit (JDK) source files
openjdk-8-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-8-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-8-doc - OpenJDK Development Kit (JDK) documentation
openjdk-8-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-8-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-8-source - OpenJDK Development Kit (JDK) source files
uwsgi-app-integration-plugins - plugins for integration of uWSGI and application
uwsgi-plugin-jvm-openjdk-11 - Java plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-jwsgi-openjdk-11 - JWSGI plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-ring-openjdk-11 - Closure/Ring plugin for uWSGI (OpenJDK 11)
uwsgi-plugin-servlet-openjdk-11 - JWSGI plugin for uWSGI (OpenJDK 11)
java-package - Utility for creating Java Debian packages
jtreg7 - Regression Test Harness for the OpenJDK platform
libasmtools-java - OpenJDK AsmTools
openjdk-19-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-19-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-19-doc - OpenJDK Development Kit (JDK) documentation
openjdk-19-jdk - OpenJDK Development Kit (JDK)
openjdk-19-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-19-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-19-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-19-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-19-source - OpenJDK Development Kit (JDK) source files
openjdk-21-dbg - Java runtime based on OpenJDK (debugging symbols)
openjdk-21-demo - Java runtime based on OpenJDK (demos and examples)
openjdk-21-doc - OpenJDK Development Kit (JDK) documentation
openjdk-21-jdk - OpenJDK Development Kit (JDK)
openjdk-21-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-21-jre - OpenJDK Java runtime, using Hotspot JIT
openjdk-21-jre-headless - OpenJDK Java runtime, using Hotspot JIT (headless)
openjdk-21-jre-zero - Alternative JVM for OpenJDK, using Zero
openjdk-21-source - OpenJDK Development Kit (JDK) source files
openjdk-21-testsupport - Java runtime based on OpenJDK (regression test support)
openjdk-8-jdk - OpenJDK Development Kit (JDK)
openjdk-8-jdk-headless - OpenJDK Development Kit (JDK) (headless)
openjdk-8-jre-zero - Alternative JVM for OpenJDK, using Zero
```

- Install java 17 version
```
sudo apt install openjdk-17-jdk -y
```
**Output**
```
rh-user@s:~$ sudo apt install openjdk-17-jdk -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
  openjdk-17-demo openjdk-17-source visualvm
The following NEW packages will be installed:
  openjdk-17-jdk
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 2,366 kB of archives.
After this operation, 2,457 kB of additional disk space will be used.
Get:1 http://in.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 openjdk-17-jdk amd64 17.0.13+11-2ubuntu1~22.04 [2,366 kB]
Fetched 2,366 kB in 3s (772 kB/s)         
Selecting previously unselected package openjdk-17-jdk:amd64.
(Reading database ... 277936 files and directories currently installed.)
Preparing to unpack .../openjdk-17-jdk_17.0.13+11-2ubuntu1~22.04_amd64.deb ...
Unpacking openjdk-17-jdk:amd64 (17.0.13+11-2ubuntu1~22.04) ...
Setting up openjdk-17-jdk:amd64 (17.0.13+11-2ubuntu1~22.04) ...
update-alternatives: using /usr/lib/jvm/java-17-openjdk-amd64/bin/jconsole to provide /usr/bin/jconsole (jconsole) in auto mode
```

- Check the java version
```
java -version
```
```
rh-user@s:~$ java -version
openjdk version "17.0.13" 2024-10-15
OpenJDK Runtime Environment (build 17.0.13+11-Ubuntu-2ubuntu122.04)
OpenJDK 64-Bit Server VM (build 17.0.13+11-Ubuntu-2ubuntu122.04, mixed mode, sharing)
```

- Install the Openssh-server
```
sudo apt install openssh-server
```
**Output**
```
rh-user@s:~$ sudo apt install openssh-server -y
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Suggested packages:
  molly-guard monkeysphere ssh-askpass
The following NEW packages will be installed:
  openssh-server
0 upgraded, 1 newly installed, 0 to remove and 7 not upgraded.
Need to get 435 kB of archives.
After this operation, 1,537 kB of additional disk space will be used.
Get:1 http://in.archive.ubuntu.com/ubuntu jammy-updates/main amd64 openssh-server amd64 1:8.9p1-3ubuntu0.10 [435 kB]
Fetched 435 kB in 2s (224 kB/s)         
Preconfiguring packages ...
Selecting previously unselected package openssh-server.
(Reading database ... 277930 files and directories currently installed.)
Preparing to unpack .../openssh-server_1%3a8.9p1-3ubuntu0.10_amd64.deb ...
Unpacking openssh-server (1:8.9p1-3ubuntu0.10) ...
Setting up openssh-server (1:8.9p1-3ubuntu0.10) ...
rescue-ssh.target is a disabled or a static unit not running, not starting it.
ssh.socket is a disabled or a static unit not running, not starting it.
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for ufw (0.36.1-4ubuntu0.1) ...
Rules updated for profile 'Nginx Full'
Firewall reloaded
```

- Restart the ssh
```
sudo systemctl restart ssh

```
- Check the status of SSH
```
sudo systemctl status ssh 
```
**Output**
```
rh-user@s:~$ sudo systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2024-11-27 15:41:09 IST; 1min 29s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 114958 (sshd)
      Tasks: 1 (limit: 38289)
     Memory: 1.7M
        CPU: 21ms
     CGroup: /system.slice/ssh.service
             └─114958 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Nov 27 15:41:09 s systemd[1]: Stopping OpenBSD Secure Shell server...
Nov 27 15:41:09 s systemd[1]: ssh.service: Deactivated successfully.
Nov 27 15:41:09 s sshd[114958]: Server listening on 0.0.0.0 port 22.
Nov 27 15:41:09 s systemd[1]: Stopped OpenBSD Secure Shell server.
Nov 27 15:41:09 s sshd[114958]: Server listening on :: port 22.
Nov 27 15:41:09 s systemd[1]: Starting OpenBSD Secure Shell server...
Nov 27 15:41:09 s systemd[1]: Started OpenBSD Secure Shell server.
```
- Make a foledr backup inside the aman
```
sudo mkdir -p Downloads/aman/backup
```

- Opens the sudoers file
```
sudo visudo
```

- Add configuration in the end[Change your server name]

```
rh-user ALL=(ALL) NOPASSWD: ALL
```


                                                                                   
