# Task: Creating Soft Links Using Ansible
The Nautilus DevOps team is practicing some of the Ansible modules and creating and testing different Ansible playbooks to accomplish tasks. Recently they started testing an Ansible file module to create soft links on all app servers. Below you can find more details about it.


Write a playbook.yml under /home/thor/ansible directory on jump host, an inventory file is already present under /home/thor/ansible directory on jump host itself. Using this playbook accomplish below given tasks:

Create an empty file /opt/security/blog.txt on app server 1; its user owner and group owner should be tony. Create a symbolic link of source path /opt/security to destination /var/www/html.

Create an empty file /opt/security/story.txt on app server 2; its user owner and group owner should be steve. Create a symbolic link of source path /opt/security to destination /var/www/html.

Create an empty file /opt/security/media.txt on app server 3; its user owner and group owner should be banner. Create a symbolic link of source path /opt/security to destination /var/www/html.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml so please make sure playbook works this way without passing any extra arguments.

# Solution

Create the playbook:

```yaml
- hosts: stapp01, stapp02, stapp03
  become: yes
  tasks:
  - name: create file
    copy:
      content: ""
      dest: '/opt/security/{{ file }}'
      force: no
      group: '{{ ansible_user }}'
      owner: '{{ ansible_user }}'
      mode: 0755
  - name: Create symbolic link 
    file:
      src: /opt/security
      dest: /var/www/html
      state: link
```

Add the variable `file` to each host in the inventory file and set the value accordingly.

```conf
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony file=blog.txt
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve file=story.txt
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner file=media.txt
```