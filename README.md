# ansible-rpi-docker-homelab-alan

An ansible playbook to set up my homelab stuff on Ubuntu 24 using Docker via Ansible

## Running

SSH into the server as your root user, clone the repo, then install the dependencies:

```
ansible-galaxy install -r requirements.yml
```

Then run the playbook:

```
sudo ansible-playbook -i ./inventories/localhost/hosts.yml main.yml
```

Note that the first time after running the docker role, you will need to close and reopen your ssh session to use docker commands without sudo.

## After Running

Set up Adguard Home if you havent by going to http://your-server:3000. Set the listen port to 3000 (so we don't eat up port 80)
