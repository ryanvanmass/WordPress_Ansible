# Steps to Install 

1. Install dependancies and start mariadb
```
zypper install php-gd  php-pdo  php-mysql  php-mbstring  php-simplexml  php-curl apache2-mod_php7 mariadb mariadb-tools

systemctl enable --now mysql
```

2. Set up Mariadb
```
# Log into Mariadb
mysql -u root -p

# Create the Database
create database wordpress character set utf8mb4;

# Create the User
create user wordpress@localhost identified by 'wordpress';

# Set Security Permissions
grant all on wordpress.* to wordpress@localhost;

# Flush Permissions
flush privileges;

exit;
```

3. Download the Binarries
```
wget https://wordpress.org/latest.tar.gz
```

4. Extract the Archive
```
tar xvf latest.tar.gz
```

5. Move wordpress to correct directory
```
mv wordpress /srv/www/htdocs/
cd /srv/www/htdocs/wordpress/
mv wp-config-sample.php wp-config.php
```

6. edit `wp-config.php` to show the following definitions
```
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'wordpress' );

/** MySQL database password */
define( 'DB_PASSWORD', 'wordpress' );
```

7. Changing file permissions
```
chown -R wwwrun:www /srv/www/htdocs/
chmod 775 -R /srv/www/htdocs/
```

8. Configure the virtual host for Wordpress
Add the following to `/etc/apache2/conf.d/wordpress.conf`
```
<virtualhost *:80>
servername www.ryanvanmassenhoven.com
documentroot "/srv/www/htdocs/wordpress/"
<directory "/srv/www/htdocs/">
AllowOverride All
Require all granted
</directory>
</virtualhost>
```


9.  Restart Apache
```
systemctl restart apache2
```

9. Open Ports on the firewall
```
firewall-cmd --add-port=443/tcp --permanent
firewall-cmd --add-port=80/tcp --permanent
firewall-cmd --reload
```