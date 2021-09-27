# Task: Ansible Replace Module

There is some data on all app servers in Stratos DC. The Nautilus development team shared some requirement with the DevOps team to alter some of the data as per recent changes they made. The DevOps team is working to prepare an Ansible playbook to accomplish the same. Below you can find more details about the task.

Write a playbook.yml under /home/thor/ansible on jump host, an inventory is already present under /home/thor/ansible directory on Jump host itself. Perform below given tasks using this playbook:

We have a file /opt/dba/blog.txt on app server 1. Using Ansible replace module replace string xFusionCorp to Nautilus in that file.

We have a file /opt/dba/story.txt on app server 2. Using Ansiblereplace module replace the string Nautilus to KodeKloud in that file.

We have a file /opt/dba/media.txt on app server 3. Using Ansible replace module replace string KodeKloud to xFusionCorp Industries in that file.

# Solution

Our task will have 3 parameters. Depending on the server, those parameters should be set differently. While we could do that setting environment variables in the inventory file for each server, I choose to experiment with creating ansible roles to achive the same and avoid poluting the inventory file. We are using `role parameters` to set the respective variables.

Crate the role folder structure:

    mkdir /home/thor/ansible/roles
    ansible-galaxy init /home/thor/ansible/roles/replace

In the main.yml of the tasks directory add the following task:

```yaml
- name: Replace exact match in file 
  replace:
    path: '{{ file_path }}'
    regexp: '{{ match_exact }}'
    replace: '{{ replace_with }}'
```

Create the playbook:

```yaml
- name: Modify welcome message on stapp01
  become: yes
  hosts: stapp01
  roles:
  - role: replace
    file_path: /opt/dba/blog.txt 
    match_exact: xFusionCorp
    replace_with: Nautilus

- name: Modify welcome message on stapp02
  become: yes
  hosts: stapp02
  roles:
  - role: replace
    file_path: /opt/dba/story.txt
    match_exact: Nautilus
    replace_with: KodeKloud

- name: Modify welcome message on stapp03
  become: yes
  hosts: stapp03
  roles:
  - role: replace
    file_path: /opt/dba/media.txt
    match_exact: KodeKloud
    replace_with: 'xFusionCorp Industries'
```