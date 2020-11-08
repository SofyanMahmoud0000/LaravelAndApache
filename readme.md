# Laravel + Apache

In this repo, we are going to explain all steps needed to run laravel on apache server 

### We assume that 
* Our project is on github
* Our OS is linux

## Table of contents
* Install Git
* Install PHP (7.4)
* Install UFW
* Install PACHE
* Install MYSQL
* Install COMPOSER
* Clone the project of laravel
* Some modification in the project
* Apache configuration
* Development and support
* Authors

## Install Git

To install Git we use those two commands 

> sudo apt-get update

> sudo apt-get install git

## Install PHP (7.4)

To install PHP7.4 we can use those commands

> sudo apt -y install software-properties-common

> sudo add-apt-repository ppa:ondrej/php

> sudo apt-get update

> sudo apt -y install php7.4

To install some important extensions 

> apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl -y

## Install UFW 

To install UFW

> sudo apt-get install ufw


To enable ufw 

> sudo ufw enable

To show the status of UFW

> sudo ufw status 

To show the enabled application 

> sudo ufw app list

## Install apache

To install apache we will run those commands 

> sudo apt-get update

> sudo apt-get install apache2

Configure the firewall

> sudo ufw allow 'Apache'

Apache configration 

> sudo systemctl restart apache2

> sudo systemctl reload apache2

> sudo systemctl stop apache2

> sudo systemctl start apache2

Note 

> After installing apache the root will be in (/var/www/html), which means if you have a project named (x), then you must put this project in this path (/var/www/html/x)
to run your project on apache server 

## Install MYSQL

To install mysql 

> sudo apt-get update

> sudo apt-get install mysql-server

To start SQL 

> sudo systemctl start mysql

To enable SQL automatically after a reboot

> sudo systemctl enable mysql


## Install composer 

Update the package manager cache by running 

> sudo apt-get update

Install some dependencies by running 

> sudo apt install curl php-cli php-mbstring git unzip

Downloading and install composer 

> cd ~ && curl -sS https://getcomposer.org/installer -o composer-setup.php


Verify that the installer matches the SHA-384 hash for the latest installer found on 
the Composer Public Keys / Signatures page 

Click [here]['https://composer.github.io/pubkeys.html'] and copy the Installer Checksum (SHA-384), then assign it to a variable called HASH in terminal like that 

>HASH=544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061

Now execute the following PHP script to verify that the installation script is safe to run:

> php -r "if (hash_file('SHA384', 'composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

You’ll see the following output.

> Installer verified

To install composer globally, use the following command which will download and install Composer as a system-wide command named composer, under /usr/local/bin:

> sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer

You’ll see the following output:

> All settings correct for using Composer
Downloading...
Composer (version 1.6.5) successfully installed to: /usr/local/bin/composer
Use it: php /usr/local/bin/composer

Finally, You can test the compser installing by running 

> composer 

## Clone the project of laravel 

on the path (/var/www/html) we will clone the project by running 

> sudo git clone project_remote project_name


## Some modification in the project

After clonning the project, you need some modification 

* update composer 
    > compser update

* update the data of .env file 

* Generate new key
    > php artisan key:generate

* clear cache 
    > php artisan cache:clear
    > php artisan config:cache
    > php artisan config:clear

* Permissions
    > sudo chmod -R 775 storage
    > sudo chmod -R 775 public
    > sudo sudo chgrp -R www-data .

    Note: We must be in the project directory when we run those commands 

## Apache configuration

Now, we need to define new virtual host for our project 
In this virtual host, we determine which project will run on apache server 

if you go to this path
> /etc/apache2/sites-available

You will see the files of virtual host and the default one is (000-default.conf)

Now, we will create new one with any name such as 
> myproject.conf

And this file will have this code 

> \<VirtualHost *:80\>
	    ServerAdmin admin@server.in
   	    ServerName project_name.site
    	    ServerAlias project_name.site
	    \<Directory /var/www/html/project_name/public\>
    	    	Options Indexes FollowSymLinks
    		AllowOverride All
    		Require all granted
	    \</Directory\>
    	    DocumentRoot /var/www/html/project_name/public
    	    ErrorLog /var/www/html/project_name/error.log
    	    CustomLog /var/www/html/project_name/access.log combined
	\</VirtualHost\>

And now we need to enable this virtual host by running

> sudo a2ensite myproject.conf

And then disable the default virtual host by running 

> sudo a2dissite 000-default.conf

If you want to check the syntax of the virtual host files 

> sudo apachectl configtest

After enabling and disabling virtual host, you need to realod the apache server by running:

> sudo systemctl reload apache2

After all that, if you run the server you will only be able to access the root page 
and if you tried to access any route
you will face this problem 
> The requested URL was not found on this server.

To solve this problem you can kindly run 
> sudo a2enmod rewrite 

Now if you tried to access your website, you may see the index.php file as a plain text
and you can solve this problem by installing libapache2-mod-php
> sudo apt-get install libapache2-mod-php7.4

and then activate php7.4 by running 

> sudo a2enmod php7.4

Finally clear the cache 
> php artisan cache:clear
> php artisan config:cache
> php artisan config:clear


## Development and support 
If you have any questions about this topics you can send me a mail to sofyan1020@gmail.com


## Authors
* [Sofyan Mahmoud](https://github.com/sofyanmahmoud0000) - Computer engineer 






