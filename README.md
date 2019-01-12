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