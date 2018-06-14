# ansible-docker-swarm
Initialize Docker Swarm with Ansible

## Pre-Check

Hosts file: 

```
$ cat /etc/hosts
10.0.8.2 client
192.168.1.10 swarm-manager
192.168.1.11 swarm-worker-1
192.168.1.12 swarm-worker-2
```

SSH Config:

```
$ cat ~/.ssh/config 
Host client
  Hostname client
  User root
  IdentityFile /tmp/key.pem
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null

Host swarm-manager
  Hostname swarm-manager
  User root
  IdentityFile /tmp/key.pem
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null

Host swarm-worker-1
  Hostname swarm-worker-1
  User root
  IdentityFile /tmp/key.pem
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null

Host swarm-worker-2
  Hostname swarm-worker-2
  User root
  IdentityFile /tmp/key.pem
  StrictHostKeyChecking no
  UserKnownHostsFile /dev/null
```

Install Ansible:

```
$ apt install python-setuptools -y
$ easy_install pip
$ pip install ansible
```

Ensure passwordless ssh is working:

```
$ ansible -i inventory.ini -u root -m ping all
client | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
swarm-manager | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
swarm-worker-2 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
swarm-worker-1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
```

