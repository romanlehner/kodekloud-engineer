# Task: Ansible Basic Playbook	

One of the Nautilus DevOps team members was working on to test an Ansible playbook on jump host. However, he was only able to create the inventory, and due to other priorities that came in he has to work on other tasks. Please pick up this task from where he left off and complete it. Below are more details about the task:



The inventory file /home/thor/ansible/inventory seems to be having some issues, please fix them. The playbook needs to be run on App Server 1 in Stratos DC, so inventory file needs to be updated accordingly.

Create a playbook /home/thor/ansible/playbook.yml and add a task to create an empty file /tmp/file.txt on App Server 1.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure the playbook works this way without passing any extra arguments.


# Solution

Changed the inventory file to reflect stapp01:

    stapp01 ansible_host=172.16.238.10 ansible_user=tony

Created the `playbook.yml` file:

```yaml
- name: create empty file
  become: yes
  hosts: stapp01
  tasks:
  - copy:
      content: ""
      dest: /tmp/file.txt 
```

When trying to apply the file I encountered a connection error to stapp01. This was solved by adding an ssh certificate to the server. I used the standard values for all inputs:

    ssh-keygen
    ssh-copy-id tony@stapp01