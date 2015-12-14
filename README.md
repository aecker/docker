# docker

Alex' Docker tools. Build order:

1. bethgelab-users
2. cuda-k40
3. caffe-k40
4. tools-k40


## bethgelab-users

Enables the following features:

1. Using LDAP user within a Docker container (more precisely: emulates it by using a local user with the same uid).
2. Runs an X server.
3. SSH daemon, i.e. allows `ssh -X` to run GUI within the Docker container.

Edit the Dockerfile and replace the following environment variables before building:

* `USER` -- your user account
* `USER_ID` -- your user id. To find out your id: `id -u USER`
* `USER_ENCRYPTED_PASSWORD` -- your user password (encrypted). To generate it: `perl -e 'print crypt('"PASSWORD"', "aa"),"\n"'`

To start the container, make sure to mount `/gpfs01/bethge` and expose the ports:
`docker run -d -P -v /gpfs01/bethge:/gpfs01/bethge bethgelab-users`

### Note

Do not override the `CMD` in bethgelab-users. If you need to execute additional programs when starting the container, add them to `/usr/local/bin/startup` as follows:

`RUN echo "./mycmd" >> /usr/local/bin/startup`

