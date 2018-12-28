FROM nginx:alpine

ARG GIT_COMMIT=unspecified
LABEL git_commit=$GIT_COMMIT

# Run this command to build the image locally:
## docker image build --no-cache --tag shazchaudhry/docker-jenkins:latest --build-arg GIT_COMMIT=$(git log -1 --format=%H) .
# Once the image has been build using the above command then run this command below to find git commit:
## docker inspect shazchaudhry/docker-jenkins:latest | jq '.[].ContainerConfig.Labels'

LABEL Author="Shahzad Chaudhry"
LABEL LinkedIn="https://www.linkedin.com/in/shahzad-chaudhry/"

COPY static-content /usr/share/nginx/html
