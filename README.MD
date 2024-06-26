# wsl2 php configs and commands

## set passwd for root
```
sudo su
sudo apt update
sudo apt upgrade
```
## install php
```
sudo apt install php
which apache2
sudo apt update
```

## install gui apps
```
sudo apt install gedit -y
sudo apt install nautilus -y
sudo apt install vlc -y
sudo apt install x11-apps -y
sudo apt install firefox
sudo apt update
cd /tmp
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install --fix-missing ./google-chrome-stable_current_amd64.deb
```

## install and config git
```
sudo apt-get install git
git config --global user.name "yourname"
git config --global user.email "yourname@emaildomain"
```

## install mysql and set passwd for root
```
sudo apt update
mysql --version
sudo apt install mysql-server
systemctl status mysql
sudo mysql
mysql -u root -p
systemctl status mysql
```
## install redis
```
sudo apt update
sudo apt install redis-server
sudo service redis-server start
sudo service redis-server status
```

## use phpredis client in proj
```sh
composer require predis/predis
```

## config redis

in `.env`,
```
CACHE_DRIVER=redis
```

in `database.php`,
```sh
'redis' => [
        
        # use phpredis client
        'client' => env('REDIS_CLIENT', 'phpredis'),

        'options' => [
            'cluster' => env('REDIS_CLUSTER', 'redis'),
            // If no prefix is ​​required, just comment
            // 'prefix' => env('REDIS_PREFIX', Str::slug(env('APP_NAME', 'laravel'), '_').'_database_'),
        ],
# other configs
]
```

in cache.php, change the default cache driver to `'redis'`
algo can add comment:
```
// If no prefix is ​​required, just comment
// 'prefix' => env('CACHE_PREFIX', Str::slug(env('APP_NAME', 'laravel'), '_').'_cache_'),
```

## go to wt(windows terminal), check docker-desktop
```
docker image ls --all
docker run hello-world
sudo apt update
```

## install c/c++ tools
```
sudo apt install g++ gdb make ninja-build rsync zip
sudo apt-get install curl
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

## install NodeJS
```
command -v nvm
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # This loads nvm bash_completion
nvm install --lts
nvm ls
which npm
```

## config php.ini
```
sudo apt install php8.1-mbstring php8.1-mysql php8.1-sockets php8.1-bz2 php8.1-gd php8.1-ftp php8.1-curl php8.1-openssl php8.1-xdebug
```

## apache conf
```
sudo vim /etc/apache2/apache2.conf
sudo vim /etc/apache2/sites-available/000-default.conf
```

## set web-server user
```
sudo chown -R www-data:www-data dir_path
```

## run as root
```
chmod -R 777 /var/www
rm -r html
touch index.php
DocumentRoot /var/www
<Directory /var/www>
     Options Indexes FollowSymLinks
     AllowOverride All
     Require all granted
</Directory>
```

## vhosts conf
### locate at /etc/apache2/sites-available/your_domain
```
<VirtualHost your_domain:80>
     ServerName your_domain
     DocumentRoot /var/www/your_domain/public

     <Directory /var/www/your_domain />
     Options Indexes FollowSymLinks
     AllowOverride All
     Require all granted
     </Directory>

     ErrorLog ${APACHE_LOG_DIR}/your_domain-error.log
     CustomLog ${APACHE_LOG_DIR}/your_domain-access.log combined
</VirtualHost>
```

## add soft link
```
sudo ln -s /etc/apache2/sites-available/yourdomain.com.conf /etc/apache2/sites-enabled/
```

## edit /etc/hosts
```
127.0.0.1 your_domain
```
## solve laravel-app code 500 error
```
sudo chown -R www-data:www-data /var/www/laravel-app
```

## fix error
UnexpectedValueException 
ile "/var/www/laravel.test/storage/logs/laravel.log" could not be opened in append mode: Failed to open stream: Permission denied
```sh
sudo chmod -R 777 /var/www/laravel.test
```

## set en-us as default charset
```
export LANG=en_US.UTF-8
```

## config nginx for PHP-FPM(php fastcgi process manager)
stop apache2
create file names `site1.com` in /etc/nginx/site-available
add lines:
```
server {
        listen 80;
        listen [::]:80;

        server_name site1.com www.site1.com;

        root /var/www/site1.com/public;
        index index.html index.htm index.php;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
         }

         location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                # see detail in /var/run/php
                fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        }
        location ~ /\.ht {
                deny all;
        }
}
```
add soft link in sites-enabled

## check php-fpm status
```
sudo systemctl status php{version}-fpm
```

### if there exists any issues, see details in /var/log/nginx/error.log
### see php-fpm config in /etc/php/{version}/fpm/php-fpm.conf
