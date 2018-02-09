1. Install NODE and NPM  // https://tecadmin.net/install-latest-nodejs-npm-on-ubuntu/
    1.1 Add Node.js PPA
        sudo apt-get update
        sudo apt-get install python-software-properties
        curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    1.2 Install Node.js and NPM
        sudo apt-get install nodejs

2. Install PHP dependecies // 
    url: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-laravel-application-with-nginx-on-ubuntu-16-04

    sudo apt-get install php7.0-mbstring php7.0-xml composer unzip
    sudo apt-add-repository ppa:ondrej/php
    sudo apt-get update
    sudo apt install php7.1-fpm php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-zip php7.1-curl

3. Install MySQL
    sudo apt-get update
    sudo apt-get install mysql-server
    sudo apt-get install mysql-client
    mysql_secure_installation  // for secure install

4. Install Nginx
    url: https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04

    sudo apt-get update
    sudo apt-get install nginx

5. Nginx Cofiguration
    sudo nano /etc/nginx/sites-available/default
    server {
        listen 80;
        listen [::]:80;
        root /var/www/html;
        index  index.php index.html index.htm;
        server_name  example.com www.example.com;

        location / {
            try_files $uri $uri/ =404;       
        }

      
         # pass PHP scripts to FastCGI server
            #
            location ~ \.php$ {
                   include snippets/fastcgi-php.conf;
            #
            #       # With php-fpm (or other unix sockets):
                   fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
            #       # With php-cgi (or other tcp sockets):
            #       fastcgi_pass 127.0.0.1:9000;
            }
    }

6. Restart Nginx and PHP
    sudo service nginx restart
    sudo service php restart

7. Create database for laravel project
    sudo mysql -u root -p
    CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
    GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;
    exit;

8. Pull laravel projects from github and configure the application
    8.1 git clone https://github.com/nahid/talk-example.git
    8.2 composer install
    8.3 sudo nano .env
        APP_ENV=production
    APP_DEBUG=false
    APP_KEY=b809vCwvtawRbsG0BmP1tWgnlXQypSKf
    APP_URL=http://example.com

    DB_HOST=127.0.0.1
    DB_DATABASE=laravel
    DB_USERNAME=laraveluser
    DB_PASSWORD=password

9. Configure Mysql Remote connection
    9.1 find the mysqld.cnf file in /etc/mysql/mysql.conf.d/mysqld.cnf
        #Replace xxx with your IP Address 
            bind-address        = xxx.xxx.xxx.xxx

    9.2 configure.
        CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass';
        CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass';

        GRANT ALL ON *.* TO 'myuser'@'localhost';
        GRANT ALL ON *.* TO 'myuser'@'%';
10. https://github.com/nahid/talk-example - guide
11. Configure Nginx for laravel
    sudo chgrp -R www-data storage bootstrap/cache
    sudo chmod -R ug+rwx storage bootstrap/cache
    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example.com
    sudo nano /etc/nginx/sites-enabled/example.com
    sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
    sudo systemctl reload nginx

