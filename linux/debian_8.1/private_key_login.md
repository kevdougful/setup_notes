# Login with a private key

1. Generate a key pair
1. Add public key to `~/.ssh/authorized_keys` file (as one line)
1. `chmod 700 ~/.ssh`
1. `chmod 600 ~/.ssh/authorized_keys`
1. `chmod $USER:$USER ~/.ssh -R`
1. Change `/etc/ssh/sshd_config` so it contains `AuthorizedKeyFile %h/.ssh/authorized_keys`
1. `sudo service ssh restart`

`tail -f /var/log/auth.log` for errors.
