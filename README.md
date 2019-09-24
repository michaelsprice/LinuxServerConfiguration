# Linux Server Configuration Project 
The Linux Server Configuration Project will allow you to navigate to a website and then display a list of categories in a database. Clicking on a category will show you a list of items associated to that Category. Clicking on the item will show the item and the description. From there, you can edit or delete the item.

## IP address: http://34.207.150.199 

## SSH Port: 2200 

## Software installed:
- Linux Distribution: Ubunto 16.04 LTS
- Virtual Private Server: Amazon Lightsail
- Web Application: [My item catalog project](https://github.com/michaelsprice/catalog_project)
- Database server: Postgres 


## The landing page looks like this:
![Landing Page](https://github.com/michaelsprice/LinuxServerConfiguration/blob/master/LandingPage.png)



## Configurations Made
- Updated all currently installed packages.
- Changed the SSH port from 22 to 2200.
- Configured the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
- Created a new user account named grader.
- Gave grader the permission to sudo.
- Configured the local timezone to UTC.
- Installed and configured Apache.
- Installed and configure PostgreSQL.
- Installed git.
- Cloned and setup my Item Catalog project from the Github repository created earlier in this Nanodegree program.
- Created a catalog.wsgi & catalog.conf file.
- Enabled FlaskApp as the default site
- Changed the application.py file to point to the fully qualified client_secret.json file as well as set the create_engine to the fully qualified db file.
- Changed the database_setup.py & populate_database.py files to point to the fully qualified db file.



## Usage
- Navigate to http://34.207.150.199 
- You are presented with the categories in the database. From here, you can login (with a google account) in order to add, edit or delete category items.
- Click on a category to see a list of items associated to that Category. On the category page, there is a link to be able to see the JSON endpoint for that category.
- Click on the item to see the item and the description. From there, you can edit or delete the item (requires you to be logged in via google).



## Third-party resources
- Used [this page](https://devops.ionos.com/tutorials/install-and-configure-mod_wsgi-on-ubuntu-1604-1/) to help install & configure mod_wsgi.
- Used [this page](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps#do-not-allow-remote-connections) & [this page](https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e) to help install & configure PostgreSQL.
- Used [this page](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps) to help deploy a flask app to the server.



## Notes:
- To connect as the `grader` user from your local machine, download the 'grader2' file, then open terminal on your local machine and run `ssh -i grader2 grader@34.207.150.199 -p 2200` and use the password `grader`. 