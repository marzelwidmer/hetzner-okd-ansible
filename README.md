# hetzner-okd-ansible

Create a hetzner VM 
```
 hcloud server create --name <YOUR_DOMAIN> --type cx31 --image centos-7 --ssh-key <YOUR_HETZNER_SSH_KEY> --datacenter hel1-dc2
 ```

Update your domain stuff in the _okd.yml_  file.

```
  vars: 
    - okd_domain: "<YOUR_DOMAIN>"
    - okd_admin_username: "<YOUR_ADMIN_USER>"
    - okd_admin_password: "<YOUR_ADMIN_USER_PASSWORD>"
```    

Check _ansible.cfg_ file your private key file have the same name, otherwise update it
```
private_key_file =  ~/.ssh/id_rsa_hetzner
```

## Playbook syntax check
```
ansible-playbook -i inventory.ini okd.yml --syntax-check
```

## Playbook 
```
ansible-playbook -i inventory.ini okd.yml
```


# Ansible 
## Uptime
```
ansible hetzner-vm -a uptime
```

## Ping
```
ansible hetzner-vm -m ping
```


# Ansible Long version
# Ping
```
ansible -i inventory.ini hetzner-vm -m ping --verbose --user root
```

## Call localhost with ansible 
### Localhost Ping
```
ansible --connection local -i inventory.ini hetzner-vm -m ping --verbose --user root
```
### Localhost Uptime
```
ansible --connection local -i inventory.ini hetzner-vm -a uptime --verbose --user root
```
### hcloud server list
```
ansible --connection local -i inventory.ini local -a 'hcloud server list' --verbose --user root
```

# Openshift
## Create first Project
```
oc new-project appdev \
         --description="Development Stage" \
         --display-name="Development"
```

## Deploy Spring Boot App with S2i
```
oc new-app codecentric/springboot-maven3-centos~https://github.com/marzelwidmer/spring-openshift.git \
                                                 --name=springboot-app \
                                                 -l app=springboot  \
                                                 -n appdev
```


