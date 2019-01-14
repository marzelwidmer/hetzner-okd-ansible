# hetzner-okd-ansible

Create a hetzner VM with the CLI https://github.com/hetznercloud/cli 

```
 hcloud server create --name <YOUR_DOMAIN> --type cx31 --image centos-7 --ssh-key <YOUR_HETZNER_SSH_KEY> --datacenter hel1-dc2
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


