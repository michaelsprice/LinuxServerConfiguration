# Linux Server Configuration Project 
The Linux Server Configuration Project will allow you to navigate to a website and then display a list of categories in a database. Clicking on a category will show you a list of items associated to that Category. Clicking on the item will show the item and the description. From there, you can edit or delete the item.

## IP address: http://52.91.30.170 

## SSH Port: 2200 

## Software installed:
- Linux Distribution: Ubunto 16.04 LTS
- Virtual Private Server: Amazon Lightsail
- Web Application: My item catalog project (link to repo in GitHub)
- Database server: Postgres 

## URL: 

*** Put Image of landing page here ***

## Configurations Made
1. Create an Instance with AWS Lightsail
- Navigate to https://lightsail.aws.amazon.com/ and sign in (or create account.
- Create an instance, select Linux/Unix, OS Only, select Ubuntu 16.04 LTS, select a tier (lowest is fine), give it a name, click Create Instance. ** TODO: BREAK THIS LINE UP**
- Once the instance is running, click on the name.

2. Connect via SSH
- Go to your AWS account page and select the instance.
- At the bottom, click 'Account Page'.
- Click Download.
- Save it as anything, click Save.
- Open terminal on your local machine, cd into the directory where it's downloaded.
- run `chmod 600 <nameoffile>` to restrict the file permission to nodoby from any group of outside
- Change the name to 'lightsail_key.rsa'
- Run `ssh -i lightsail_key.rsa ubuntu@<publicIP>` 
- Type yes if prompted

3. Upgrade & Install packages
- In terminal, run `sudo apt-get update`
- Run `sudo apt-get upgrade` 

4. Change the SSH port from 22 to 2200 
- In terminal, run `sudo nano /etc/ssh/sshd_config`
- Find the line Port 20 and change it to Port 2200
- Save & Exit the file
- Restart SSH by running `sudo service ssh restart`

5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
- `sudo ufw status` (to check the status, UFW should be inactive) 
- `sudo ufw default deny incoming` (denies all incoming requests)
- `sudo ufw default deny outgoing` (denies all outgoing requests)
- `sudo ufw allow 2200/tcp` (this will allow incoming SSH requests)
- `sudo ufw allow 80/tcp` (allows all HTTP requests)
- `sudo ufw allow 123/udp` 
- `sudo ufw deny 22` (denies port 22 requests)
- `sudo ufw enable` (enables UFW)
- `sudo ufw status` (to check the status, UFW should be active) 
- Go to the AWS page, networking tab
- Click Add Another, leave Custom, select UDP as the protocol, 123
- Custom TCP 2200

6. Create a new user account named grader.
- `sudo adduser grader`

7. Give grader the permission to sudo.
- `sudo nano /etc/sudoers.d/grader`
- Enter the following text: grader ALL=(ALL) ALL
- Save and exit the file 

8. Set SSH keys for grader user with ssh-keygen on local machine
- `ssh-keygen` then enter 'grader', hit enter
- `cat grader.pub` then copy the contents
- `su - grader` 
- `mkdir .ssh`
- `sudo nano .ssh/authorized_keys`
- Paste the contents, save and exit the file.
- `sudo service ssh restart`
- Disable root login 
- `sudo nano /etc/ssh/sshd_config` and find PermitRootLogin and change it to no, save and exit the file.

9. Configure the local timezone to UTC.
- `sudo dpkg-reconfigure tzdata`, select none of the above, UTC

10. Install and configure Apache to serve a Python mod_wsgi application.


11. Install and configure PostgreSQL:


12. Install git.


13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.


14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!


Restart apache
sudo service apache2 restart
then check the public ip address


## Usage
- Navigate to _______
- You are presented with the categories in the database. From here, you can login (with a google account) in order to add, edit or delete category items.
- Click on a category to see a list of items associated to that Category. On the category page, there is a link to be able to see the JSON endpoint for that category.
- Click on the item to see the item and the description. From there, you can edit or delete the item (requires you to be logged in via google).

## Third-party resources