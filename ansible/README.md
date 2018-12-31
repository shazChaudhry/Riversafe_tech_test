## Execute playbooks manually
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

## Create a role or a scenario with Molecule
Molecule helps develop roles using tests. The tool can even initialize a new roles e.g.
- `cd /vagrant/ansible/roles`
- `molecule init role --role-name httpd_webserver --verifier-name goss`
```
          [vagrant@techtest roles]$ molecule init role --help
          Usage: molecule init role [OPTIONS]

            Initialize a new role for use with Molecule.

          Options:
            --dependency-name [galaxy]      Name of dependency to initialize. (galaxy)
            -d, --driver-name [azure|delegated|docker|ec2|gce|lxc|lxd|openstack|vagrant]
                                            Name of driver to initialize. (docker)
            --lint-name [yamllint]          Name of lint to initialize. (yamllint)
            --provisioner-name [ansible]    Name of provisioner to initialize. (ansible)
            -r, --role-name TEXT            Name of the role to create.  [required]
            --verifier-name [goss|inspec|testinfra]
                                            Name of verifier to initialize. (testinfra)
            --help                          Show this message and exit.
```
Or for existing roles e.g.
- `cd /vagrant/ansible/roles/deploy-nginx`
- `molecule init scenario --role-name deploy-nginx --verifier-name goss`
```
          [vagrant@techtest roles]$ molecule init scenario --help
          Usage: molecule init scenario [OPTIONS]

            Initialize a new scenario for use with Molecule.

          Options:
            --dependency-name [galaxy]      Name of dependency to initialize. (galaxy)
            -d, --driver-name [azure|delegated|docker|ec2|gce|lxc|lxd|openstack|vagrant]
                                            Name of driver to initialize. (docker)
            --lint-name [yamllint]          Name of lint to initialize. (ansible-lint)
            --provisioner-name [ansible]    Name of provisioner to initialize. (ansible)
            -r, --role-name TEXT            Name of the role to create.  [required]
            -s, --scenario-name TEXT        Name of the scenario to create. (default)
                                            [required]
            --verifier-name [goss|inspec|testinfra]
                                            Name of verifier to initialize. (testinfra)
            --help                          Show this message and exit.
```
You will then need to update relevant files under molecule directory and then run the role inside a docker container as follows _(deploy-nginx role is assumed here)_:
```
          docker run --rm -it \
          -v $PWD:/tmp/$(basename "${PWD}"):ro \
          -v /var/run/docker.sock:/var/run/docker.sock \
          -w /tmp/$(basename "${PWD}") \
          quay.io/ansible/molecule:latest \
          sudo molecule test
```
## Running individual Molecule commands
- lint - Executes yaml-lint, ansible-lint, and flake8, reporting failure if there are issues
- syntax - Verifies the role for syntax errors
- create - Creates an instance with the configured driver
- prepare - Configures instances with preparation playbooks
- converge - Executes playbooks targeting hosts
- idempotence - Executes a playbook twice and fails in case of changes in the second run (non-idempotent)
- verify - Execute server state verification tools (testinfra or goss)
- destroy - Destroys instances
- test - Executes all the previous steps
> The `login` command can be used to connect to provisioned servers for troubleshooting purposes
