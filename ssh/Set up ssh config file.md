# Setting up SSH Config file for Easy ssh Login.
Create a file or update an existing file.
```sh
touch ~/.ssh/config
```

```sh
Host simple-name
    HostName 192.168.1.23 # uri or ip of host 
    port 22 # any
    User username # used for loging in
    IdentityFile ~/.ssh/id_rsa # location of private key for logging in.
```

So calling `ssh simple-name` will log you into the server.