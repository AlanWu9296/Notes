## check running port

1. `lsof -i tcp:<target port>` to find the PID
2. `kill -9 <PID>` to kill the process

## ssh connect

1. `chmod 400 <.pem>` to change the permission of the key file to private
2. `ssh -i <pem path> <hostname>@<adress>` to connect to the remote server

