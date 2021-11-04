# Task: Ansible file module

The Nautilus DevOps team is working to test several Ansible modules on servers in Stratos DC. Recently they wanted to test the file creation on remote hosts using Ansible. Find below more details about the task:



a. Create an inventory file ~/playbook/inventory on jump host and add all app servers in it.

b. Create a playbook ~/playbook/playbook.yml to create a blank file /tmp/nfsshare.txt on all app servers.

c. The /tmp/nfsshare.txt file permission must be 0655.

d. The user/group owner of file /tmp/nfsshare.txt must be tony on app server 1, steve on app server 2 and banner on app server 3.

Note: Validation will try to run the playbook using command ansible-playbook -i inventory playbook.yml, so please make sure the playbook works this way without passing any extra arguments.


# Solution

Create the files:

```yaml
cat <<EOF > ~/playbook/playbook.yml
- hosts: all
  become: yes
  tasks:
  - file:
      path: /tmp/nfsshare.txt
      state: touch
      owner: '{{ ansible_user }}' 
      group: '{{ ansible_user }}'
      mode: '0655'
EOF
```

```bash
cat <<EOF > ~/playbook/inventory
stapp01 ansible_host=stapp01 ansible_connection=ssh ansible_user=tony
stapp02 ansible_host=stapp02 ansible_connection=ssh ansible_user=steve
stapp03 ansible_host=stapp03 ansible_connection=ssh ansible_user=banner
EOF
```

Create a SSH key and populate it on the servers:

    ssh-keygen

    ssh-copy-id tony@stapp01
    ssh-copy-id steve@stapp02
    ssh-copy-id banner@stapp03 
    