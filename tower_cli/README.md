## Projects

GitHub
```bash
tower-cli project create \
        -n tower-demo-pj \
        --organization Default \
        --scm-type git \
        --scm-url https://github.com/irixjp/ansible-tower-demo.git \
        --scm-update-on-launch true \
        --monitor
```

Update manually
```bash
tower-cli project update -n tower-demo-pj --organization Default --monitor
```


## Credentials

Machine
```bash
tower-cli credential create \
        -n tower-demo-cred-ssh \
        --organization Default \
        --credential-type Machine \
        --inputs='{"username": "centos", "password": "redhat", "become_method": "sudo"}'
```

OpenStack
```bash
tower-cli credential create \
        -n tower-demo-cred-rhosp \
        --organization Default \
        --credential-type OpenStack \
        --inputs='{"project": "pj-name", "username": "irixjp", "password": "redhat", "host": "http://xxx.xxx.xxx.xxx:5000/v3", "domain": "default"}'
```


## Inventories

localhost
```bash
tower-cli inventory create -n localhost --organization Default

tower-cli host create \
        -n localhost \
        -i localhost \
        --variables 'ansible_connection: local'
```

typical nodes
```bash
tower-cli inventory create -n tower-demo-inv-clients --organization Default

tower-cli host create \
        -n node-1 \
        -i tower-demo-inv-clients \
        --variables 'ansible_host: 172.20.0.2'

tower-cli host create \
        -n node-2 \
        -i tower-demo-inv-clients \
        --variables 'ansible_host: 172.20.0.3'
```

OpenStack Dynamic Inventory
```bash
tower-cli inventory create -n tower-demo-inv-openstack --organization Default

tower-cli inventory_source create \
        -n openstack-source \
        -i tower-demo-inv-openstack \
        --source openstack \
        --credential tower-demo-cred-rhosp \
        --overwrite true \
        --overwrite-vars true \
        --update-on-launch true

tower-cli inventory_source update --monitor openstack-source
```

Git Inventory
```bash

```

## Ad-hoc Commands

ping
```
tower-cli ad_hoc launch \
        --job-type run \
        -i tower-demo-inv-openstack \
        --credential tower-demo-ssh-cred \
        --module-name ping \
        --monitor
```

shell
```
tower-cli ad_hoc launch \
        --job-type run \
        -i tower-demo-inv-openstack \
        --credential tower-demo-ssh-cred \
        --module-name shell \
        --module-args "hostname" \
        --monitor
```


## Job Templates


Normal
```
tower-cli job_template create \
        -n tower-demo-job1 \
        --job-type run \
        -i tower-demo-inv-openstack \
        --project tower-demo-pj \
        --playbook utils/hostname.yml \
        --credential tower-demo-cred-ssh \
        --become-enabled true

tower-cli job launch -J tower-demo-job1 --monitor
```

Survey
```
tower-cli job_template create \
        -n tower-demo-job2 \
        --job-type run \
        -i tower-demo-inv-openstack \
        --project tower-demo-pj \
        --playbook utils/hostname.yml \
        --credential tower-demo-cred-ssh \
        --become-enabled true \
        --survey-enabled true \
        --survey-spec=@survey.json

tower-cli job launch -J tower-demo-job2 --monitor -e @survey_answer.txt
```


