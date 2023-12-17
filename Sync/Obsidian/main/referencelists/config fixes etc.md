# Permission denied (publickey)

Check value of `PasswordAuthentication` in `/etc/ssh/sshd_config` and if it's `no` change it to `yes` then restart ssh service.