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

## Deploy Docker Swarm

```
$ ansible-playbook -i inventory.ini -u root deploy-swarm.yml 
PLAY RECAP 

client                     : ok=11   changed=3    unreachable=0    failed=0   
swarm-manager              : ok=18   changed=4    unreachable=0    failed=0   
swarm-worker-1             : ok=15   changed=1    unreachable=0    failed=0   
swarm-worker-2             : ok=15   changed=1    unreachable=0    failed=0   
```

SSH to the Swarm Manager and List the Nodes:

```
$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
0ead0jshzkpyrw7livudrzq9o *   swarm-manager       Ready               Active              Leader              18.03.1-ce
iwyp6t3wcjdww0r797kwwkvvy     swarm-worker-1      Ready               Active                                  18.03.1-ce
ytcc86ixi0kuuw5mq5xxqamt1     swarm-worker-2      Ready               Active                                  18.03.1-ce
```

## Delete the Swarm:

```
$ ansible-playbook -i inventory.ini -u root delete-swarm.yml 

PLAY RECAP 
swarm-manager              : ok=2    changed=1    unreachable=0    failed=0   
swarm-worker-1             : ok=2    changed=1    unreachable=0    failed=0   
swarm-worker-2             : ok=2    changed=1    unreachable=0    failed=0   
```

Ensure the Nodes is removed from the Swarm, SSH to your Swarm Manager:

```
$ docker node ls
Error response from daemon: This node is not a swarm manager. Use "docker swarm init" or "docker swarm join" to connect this node to swarm and try again.
```

Thanks a lot to [lucj](https://github.com/lucj/swarm-rexray-ceph), make sure to checkout his repo.
