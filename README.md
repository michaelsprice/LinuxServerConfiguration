# Linux Server Configuration Project 
The Linux Server Configuration Project will allow you to navigate to a website and then display a list of categories in a database. Clicking on a category will show you a list of items associated to that Category. Clicking on the item will show the item and the description. From there, you can edit or delete the item.

## IP address: http://34.207.150.199 

## SSH Port: 2200 

## Software installed:
- Linux Distribution: Ubunto 16.04 LTS
- Virtual Private Server: Amazon Lightsail
- Web Application: My item catalog project (link to repo in GitHub)
- Database server: Postgres 


TODO - Put Image of landing page here

## Configurations Made
1. Create an Instance with AWS Lightsail
- Navigate to https://lightsail.aws.amazon.com/ and sign in (or create account).
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
- `sudo apt-get update`
- `sudo apt-get upgrade` 

4. Change the SSH port from 22 to 2200 
- `sudo nano /etc/ssh/sshd_config`
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
- Click Add Another, leave Custom, select UDP as the protocol, port 123
- Click Add Another, leave Custom, select TCP as the protocol, port 2200

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
- Disable root login by running `sudo nano /etc/ssh/sshd_config` and find PermitRootLogin and change it to no, save and exit the file.

9. Configure the local timezone to UTC.
- `sudo dpkg-reconfigure tzdata`, select none of the above, UTC

10. Install and configure Apache to serve a Python mod_wsgi application.
- `sudo apt-get install apache2 apache2-utils libexpat1 ssl-cert python` (installs some prerequisite Apache components in order to work with mod_wsgi)
- `curl http://localhost` (verifies it's working correctly)
- `sudo apt-get install libapache2-mod-wsgi` (installs mod_wsgi)
- `sudo /etc/init.d/apache2 restart` 
- `sudo nano /etc/apache2/conf-available/wsgi.conf`
- Add the following line, then save & exit the file: WSGIScriptAlias /test_wsgi /var/www/html/test_wsgi.py 

TODO: This is to test it, may need to remove
- `sudo nano  /var/www/html/test_wsgi.py`
- Add the following lines then save and exit the file:

def application(environ,start_response):
    status = '200 OK'
    html = '<html>\n' \
           '<body>\n' \
           '<div style="width: 100%; font-size: 40px; font-weight: bold; text-align: center;">\n' \
           'mod_wsgi Test Page\n' \
           '</div>\n' \
           '</body>\n' \
           '</html>\n'
    response_header = [('Content-type','text/html')]
    start_response(status,response_header)
    return [html]
- `sudo a2enconf wsgi`
- `sudo /etc/init.d/apache2 restart`

end TODO


11. Install and configure PostgreSQL:
- `sudo apt-get install postgresql postgresql-contrib`

TODO: Do not allow remote connections
I know i need to run this, but not sure what to put in that file
- `sudo nano /etc/postgresql/9.5/main/pg_hba.conf`

end TODO
- `sudo su - postgres`
- `psql`
- `create database catalog;`
- `create user catalog;`
- `alter user catalog with password 'catalog';`
- `grant all privileges on database catalog to catalog;`
- `\q`
- `exit`

12. Install git.
- `sudo apt-get install git`
- `git config --global user.name "Michael Price"`
- `git config --global user.email mprice0064@gmail.com`

13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
- `sudo git clone https://github.com/michaelsprice/catalog_project.git /var/www/catalog`
- `sudo chown -R grader:grader /var/www/catalog`
- ``
- ``
- ``
- ``

14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!


## Usage
- Navigate to _______
- You are presented with the categories in the database. From here, you can login (with a google account) in order to add, edit or delete category items.
- Click on a category to see a list of items associated to that Category. On the category page, there is a link to be able to see the JSON endpoint for that category.
- Click on the item to see the item and the description. From there, you can edit or delete the item (requires you to be logged in via google).

## Third-party resources
- Used [this page](https://devops.ionos.com/tutorials/install-and-configure-mod_wsgi-on-ubuntu-1604-1/) to help install & configure mod_wsgi
- Used [this page](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps#do-not-allow-remote-connections) & [this page](https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e) to help install & configure PostgreSQL

## Notes:
- To SSH in after enabling the firewall, cd into Downloads (or wherever the lightsail_key.rsa file is) and then run `ssh -i lightsail_key.rsa ubuntu@34.207.150.199 -p 2200`