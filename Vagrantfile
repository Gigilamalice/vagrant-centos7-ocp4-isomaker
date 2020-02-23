# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# define hostname
NAME = "isomaker"

OCBIN="https://github.com/openshift/origin/releases/download/v3.7.2/openshift-origin-client-tools-v3.7.2-282e43f-linux-64bit.tar.gz"


$deployuserrootscript = <<-SCRIPT
echo ================================================================================
echo I am provisioning linux package and configuration ...
echo
echo as user: $(whoami), at current path: $(pwd), on hostname: $(hostname)
echo with args passed: $1
echo
echo ================================================================================

## fix locale warning
echo LANG=en_US.utf-8 >> /etc/environment
echo LC_ALL=en_US.utf-8 >> /etc/environment

# yum -y update
yum install -y wget
yum install -y git
yum install -y nano tree
yum install epel-release -y
yum install ansible -y
# yum -y install python-pip

## Download openshift client oc
# cd /tmp
# wget $1 -o wget.log
# tar xvzf openshift-origin-client-tool*
# cd openshift-origin-client-tools*
# cp oc  /usr/local/sbin

SCRIPT

$deployuservagrantscript = <<-SCRIPT
echo ================================================================================
echo
echo as user: $(whoami), at current path: $(pwd), on hostname: $(hostname)
echo with args passed: $1
echo  
echo ================================================================================
echo .
echo .
git clone https://github.com/chuckersjp/coreos-iso-maker.git
cd coreos-iso-maker
cat << EOF >  group_vars/all.yml
---
gateway: 192.168.99.1
netmask: 255.255.255.0
# VMWare default ens192
# KVM default ens3
# VBOX default enp0s3
interface: enp0s3
dns: 192.168.99.50
webserver_url: 192.168.1.50
webserver_port: 80

#ocp_version: 4.2
#iso_checksum: 649cdcf265bf39c0386331ec9b100da7c4800eae6c5aab4a8c28514b6efc5289

ocp_version: 4.3
iso_checksum: 302081da24277ed752fee8d69839227f4f24ec71261f9dfe2752ea8c0f20a0ed
iso_name: rhcos-{{ ocp_version }}.0-x86_64-installer.iso
rhcos_bios: rhcos-{{ ocp_version }}-x86_64-metal-bios.raw.gz
...
EOF

cat << EOF > inventory.yml
---
all:
  children:
    bootstrap_node:
      hosts:
        bootstrap.example.com:
          ipv4: 192.168.99.50

    masters:
      hosts:
        master1.example.com:
          ipv4: 192.168.99.51

        master2.example.com:
          ipv4: 192.168.99.52

        master3.example.com:
          ipv4: 192.168.99.53

    workers:
      hosts:
        worker1.example.com:
          ipv4: 192.168.99.54

        worker2.example.com:
          ipv4: 192.168.99.55

        worker3.example.com:
          ipv4: 192.168.99.56
...
EOF

# ansible-playbook playbook-single.yml -K
ansible-playbook playbook-single.yml

cd /tmp/rh* -l
ls /tmp


SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos/7"
  # config.vm.box_version = "1811.02"
  config.vm.hostname = NAME
  config.vm.define NAME

  config.vm.network "private_network", ip: "192.168.99.10"
  config.vm.synced_folder ".", "/vagrant"
  
  # Port forwarding
  config.vm.network "forwarded_port", guest: 22, host: 2220
  # Port for Local Docker Repository 
  # config.vm.network "forwarded_port", guest: 5000, host: 5000
  # Port for Webgui Portainer
  # config.vm.network "forwarded_port", guest: 9000, host: 9000
  # Port for Webgui Registry
  # config.vm.network "forwarded_port", guest: 80, host: 8080


  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
    vb.cpus = "1"
  end

  config.vm.provision "shell", privileged: true,  keep_color: true, inline: $deployuserrootscript, args:[OCBIN]
  config.vm.provision "shell", privileged: false, keep_color: true, inline: $deployuservagrantscript, args:[OCBIN]
  config.vm.provision "shell", inline: "echo 'INSTALLER: Installation complete, isomaker for OCP4 static IP ready to use!'"
  
  #config.vm.provision "shell", path: "scripts/install.sh"
  # To reload reboot guest 
  # config.vm.provision :reload

end
