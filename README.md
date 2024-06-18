# ansible-rpi-docker-homelab-alan

An ansible playbook to set up my homelab stuff on Ubuntu 24 using Docker via Ansible

## Running

SSH into the server as your root user, and then run the playbook:

```
ansible-playbook -i ./inventories/localhost/hosts.yml main.yml
```
