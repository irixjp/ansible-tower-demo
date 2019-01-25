curl -O https://releases.ansible.com/ansible-tower/setup-bundle/ansible-tower-setup-bundle-latest.el7.tar.gz
tar zxvf tar zxvf ansible-tower-setup-bundle-latest.el7.tar.gz
cd ansible-tower-setup-bundle-*



---
[tower]
192.168.199.8
192.168.199.22
192.168.199.7

[database]
192.168.199.10

[all:vars]
admin_password='redhat'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='redhat'

rabbitmq_username=tower
rabbitmq_password='redhat'
rabbitmq_cookie=cookiemonster

# Isolated Tower nodes automatically generate an RSA key for authentication;
# To disable this behavior, set this value to false
# isolated_key_generation=true

ansible_user=centos
ansible_ssh_password=redhat
