SSH to the guest machine and change directory:
- `vagrant ssh`
- `cd /vagrant/docker`

Run this command to build the image locally:
- `docker image build --no-cache --tag shazchaudhry/nginx:latest --build-arg GIT_COMMIT=$(git log -1 --format=%H) .`

Once the image has been build using the above command then run this command below to find the git commit that this image comes from:
- `docker image inspect shazchaudhry/nginx:latest | jq '.[].ContainerConfig.Labels'`

Run the image
- `docker container run -d --rm --name nginx -p 8080:80 shazchaudhry/nginx:latest`
