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


