# ansible-rpi-docker-homelab-alan

An ansible playbook to set up my homelab stuff on Ubuntu 24 using Docker via Ansible

## Disclaimers

Running this without knowing what it does may break your machine! You have been warned! Some notes:

- This assumes you are running the playlook locally. If you plan to run it remotely, you will need to make changes.
- This has only been tested on Ubuntu 24. Other Ubuntu versions may work (try at your own risk!). Other distros are not supported.
- This uses Watchtower to auto update running containers, which may increase instability or cause issues for some containers
- This relies on your ethernet interface being labeled eth0, and sets your static ip to 192.168.1.10, with your router at 192.168.1.1.
- Adguard is setup to only use standard port 53. If you plan to do more stuff like DNS-over-HTTPS, you will need to edit the role to pass the proper ports
- Smokeping is using a default target from the container which is solid. If you want to customie it, you will need to add to that role

## Prerequisites

Install Ansible & Git

```shell
sudo apt update
sudo apt install -y software-properties-common git
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
```

Then generate an APP Key for the [speedtest_tracker](https://speedtest-tracker.dev/) for yourself, and encrypt it using ansible vault. You will only need to do this once - and the secret is safe to commit once encrypted (but remember your encryption password):

```
rm secrets.enc
touch secrets.enc
echo "speedtest_tracker_app_key: base64:TS7jPuL2/944ZtDwxT+iX/pVoQ4CILkVvweVy+Wdxl0=" >> secrets.enc
ansible-vault encrypt secrets.enc
```

Use a good password, and store it in your personal password manager!

## Running

SSH into the server as your root user, clone the repo, then install the dependencies:

```shell
ansible-galaxy install -r requirements.yml
```

Then run the playbook:

```shell
sudo ansible-playbook -i ./inventories/localhost/hosts.yml -e @secrets.enc --ask-vault-pass main.yml
```

Note that the first time after running the docker role, you will need to close and reopen your ssh session to use docker commands without sudo.

## After Running

Set up Adguard Home if you havent by going to http://your-server:3000. Set the listen port to 3000 (so we don't eat up port 80)

## Ports Used

UIs:

- Adguard Home: 3000 (on /)
- Smokeping: 8080 (on /smokeping/smokeping.cgi)
- Omada Controller: 8088, 8043 (on /)
- Speedtest Tracker: 8081 (on /). See the [docs](https://docs.speedtest-tracker.dev/security/authentication) for default login info.

Other:

- Adguard Home: 53

## Troubleshooting

If your not sure if ipv6 is working, run `docker run --rm curlimages/curl -6 https://ifconfig.co`. If you get cannot connect to server, check the bridge network via `docker network inspect bridge`. If you see EnableIPV6 as false, try restarting docker (since the notify didn't work for me): `sudo systemctl restart docker`

Then ipv6 should be working, assuming your underlying internet provider supports it and your router is correctly configured.
