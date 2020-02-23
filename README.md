# vagrant-centos7-oc

Create a vagrant box CENTOS/7 to run OpenShift OC client and Docker

## Projects and references

* xxxx

## oc Tool

oc login 

```shell
oc login xxxx
```

## Github

Git project Clone, you can choose target folde

``` shell
git clone https://github.com/Gigilamalice/vagrant-centos7-oc.git vagrant-centos7-oc
cd vagrant-centos7-omdbtool
```

Update your origin/master (remote repo from your clone / upstream)

```shell
git add .
git commit -m "version initiale du projet"
git push
```

Refresh your head (local repo)

```shell
git pull
```



## Vagrant tool

add box

```shell
vagrant box add centos/7
```

Install vagrant plugin tool

Scp between guest and host

```shell
# 1 - Install pluginvagrant plugin
vagrant plugin install vagrant-vbguest
vagrant plugin install vagrant-scp
# 2 - Retreive ID vm
vagrant global-status
# 3a - Copy file or directory from Vagrant host machine to guest VM:
vagrant scp <some_local_file_or_dir> <vm_name>:<some_path_on_vm>
# 3b Copy file or directory from guest VM to Vagrant host machine:
vagrant scp <vm_name>:<some_file_or_dir_on_vm> <some_local_path>
```

Start Vagrant box

```shell
vagrant up
vagrant ssh
```

Stop vagrant box

```shell
vagrant halt
```

To a rsync from host to vagrant box (guest)

```shell
vagrant rsyn
vagrant rsyn-auto
```

Delete vagrant box

```shell
vagrant destroy -f
```

vagrant ssh config for putty by example

```shell
vagrant ssh-config
```
