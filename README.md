# hetzner-okd-ansible

Create a hetzner VM with the CLI https://github.com/hetznercloud/cli

```
 hcloud server create --name <YOUR_DOMAIN> --type cx41 --image centos-7 --ssh-key <YOUR_HETZNER_SSH_KEY> --datacenter hel1-dc2
 ```
 if you have allready one and want it just rest and re-install the cluster you can user the following command.

 ```
hcloud server rebuild c3smonkey.ch --image centos-7
 ```


# install manual
## DNS
```
@ 10800 IN SOA ns1.gandi.net. hostmaster.gandi.net. 1576067605 10800 3600 604800 10800
* 420 IN CNAME apps.console
@ 1800 IN A 116.203.227.123
_acme-challenge.apps.c3smonkey.ch 1800 IN TXT "nD_hDgQiLboyZmAiVGVi5gtV5-EiHuBYCQxdLBXEW_g"
_acme-challenge.c3smonkey.ch 1800 IN TXT "aubRmt7tAt0gLwVEU9k-kqQ2geJM4jAggITCWGGkirM"
apps.console 300 IN A 116.203.227.123
console 300 IN A 116.203.227.123
```

## Install Manual
```
yum update & yum upgrade
yum install git -y
git clone https://github.com/okd-community-install/installcentos.git
cd installcentos/


export DOMAIN=c3smonkey.ch
export USERNAME=monkey
export PASSWORD=monkeyPassword
export LETSENCRYPT=true
export MAIL=marzelwidmer@gmail.com

echo $DOMAIN
echo $USERNAME
echo $PASSWORD
echo $LETSENCRYPT
echo $MAIL

./install-openshift.sh

htpasswd -b /etc/origin/master/htpasswd developer monkeyPassword

```

## DNS Backup
```
@ 10800 IN SOA ns1.gandi.net. hostmaster.gandi.net. 1576072417 10800 3600 604800 10800
* 420 IN CNAME apps.console
@ 1800 IN A 116.203.227.123
@ 1800 IN TXT "v=spf1 include:_mailcust.gandi.net ?all"
_acme-challenge 1800 IN TXT "Tdq40VlfIWctz0BGCUJqvJwd9kiW-KyVZ9XxiwbbMlg"
_acme-challenge 1800 IN TXT "eaP69focPsZdRJIkzl1ARUKDNR860dtDO1RgmGNS-hI"
_acme-challenge.apps 1800 IN TXT "NvM7WE56AUaYM3Jzy5uCbXncqH0eSeiV7EpRdax325U"
apps.console 300 IN A 116.203.227.123
console 300 IN A 116.203.227.123
```

### For Apps and Sub domains i have to remove it again.
```
_acme-challenge 1800 IN TXT "Tdq40VlfIWctz0BGCUJqvJwd9kiW-KyVZ9XxiwbbMlg"
_acme-challenge 1800 IN TXT "eaP69focPsZdRJIkzl1ARUKDNR860dtDO1RgmGNS-hI"
_acme-challenge.apps 1800 IN TXT "NvM7WE56AUaYM3Jzy5uCbXncqH0eSeiV7EpRdax325U"
```







Update your domain stuff in the _okd.yml_  file.

```
  vars:
    - okd_domain: "YOUR_DOMAIN"
    - okd_admin_username: "YOUR_ADMIN_USER"
    - okd_admin_password: "YOUR_ADMIN_USER_PASSWORD"
```    

Check _ansible.cfg_ file your private key file have the same name, otherwise update it
```
private_key_file =  ~/.ssh/id_rsa_hetzner
```
## Playbook

### Call playbook
```
ansible-playbook -i inventory.ini okd.yml
```

Wait for the summary...

```
Sunday 13 January 2019  09:45:58 +0100 (0:00:07.390)       0:27:30.351 ********
===============================================================================
common : Install OKD on Hetzner -------------------------------------- 1631.71s
~/hetzner-okd-ansible/roles/common/tasks/main.yml:2----------------------------
common : Read installation log file ------------------------------------- 7.39s
~//hetzner/hetzner-okd-ansible/roles/common/tasks/main.yml:32 -----------------
common : Install Git applications --------------------------------------- 3.89s
~/hetzner/hetzner-okd-ansible/roles/common/tasks/main.yml:2 -------------------
common : Transfer the installation script ------------------------------- 3.04s
~/hetzner/hetzner-okd-ansible/roles/common/tasks/main.yml:14 ------------------
common : Clone OKD installation repository ------------------------------ 2.72s
~/hetzner/hetzner-okd-ansible/roles/common/tasks/main.yml:10 ------------------
common : Check Git version ---------------------------------------------- 1.47s
~/hetzner/hetzner-okd-ansible/roles/common/tasks/main.yml:7 -------------------
```


# Ansible hints

## Playbook syntax check
```
ansible-playbook -i inventory.ini okd.yml --syntax-check
```
## Uptime
```
ansible hetzner-vm -a uptime
```
## Ping
```
ansible hetzner-vm -m ping
```
## Ansible Long version
### Ping
```
ansible -i inventory.ini hetzner-vm -m ping --verbose --user root
```
### Call localhost with ansible
#### Localhost Ping
```
ansible --connection local -i inventory.ini hetzner-vm -m ping --verbose --user root
```
#### Localhost Uptime
```
ansible --connection local -i inventory.ini hetzner-vm -a uptime --verbose --user root
```
#### hcloud server list
```
ansible --connection local -i inventory.ini local -a 'hcloud server list' --verbose --user root
```

# Openshift
## Create first Project
```
oc new-project dev \
         --description="Development Stage" \
         --display-name="Development"
```

## Deploy Spring Boot App with S2i
```
oc new-app codecentric/springboot-maven3-centos~https://github.com/marzelwidmer/spring-openshift.git \
                                                 --name=springboot-app \
                                                 -l app=springboot  \
                                                 -n dev
```
