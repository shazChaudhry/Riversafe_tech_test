# Work in progress


## Tech Test
Please complete the following steps:
1. Create a hosted git repository account of your choice (if you do not already possess one) e.g. github, gitlab, bitbucket, etc.
2. Provide us with the account name
3. Create a public repository for this assessment and provide the name once complete
4. Use the repository for the configuration and code that will:
    1. Spin up a Linux Vagrant box of your choice
    2. Configure Vagrant to use Ansible as the provisioner
    3. Configure the box with an IP address available to the hosting OS
    4. Write playbook/playbooks to:
        1. Install and start docker
        2. Build a docker container based on the official Alpine Linux container (library/alpine:latest)
        3. Build the container so that nginx is installed and started
        4. Configure nginx to serve out some static “Hello World” content
        5. Start the container as a micro service
    5. At the end of the provisioning the URL to the web page should be available from the hosting OS
    6. Write appropriate documentation in the repository to explain how someone cloning it should provision the Vagrant VM and access the web URL serving out the “Hello World”
5. Commit as little or as often as you like
6. **For  extra points write some tests using the test framework of choice (serverspec, testinfra, etc) or even simple bash/python scripts to run tests of your choice to confirm/deny that the deployment has worked**

## The solution
The assumption is that this solution is being re-created on a Windows 10 pro machine as only Windows was available for testing. The test machine had 12GB RAM available.

### Prerequisites
- The user has admin privileges on the machine
- At least 3GB of free RAM is available on the machine. Otherwise, Vagrantfile will need editing to adjust available memory:
  - `v.customize ["modifyvm", :id, "--memory", <MEMORY_ALLOCATION>]`
- Latest version of [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Latest version of [Git Bash](https://git-scm.com/downloads)
- Latest version of [Vagrant](https://www.vagrantup.com/intro/getting-started/install.html)
  - Install Vagrant Host Manager plugin by running `vagrant plugin install vagrant-hostmanager` in Git Bash terminal. This will update host files on both guest and host machines
  - Install vagrant-vbguest plugin: `vagrant plugin install vagrant-vbguest`. See the comments in [Vagrantfile](Vagrantfile) regarding shared folders.

### Instructions
1. Clone this repo and change the directory: `git clone https://github.com/shazChaudhry/tech_test.git && cd tech_test`
1. `vagrant destroy --force; vagrant box update; vagrant box prune; vagrant up`. This command updates the box for the current Vagrant environment if there are updates available and then creates and configures the guest machine according to the Vagrantfile
2. Should you have a need to SSH to the box _(e.g. troubleshooting)_, then run `vagrant ssh` command

### Testing
The web URL serving out the “Hello World” should be accessible at [http://techtest](http://techtest)

### Clean up
Once finished, change to the directory in GitBash terminal to where this repo was cloned and run `vagrant destroy --force`
