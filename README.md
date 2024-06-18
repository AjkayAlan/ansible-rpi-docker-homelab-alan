# ansible-rpi-docker-homelab-alan

An ansible playbook to set up my homelab stuff on Ubuntu 24 using Docker via Ansible

## Distro Support

This has only been tested on Ubuntu 24. Other Ubuntu versions may work (try at your own risk!). Other distros are not supported.

## Before Running

Adguard is setup to only use standard port 53. If you plan to do more stuff like DNS-over-HTTPS, you will need to edit the role to pass the proper ports

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

## Ports Used

UIs:

- Adguard Home: 3000
- Smokeping: 8080

Other:

- Adguard Home: 53

## Troubleshooting

If your not sure if ipv6 is working, run `docker run --rm curlimages/curl -6 https://ifconfig.co`. If you get cannot connect to server, check the bridge network via `docker network inspect bridge`. If you see EnableIPV6 as false, try restarting docker (since the notify didn't work for me): `sudo systemctl restart docker`

Then ipv6 should be working, assuming your underlying internet provider supports it and your router is correctly configured.
