# Compiled Server Cheatsheet

Scripts and config files to quickly start a new Debian & derivatives webserver that has:

- ufw
- Node.js
- MariaDb
- PostgreSQL
- Nginx
- Let's Encrypt (Certbot)
- Build Tools
- Docker
- Docker PostgreSQL

Assumes you are logged in as root.

# ufw

    ufw default deny incoming
    ufw default allow outgoing
    ufw allow 22  # ssh
    ufw allow 443 # https
    ufw allow 80  # http
    # allow port from specific IP
    # ufw allow from 1.1.1.1 to any port 22
    # allow port from specific interface
    # ufw allow in on eth0 to any port 80
    ufw enable

# Node.js

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
    # follow instructions in terminal
    nvm install 12.16.3

# MariaDB

    sudo apt update
    sudo apt install mariadb-server
    sudo mysql_secure_installation
    # follow instructions in terminal, and change the root password

### (Optional) Adjusting User Authentication and Privileges

    sudo mysql

Run SQL Command

    GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    exit

### (Optional) Testing MariaDB

    sudo systemctl status mariadb

### (Optional) Checking MariaDB Version

    sudo mysqladmin version
    # or
    mysqladmin -u admin -p version

# PostgreSQL

    sudo apt update
    sudo apt install postgresql postgresql-contrib

### Switching to Postgres Account

    sudo -i -u postgres
    #or access directly with
    sudo -u postgres psql

    # run postgres CLI
    psql

    # quit by using this command below
    \q

### Create Postgres Account

    createuser --interactive
    # or
    sudo -u postgres createuser --interactive

    # follow instructions in terminal

### Create Postgres Database

    createdb sammy
    # or
    sudo -u postgres createdb sammy

### Opening a Postgres Prompt with the New Role

    # first, create the unix account
    sudo adduser sammy

    # connect by using
    sudo -i -u sammy && psql
    # or
    sudo -u sammy psql

    # check connection by using
    conninfo

# Nginx

    sudo apt update
    sudo apt install nginx

    # optional if ufw is enabled
    sudo ufw app list
    sudo ufw allow 'Nginx HTTP'
    sudo ufw status

    # check status by using
    systemctl status nginx
    curl -4 icanhazip.com
    http://your_server_ip

### !**TODO** Configuration for PHP-FPM

    # configuration for PHP-FPM

### Managing the Nginx Process

    # disable
    sudo systemctl disable nginx

    # enable
    sudo systemctl enable nginx

    # stop
    sudo systemctl stop nginx

    # start
    sudo systemctl start nginx

    # restart
    sudo systemctl restart nginx

    # reload config
    sudo systemctl reload nginx

    # test config
    sudo service nginx configtest

### !**TODO** Nginx Templates

    # Node.js Express.js Template

    # PHP Laravel Template

    # CodeIgniter Template

### Symbolic Links for Nginx Config

    sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/

# Let's Encrypt

    sudo add-apt-repository ppa:certbot/certbot
    # follow instructions on terminal

    sudo apt install python-certbot-nginx

    # optional if using ufw
    sudo ufw status
    sudo ufw allow 'Nginx Full'
    sudo ufw delete allow 'Nginx HTTP'

### Obtaining an SSL Certificate

    sudo certbot --nginx -d example.com -d www.example.com

### Verifying Certbot Auto-Renewal

    sudo certbot renew --dry-run

# Build Tools

    apt-get install python python3 make build-essential

# Docker

    apt-get update
    apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    # apt-key fingerprint 0EBFCD88
    add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"
    apt-get update
    apt-get install docker-ce docker-ce-cli containerd.io

# Docker PostgreSQL

    cd ~
    mkdir postgresdata
    docker run --rm   --name pg-docker -e POSTGRES_PASSWORD=YOUR-PW -d -p 5432:5432 -v $HOME/postgresdata:/var/lib/postgresql/data  postgres
    docker ps
