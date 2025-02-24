we can run a playbook command on a group of hosts
ansible app -m 'ping'

we can specify the targets host directly in playbooks with the `host: app,db` directive and use the command bellow to run our playbook
ansible -i hosts.ini -m 'ping'
ansible -i hosts.ini playbook.yaml
