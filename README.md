# ace3 Compiled Cheatsheet


Scripts and config files to quickly start a new Debian & derivatives webserver that has:

- ufw
- node.js
- mariadb
- postgres
- nginx
- letsenscrypt

Assumes you are logged in as root.

# UFW
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

`GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

`FLUSH PRIVILEGES;`

`exit`


### (Optional) Testing MariaDB
	sudo systemctl status mariadb
    
### (Optional) Checking MariaDB Version
	sudo mysqladmin version
    # or
    mysqladmin -u admin -p version
    
