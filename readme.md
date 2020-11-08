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

```shell
sofyan@sofyan:~$ sudo apt-get update
```

```shell
sofyan@sofyan:~$ sudo apt-get install git
```

## Install PHP (7.4)

To install PHP7.4 we can use those commands



```shell
sofyan@sofyan:~$ sudo apt -y install software-properties-common
```

```shell
sofyan@sofyan:~$ sudo add-apt-repository ppa:ondrej/php
```

```shell
sofyan@sofyan:~$ sudo apt-get update
```

```shell
sofyan@sofyan:~$ sudo apt -y install php7.4
```

To install some important extensions 

```shell
sofyan@sofyan:~$ sudo apt install php7.4-common php7.4-mysql php7.4-xml php7.4-xmlrpc php7.4-curl php7.4-gd php7.4-imagick php7.4-cli php7.4-dev php7.4-imap php7.4-mbstring php7.4-opcache php7.4-soap php7.4-zip php7.4-intl -y
```

To install some important extensions 


## Install UFW 

To install UFW

```shell
sofyan@sofyan:~$ sudo apt-get install ufw
``` 


To enable ufw 

```shell
sofyan@sofyan:~$ sudo ufw enable
``` 

To show the status of UFW

```shell
sofyan@sofyan:~$ sudo ufw status 
``` 

To show the enabled application 

```shell
sofyan@sofyan:~$ sudo ufw app list
``` 

## Install apache

To install apache we will run those commands 

```shell
sofyan@sofyan:~$ sudo apt-get update
``` 

```shell
sofyan@sofyan:~$ sudo apt-get install apache2
``` 

Configure the firewall

```shell
sofyan@sofyan:~$ sudo ufw allow 'Apache'
``` 

Apache configration 

```shell
sofyan@sofyan:~$ sudo systemctl restart apache2
``` 

```shell
sofyan@sofyan:~$ sudo systemctl reload apache2
``` 

```shell
sofyan@sofyan:~$ sudo systemctl stop apache2
``` 

```shell
sofyan@sofyan:~$ sudo systemctl start apache2
``` 

Note 


> After installing apache the root will be in (/var/www/html), which means if you have a project named (x), then you must put this project in this path (/var/www/html/x)
to run your project on apache server 

## Install MYSQL

To install mysql 

```shell
sofyan@sofyan:~$ sudo apt-get update
``` 

```shell
sofyan@sofyan:~$ sudo apt-get install mysql-server
``` 

To start SQL 

```shell
sofyan@sofyan:~$ sudo systemctl start mysql
``` 

To enable SQL automatically after a reboot

```shell
sofyan@sofyan:~$ sudo systemctl enable mysql
``` 


## Install composer 

Update the package manager cache by running 

```shell
sofyan@sofyan:~$ sudo apt-get update
``` 

Install some dependencies by running 

```shell
sofyan@sofyan:~$ sudo apt install curl php-cli php-mbstring git unzip
```

Downloading and install composer 

```shell
sofyan@sofyan:~$ cd ~ && curl -sS https:getcomposer.org/installer -o composer-setup.php
```


Verify that the installer matches the SHA-384 hash for the latest installer found on 
the Composer Public Keys / Signatures page 

Click [here](https://composer.github.io/pubkeys.html) and copy the Installer Checksum (SHA-384), then assign it to a variable called HASH in terminal like that 

```shell
sofyan@sofyan:~$ HASH=544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061
```

Now execute the following PHP script to verify that the installation script is safe to run:

```shell
sofyan@sofyan:~$ php -r "if (hash_file('SHA384','composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```

You’ll see the following output.

```shell
sofyan@sofyan:~$ Installer verified
``` 

To install composer globally, use the following command which will download and install Composer as a system-wide command named composer, under /usr/local/bin:

```shell
sofyan@sofyan:~$ sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
```
You’ll see the following output:


> All settings correct for using Composer

> Downloading...
Composer (version 1.6.5) successfully installed to: /usr/local/bin/composer
Use it: php /usr/local/bin/composer

Finally, You can test the compser installing by running 

```shell
sofyan@sofyan:~$ composer 
```

## Clone the project of laravel 

on the path (/var/www/html) we will clone the project by running 

```shell
sofyan@sofyan:~$ sudo git clone project_remote project_name
``` 


## Some modification in the project

After clonning the project, you need some modification 

* update composer 
    ```shell
	sofyan@sofyan:~$ compser update
	```	 

* update the data of .env file 

* Generate new key
    ```shell
	sofyan@sofyan:~$ php artisan key:generate
	```	 

* clear cache 
    ```shell
	sofyan@sofyan:~$ php artisan cache:clear
	```	 
    ```shell
	sofyan@sofyan:~$ php artisan config:cache
	```	 
    ```shell
	sofyan@sofyan:~$ php artisan config:clear
	```	 

* Permissions
    ```shell
	sofyan@sofyan:~$ sudo chmod -R 775 storage
	```	 
    ```shell
	sofyan@sofyan:~$ sudo chmod -R 775 public
	```	 
    ```shell
	sofyan@sofyan:~$ sudo sudo chgrp -R www-data .
	```	 

    **Note** 
	>We must be in the project directory when we run those commands 

## Apache configuration

Now, we need to define new virtual host for our project 
In this virtual host, we determine which project will run on apache server 

if you go to this path
> /etc/apache2/sites-available
 

You will see the files of virtual host and the default one is (000-default.conf)

Now, we will create new one with any name such as **myproject.conf** 

And this file will have this code 

```conf
<VirtualHost *:80>
    ServerAdmin admin@server.in
    ServerName site_name.tld		# will be used in ssl (https)
    ServerAlias www.site_name.tld   # will be used in ssl (https)
	<Directory /var/www/html/project_name>
    	Options Indexes FollowSymLinks
    	AllowOverride All
    	Require all granted
	</Directory>
    DocumentRoot /var/www/html/project_name/public
    ErrorLog /var/www/html/project_name/error.log
    CustomLog /var/www/html/project_name/access.log combined
</VirtualHost>
```

And now we need to enable this virtual host by running

```shell
sofyan@sofyan:~$ sudo a2ensite myproject.conf
``` 

And then disable the default virtual host by running 

```shell
sofyan@sofyan:~$ sudo a2dissite 000-default.conf
``` 

If you want to check the syntax of the virtual host files 

```shell
sofyan@sofyan:~$ sudo apache2ctl configtest
``` 

After enabling and disabling virtual host, you need to realod the apache server by running:

```shell
sofyan@sofyan:~$ sudo systemctl reload apache2
``` 

After all that, if you run the server you will only be able to access the root page 
and if you tried to access any route
you will face this problem 

>The requested URL was not found on this server.

To solve this problem you can kindly run 
```shell
sofyan@sofyan:~$ sudo a2enmod rewrite 
``` 

Now if you tried to access your website, you may see the index.php file as a plain text
and you can solve this problem by installing libapache2-mod-php
```shell
sofyan@sofyan:~$ sudo apt-get install libapache2-mod-php7.4
``` 

and then activate php7.4 by running 

```shell
sofyan@sofyan:~$ sudo a2enmod php7.4
``` 

Finally clear the cache 
```shell
sofyan@sofyan:~$ php artisan cache:clear
``` 
```shell
sofyan@sofyan:~$ php artisan config:cache
``` 
```shell
sofyan@sofyan:~$ php artisan config:clear
``` 


## Development and support 
If you have any questions about this topics you can send me a mail to sofyan1020@gmail.com


## Authors
* [Sofyan Mahmoud](https://github.com/sofyanmahmoud0000) - Computer engineer 






