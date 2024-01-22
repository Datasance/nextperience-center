# Prepare Remote Hosts


ECN's are deployed using `potctl`. The following steps will ensure that our remote Agent hosts are ready to have `potctl` deploy ECN components onto them remotely.

## Add SSH RSA Public Key to the Remote Host

`potctl` will SSH into our remote hosts using an RSA SSH key-pair we specify.

We add the public key of the key-pair that we intend to use with `potctl` to the remote host to deploy ECN components onto. For example, we could run a command like this:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub <username>@<controller-or-agent-hostname>
```

We do this for Controller host (only when we intend to deploy Controller directly, i.e. not to Kubernetes cluster) and all the agents. Node this is the key we will later use to deploy Controller and Agent using `potctl`, please keep track of it.

## Add the Remote Host's User to Sudo Group

`potctl` will need to run certain commands as sudo. In order to do this without using the root user, we must add the user we intend to use on the remote host to the sudo group. Run this command on all the remote host:

```bash
usermod -aG sudo $USER
```

## Allow Sudoers to Sudo without Password

In order to prevent `potctl` from failing its SSH sessions, we need to ensure that the sudoers in the remote host can run commands without having to enter a password. Note that `potctl` is unable to prompt the user for passwords.

We can achieve this by editing the sudoers file by first running `su visudo` on the remote host. This will open up the sudoers file for editing. Make sure this line is present in the file `%sudo ALL=(ALL) NOPASSWD:ALL`.


