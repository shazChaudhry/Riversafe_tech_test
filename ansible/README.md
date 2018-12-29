This is how you execute playbooks manually from within the guest machine
1. `vagrant ssh`
1. `cd /vagrant/ansible`
1. `export ANSIBLE_CONFIG=./ansible.cfg`. Without this system variable, you may get an error / warning saying _Ansible is being run in a world writable directory_

Execute all roles defined in the main playbook:
- `ansible-playbook site.yml`

Execute install-docker tests:
- `ansible-playbook -i roles/install-docker/tests/hosts roles/install-docker/tests/test.yml`

Execute deploy-nginx tests:
- `ansible-playbook -i roles/deploy-nginx/tests/hosts roles/deploy-nginx/tests/test.yml`
