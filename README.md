# Ansible playbook for a quick start with Kubernetes on Centos7

## Get your environment ready:
```
$ vagrant up
$ vagrant ssh ingress
[vagrant@ingress ~]$ ip a
.....................
4: eth2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP qlen 1000
    link/ether 08:00:27:11:94:82 brd ff:ff:ff:ff:ff:ff
    inet 192.168.11.11/24 brd 192.168.11.255 scope global eth2
       valid_lft forever preferred_lft forever
    inet 192.168.11.12/24 brd 192.168.11.255 scope global secondary eth2
       valid_lft forever preferred_lft forever
    inet 192.168.11.13/24 brd 192.168.11.255 scope global secondary eth2
       valid_lft forever preferred_lft forever
    inet 192.168.11.14/24 brd 192.168.11.255 scope global secondary eth2
       valid_lft forever preferred_lft forever
    inet 192.168.11.15/24 brd 192.168.11.255 scope global secondary eth2
       valid_lft forever preferred_lft forever
    inet 192.168.11.16/24 brd 192.168.11.255 scope global secondary eth2
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe11:9482/64 scope link 
       valid_lft forever preferred_lft forever

Those are supposed to be your pseudo-external IP-addresses
```

## Run ansible playbook
```
$ ansible-playbook -i inventory site.yml
```

## Sometimes you need to restart flanneld in case of network failures
```
$ ansible-playbook -i inventory restart-flanneld.yml
```

## Enter the master node
```
$ vagrant ssh master
[vagrant@master ~]# cd /vagrant/examples
[vagrant@master ~]# kubectl create -f mysql.yaml
[vagrant@master ~]# kubectl create -f mysql-service.yaml
[vagrant@master ~]# kubectl get pods
... The mysql pod should be ready
```

## Try connecting to your mysql pod within your host-machine, using the "external" ip-address:
```
$ mysql -h 192.168.11.15 -u root -p
myassword
```

## You can also try a more complicated example, which is actually a slightly modified Guestbook multitier application
```
$ vagrant ssh master
[vagrant@master ~]# cd /vagrant/examples/guestbook
[vagrant@master ~]# kubectl create -f all-in-one/guestbook-all-in-one.yaml
[vagrant@master ~]# kubectl get pods
```
## Try opening http://192.168.11.14 in your browser
