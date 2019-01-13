## Running tests
You will need to update relevant files under "_ansible/roles/deploy-nginx/molecule_" directory and then may execute tests. Test scripts can be found in "_ansible/roles/deploy-nginx/molecule/default/tests/test_default.yml_"

You will need to ensure that all existing container have stoped and removed: `docker container stop $(docker container ps -aq)`. Change directory to the role to be tested and then run:
- `cd /vagrant/ansible/roles/deploy-nginx`
- `molecule test`

## Running individual Molecule commands
You will need to ensure that all existing container have stoped and removed: `docker container stop $(docker container ps -aq)`
- `cd /vagrant/ansible/roles/deploy-nginx`
- `molecule lint` - Executes yaml-lint, ansible-lint, and flake8, reporting failure if there are issues
- `molecule syntax` - Verifies the role for syntax errors
- `molecule create` - Creates an instance with the configured driver
- `molecule prepare` - Configures instances with preparation playbooks
- `molecule converge` - Executes playbooks targeting hosts
- `molecule idempotence` - Executes a playbook twice and fails in case of changes in the second run (non-idempotent)
- `molecule verify` - Execute server state verification tools (testinfra or goss)
- `molecule destroy` - Destroys instances
- `molecule test` - Executes all the previous steps _(dependant on on scenario sequence in molecule.yml)_
> The `login` command can be used to connect to provisioned servers for troubleshooting purposes

## Execute Ansible playbooks manually
This is how you execute playbooks manually from within the guest machine
1. `vagrant ssh`
1. `cd /vagrant/ansible`
1. `export ANSIBLE_CONFIG=./ansible.cfg`. Without this system variable, you may get an error / warning saying _Ansible is being run in a world writable directory_

Execute all roles defined in the main playbook:
- `ansible-playbook site.yml`

Controlling which one playbook to run at a time:
- ansible-playbook site.yml --tags "TAG"
  - where TAG is either `docker` or `nginx`


Executing roles from within their test directories:
- `ansible-playbook -i roles/install-docker/tests/hosts roles/install-docker/tests/test.yml`
- `ansible-playbook -i roles/deploy-nginx/tests/hosts roles/deploy-nginx/tests/test.yml`
