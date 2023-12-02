Kasim abe
# Tutorial
## Creating new user
to get started in creating your **new user**, lets log into your droplet.

we can log into our droplet by entering the command below into your terminal:
```bash
ssh -i path/to/your/key root@ipv4.address.of.droplet
#example:
ssh -i .ssh/do-key root@24.199.98.191
```
now that we've logged into the droplet, we can get started with **creating a new user**.

we can start by adding a new user using the `useradd` command
here is the general syntax:
```
useradd [OPTIONS] USERNAME
```
for this tutorial, we'll be using the command like this:
```bash
useradd -ms /bin/bash your-username
```
in this command we're using `-m` to create a new home directory `/home/username` and we're using `-s` to set the path to our login shell as `/bin/bash` which we've specified.

The next step is to **give our user a password**.
```bash
passwd your-username
```
once inputed, you'll be prompted to enter your password twice.

So far we've:
  1. Created a new user
  2. Created a password for our user

now lets give our user sudo privleges to perform administrative tasks

to do that we need to use the `usermod` command.
here is the general syntax:
```bash
usermod [OPTIONS] your-username
```
this is what we'll be using for this tutorial:
```bash
usermod -aG sudo your-username
```
the `-a` option means append, so were appending the user to a group. `-G` is an option for a list of groups the user is apart of. if we just did `usermod -G sudo your-username` it would actually replace the user groups with the new group provided.

now that our user creation is complete, let's make sure it can access the server via SSH.

to do that let's copy the `.ssh` directory and paste it in the home directory of our user.
we can use the `cp` command to complete this task.
```bash
cp [OPTION] Source Destination
```
**NOTE:**
before doing this command make sure you're in the root directory

for this command we're going to use `-R` so it copies everything in the directory.
```bash
cp -R .ssh /home/your-username
```

since we've copied the `.ssh` directory, let's also change the ownership of the directory.
we can use `chown` to change file and directory ownerships.
syntax:
```bash
chown [OPTION]… [OWNER][:GROUP] FILE…
```
for us, we'll use it like this:
```bash
chown -R your-username: .ssh
```
we use `-R` again for the same reason as last time. also make sure you do this in the `/home/your-username` path.









