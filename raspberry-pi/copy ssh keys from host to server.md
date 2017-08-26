# Copy ssh keys from host to server

* If the `~/.ssh` folder exists on the pi host use the following command

```sh
cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'cat >> .ssh/authorized_keys && echo "Key copied"'
```

* If the `~/.ssh` folder does not exist, then use the following command:

```sh
cat ~/.ssh/id_rsa.pub | ssh <user>@<hostname> 'umask 0077; mkdir -p .ssh; cat >> .ssh/authorized_keys && echo "Key copied"'
```

Make modifications to the commands as necessary. eg. change the `<user>` and `<hostname>` variables to match the expected resources.