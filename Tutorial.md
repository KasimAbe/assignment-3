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

for the last part, let's login into your newly created user by exiting out the terminal and relogging on.
when we log on, we use the same command but with your username instead of root.

example:
```bash
ssh -i .ssh/do-key your-username@ipv4.address.of.droplet
```
now that we've logged into the droplet with the new user, let's make sure we can't log in with root.

to do that we need to edit the ssh configuration which has the path of `/etc/ssh/sshd_config`
let's edit the file using vim by typing:
```bash
#from the home directory
sudo vim /etc/ssh/sshd_config
```
after you enter your password you should be viewing the file.

press "I" on your keyboard to enter "insert mode".

now go down the file and look for the line that says `PermitRootLogin yes` and change the "yes" to "no" so it looks like `PermitRootLogin no`.

after that press the "ESC" button on your keyboard and type `:wq` to save and quit.

after that restart the ssh service by using this command:
```bash
sudo systemctl restart ssh.service
```
and now if you try logging in with your root user you will be denied access!

# Create sample website
In the first part we made a new user and gave it access to login.

now lets create a sample website using nginx.

first we want to install nginx but before that lets make sure our packages are up to date.

to do that we type:
```bash
sudo apt-get upgrade
```
after it will give us a prompt, just press `y`. 

since were done upgrading, lets install nginx.

to install nginx, type:
```bash
sudo apt install nginx
```
now that nginx is installed, lets start configuring nginx to serve a sample website.

Firstly lets go into the `www` directory by using the path `/var/www`.

lets go there by doing:
```bash
cd /var/www
```
in there we want to create another directory named "my-site" by using `mkdir`.

heres how we do it:
```bash
mkdir my-site
```
after that lets `cd my-site` and create a file called "index.html"

to create it we can do: 
```bash
vim index.html
```

now that we've made the file and we're inside it, press "I" on your keyboard and copy paste this block of code:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello, World</h1>
</body>
</html>
```

after pasting, press the "ESC" button on your keyboard and type `:wq` to save and quit.

now lets go back to our home directory by typing `cd` 
then lets go create a file in `/etc/nginx/sites-available` and paste the nginx config below:
```bash
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	
	root /var/www/my-site;
	
	index index.html;
	
	server_name _;
	
	location / {
		try_files $uri $uri/ =404;
	}
}
```

now that you've pasted it, press the "ESC" button on your keyboard and type `:wq` to save and quit.

for the last part we need to make a symbolic link to `/etc/nginx/sites-enabled`.

here is the general syntax for creating links between files: 
```bash
ln -s [OPTIONS] FILE LINK
```

to do that let's type:
```bash
ln -s /etc/nginx/sites-available /etc/nginx/sites-enabled
```
here we use `-s` which creates a symbolic link

after making the link, lets enter this command to test our nginx configurations:
```bash
sudo nginx -t
```
you should get these two messages if successful:
```bash
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
then we lets restart the nginx server so we can see our site, you can do this by doing:
```bash
sudo systemctl restart nginx
```

and to check the the sample HTML, use the `curl` command like this: 
```bash
curl ipv4.address.of.droplet
```
after you should see what you copy pasted into the `index.html` file.

thats it for the tutorial and thank you for following along!




