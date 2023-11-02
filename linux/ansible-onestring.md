# Ansible In One String
Do something quickly using Ansible without writing playbook
```
ansible <host_group|all> -i <inventory_name> -m <module> -a "<module_params>" -l <limit_host(s)>
```
For example
```shell
ansile web-servers -i prod -m shell -a "ls -al /var/www" -l web-server01,web-server02
```