# hetzner-okd-ansible


## Playbook syntax check
```
ansible-playbook -i inventory.ini okd.yml --syntax-check
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