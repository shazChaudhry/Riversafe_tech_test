# This is work in progress


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
