# Howto

Edit the Dockerfile and replace the following environment variables before building:

* `USER` -- your user account
* `USER_ID` -- your user id. To find out your id: id -u <USER>
* `USER_ENCRYPTED_PASSWORD` -- your user password (encrypted). To generate it: perl -e 'print crypt('"<PASSWORD>"', "aa"),"\n"'

When starting the Docker container, make sure to mount /gpfs01/bethge:
`docker run -d -P -v /gpfs01/bethge:/gpfs01/bethge bethgelab-users`
