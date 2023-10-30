#
# JFROG Container Registry.
# This can be used to setup a repository server that can be used for followings
# - Container registry - Storing Docker Images.
# - Generic repository - Storing Generic Data.
#

JFROG Container Registry - https://jfrog.com/container-registry/

#
# Main configuration file - configs/system.yaml
#

#
# Starting the container.
#
docker-compose up -d

#
# Checking the container logs.
#
docker-compose logs -f

#
# Once Container is successfully created, we can access the webui
# in the browser - http://artifactory.mydomain.com
# Default username and password - admin/password
#

#
# Setting up first time.
# Setup some generic and/or docker repositories.
#

#
# Working with Docker repositories.
# For this tests, we have setup a local docker repository 'dev-docker-repo'
# and created a user with minimal at-least contribute role access for this repo.
#

#
# Instead of the Password a Token can be generated for authentication.
#

# Login to the repository.
# Tag a image that we want to push to this repo.
# Then push the image.
# Remove image and then pull it back.
docker login artifactory.mydomain.com
docker tag debian:12.2 artifactory.mydomain.com/dev-docker-repo/debian/bookworm:12.2
docker push artifactory.mydomain.com/dev-docker-repo/debian/bookworm:12.2
docker rmi artifactory.mydomain.com/dev-docker-repo/debian/bookworm:12.2
docker pull artifactory.mydomain.com/dev-docker-repo/debian/bookworm:12.2

#
# Working with Generic repositories.
# For this tests, we have setup a local Generic repository 'generic-file-storage-repo'
# and created a user with minimal at-least contribute role access for this repo.

#
# Using curl tool.
# We will use a dummy file to upload and download tests on repo.
#
touch file-upload.test
curl -u <USER>:<PASSWORD> -T ./file-upload.test https://artifactory.mydomain.com/artifactory/generic-file-storage-repo/test-files/file-upload.test
rm file-upload.test
curl -u <USER>:<PASSWORD> -L -O https://artifactory.mydomain.com/artifactory/generic-file-storage-repo/test-files/file-upload.test

#
# Using JFROG CLI tool.
# https://jfrog.com/getcli/
#
jf rt upload ./file-upload.test --url=https://artifactory.mydomain.com/artifactory --user=<USER> --password=<PASSWORD> generic-file-storage-repo/test-files/file-upload.test
jf rt download --url=https://artifactory.mydomain.com/artifactory --user=<USER> --password=<PASSWORD> generic-file-storage-repo/test-files/file-upload.test

