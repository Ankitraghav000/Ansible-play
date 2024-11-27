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
        
