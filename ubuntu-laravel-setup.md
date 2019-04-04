# Ubuntu Laravel Setup

Variables Used:

* ```{{ENDPOINT}}```

The endpoint used for project.
e.g. ```api.xyz.com```


***
## One Time Server Setup
* #####  Install PHP:
1. ```sudo LC_ALL=C.UTF-8 add-apt-repository ppa:ondrej/php```
2. ```sudo apt-get update```
3. ```sudo apt-get install apache2```
4. ```sudo apt-get install gzip```
5. ```sudo apt-get install php7.1```
6. ```sudo apt install zip unzip php7.1-zip```

* #####  Install PHP Plugins
1. ```sudo apt-get install php7.1-curl```
2. ```sudo phpenmod curl```
3. ```sudo apt-get install php7.1-xmlrpc```
4. ```sudo phpenmod xmlrpc```
5. ```sudo apt-get install php7.1-mbstring```
6. ```sudo phpenmod mbstring```
7. ```sudo apt-get install php7.1-dom```
8. ```sudo phpenmod dom```
9. ```sudo apt-get install php7.1-gd```
10. ```sudo phpenmod gd```
11. ```sudo apt-get install php7.1-mysql```

* #####  Install Composer
1. ```curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer```

* ##### Fixing the environment issue in ubuntu
1. ```sudo nano /etc/environment```

Paste the following lines:
```
LC_ALL=en_US.UTF-8
LANG=en_US.UTF-8
```

* ##### Apache Restart
1. ```sudo a2enmod rewrite```
2. ```sudo service apache2 restart```

* ##### Create Groups and add Users
1. ```sudo groupadd htmlowners```
2. ```sudo usermod -a -G htmlowners ubuntu```
3. ```sudo usermod -a -G htmlowners www-data```

***
## Per-Project Setup

* ##### Make Directory on server
1. ```cd /var/www```
2. ```sudo mkdir {{ENDPOINT}}```

* ##### Change User and group of directory
1. ```sudo chown -R ubuntu {{ENDPOINT}}```

* ##### Beanstalk Account Setup
1. Create Account
2. Create Repository
3. Copy your ssh key to beanstalk
4. copy beanstalk's ssh key on clipboard
5. ```sudo nano ~/.ssh/authorized_keys```
6. paste the copied ssh key from beanstalk here.
7. Deploy code from beanstalk

* #####  Create Virtual Host
1. ```cd /etc/apache2/sites-available/```
2. ```sudo nano {{ENDPOINT}}.conf```
3. ```Configure and save``` [Sample File Here](https://github.com/devslane/setup-helper/blob/master/api.xyz.com.conf)
4. ```sudo a2ensite {{ENDPOINT}}.conf```
5. ```sudo service apache2 restart```

* #####  Composer Install in Project
1. ```cd /var/www/{{ENDPOINT}}```
2. ```composer install```


* #####  Directory Permissions
###### below are to be run from {{ENDPOINT}} directory
1. ```sudo chown -R ubuntu:htmlowners storage``` 
2. ```sudo chown -R ubuntu:htmlowners bootstrap/cache``` 
3. ```sudo chmod -R g+s storage```
4. ```sudo chmod -R g+s bootstrap/cache```
5. ```sudo chmod -R u+s storage```
6. ```sudo chmod -R u+s bootstrap/cache```
7. ```sudo find storage -type d -exec chmod 775 {} \;```
8. ```sudo find storage -type f -exec chmod 664 {} \;```
9. ```sudo find bootstrap/cache -type d -exec chmod 775 {} \;```
10. ```sudo find bootstrap/cache -type f -exec chmod 664 {} \;```

###### below are to be run from /var/www directory
1. ```sudo find {{ENDPOINT}} -type d -exec chmod 775 {} \;```
2. ```sudo find {{ENDPOINT}} -type f -exec chmod 664 {} \;```

* #####  ENV Setup (Changes project to project)

