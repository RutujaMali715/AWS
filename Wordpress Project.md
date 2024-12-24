# Create a wordpress website using EC2 and RDS

1. Launch an ec2 instance (Launch Amazon Linux 2)
2. Launch rds cluster 
3. Configure ec2 instance to connect to rds


**Install mysql client in ec2 instance to connect to RDS cluster**

```bash
sudo yum install -y mysql
```

**Export mysql endpoint as MySQSL_HOST Variable**

```bash
export MYSQL_HOST=<your-RDS-endpoint>
```

Example : export MYSQL_HOST=wordpress.cfkemkqqeqi8.ap-south-1.rds.amazonaws.com


**Now Connect to mysql to create required user users that we are going to use**

*Connect to RDS using below command (Replace the host-info and user info)*

```bash
mysql -h wordpress.cfkemkqqeqi8.ap-south-1.rds.amazonaws.com -P 3306 -u admin -p
```

Create required Database, user and provide permisisons to newsly created user on schema we created.

```bash
CREATE DATABASE wordpress;
CREATE USER 'wordpressuser' IDENTIFIED BY 'Rutuja12345';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser;
FLUSH PRIVILEGES;
Exit
```

**Now Install and configure apache**

```bash
sudo yum install -y httpd
```
Start Apache Service

```bash
sudo service httpd start
```

**Download a wordpress template and unzip the wordpress**

```bash
wget https://wordpress.org/latest.tar.gz
```

```bash
tar -xzf latest.tar.gz
```

```bash
ls
```

When you enter ls it should deisplay these files and directory "latest.tar.gz" and "wordpress"

```bash
cd wordpress
```

**create a wp-config file from sample file already provided**

```bash
cp wp-config-sample.php wp-config.php
```

**edit the wp-config.php file to point to database**

```bash
vim wp-config.php
```

***Replace the Database_name_here , "username_here", "password_here" and "rds-endpoint-name" with valid info***

```bash
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'database_name_here' );

/** MySQL database username */
define( 'DB_USER', 'username_here' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password_here' );

/** MySQL hostname */
define( 'DB_HOST', 'rds-endpoint-name' );
```


Now go to below link and it provides some information to update the wp-config file. It looks like below shared one.

```bash
https://api.wordpress.org/secret-key/1.1/salt/
```

***Acquire the valid information and update wp-config file accordingly***

```bash
define('AUTH_KEY',         'N`.[pmTxW?-6:Gx%OS_=S8X~N7|;{@yjN&xPAS(y+Ykdxf).oA-+3M/Q9Jyt#?&H');
define('SECURE_AUTH_KEY',  'O_t-/UJ&QoJ%f|RW[Dq)<L|~aMrkNAl+66BzAPLUWTTQNSKc/O#<_|g:UiKZ%8{Y');
define('LOGGED_IN_KEY',    '-!mak)|,lbKe./EsN7CI2w5IE9sl0-GbCvNAww$A{@~ot`@oHkMf2y~1_|M]-nOk');
define('NONCE_KEY',        'D|LL=FDDOzCx>WURKYA[l[CBV@+XhS0gCxUX[?u`)T)8o)a-uIXe(LSI w}^7pHQ');
define('AUTH_SALT',        'yb41agSwX*OqA&f) :G}T*PN8rf52(1BDAQy=Md]+jYW0o0C,4|u|k66`t*N,gxx');
define('SECURE_AUTH_SALT', 't|OcF/mpCnO4p*?HgO/DiX>m9=u>}1L.+)>-p5SRl|> ;iRB8^AI+)zY/llP3N+K');
define('LOGGED_IN_SALT',   'MHb-8<e wZ/2Xo:cISiF:tP`9|`I0s` Bd1 y 3X7K|c~-6S^`0[J2@IuSBdE+Wa');
define('NONCE_SALT',       'wcEwuc9TdhwSOxw0we59y;^8{G,xvtt6m0hj3?c-Rn9PglVr/OKg#QjCbyH|7Jb{');
```

**Now install dependencies**

```bash
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
```

**come back to home directory and copy all content to /var/www/html, Then restart the service.**

```bash
cd /home/ec2-user
```

```bash
sudo cp -r wordpress/* /var/www/html/
```
```bash
sudo service httpd restart
```

**Get your instance public IP and Paste it in browser, It should give you wordpress initial configuration page.** 
