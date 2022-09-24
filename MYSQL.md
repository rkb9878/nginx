# INSTALLATION AND CONF. OF MYSQL IN UBUNTU SYSTEM


### To install it, Update the package index on your server if you've not done so recently:
```
    sudo apt update
```

Then install the  **mysql-server** package:
```
    sudo apt install mysql-server
```

Ensure that the server is runing using the **systemctl start** command:
```
    sudo systemctl start mysql.service
```

### Configuring MySQL

For fresh installations of **MySQL**, you'll want to run the DBMS's included sequrity script. This script changes some of the less secure default option for things like remote root login and sample user

#### Stepts
- sudo mysql
- ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
- exit
- mysql -u root -p

Run the security script:
```
    sudo mysql_secure_installation
```
This will take you through a series of prompts where you can make some changes to your MySQL installation’s security options. The first prompt will ask whether you’d like to set up the Validate Password Plugin, which can be used to test the password strength of new MySQL users before deeming them valid.

### Creating a Dedicated MySQL User and Granting Privileges

- login 
    ```
     mysql -u root -p
    ```
- Once you have access to the MySQL prompt, you can create a new user with a CREATE USER statement. These follow this general syntax:
    ```
    CREATE USER 'username'@'host' IDENTIFIED WITH authentication_plugin BY 'password';
    ```

    eg:
    if you want to access user from outside the system the change **host** into **%**

    ```
    CREATE USER 'user'@'localhost' IDENTIFIED BY 'P@$$w0rd';
    CREATE USER 'user'@'%' IDENTIFIED BY 'P@$$w0rd';
    ```
- After creating your new user, you can grant them the appropriate privileges. The general syntax for granting user privileges is as follows:
    ```
    GRANT PRIVILEGE ON database.table TO 'username'@'host';
    ```

    eg:
    ```
    GRANT ALL ON *.* TO 'myuser'@'localhost';
    GRANT ALL ON *.* TO 'myuser'@'%';
    ```
- Following this, it’s good practice to run the FLUSH PRIVILEGES command. This will free up any memory that the server cached as a result of the preceding CREATE USER and GRANT statements:
    ```
    FLUSH PRIVILEGES;
    ```

<hr style="background:red"/>

## Allow Remote Connections to MySQL

#### connections to a remote MySQL server is set up in 3 steps:

1. Edit MySQL config file.
2. Configure firewall.
3. Connect to remote MySQL server.

STEP.1: Edit MySQL config file.
    
    1. Access mysqld.cnf File
        
        sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
        
    2. Change Bind-Address IP

        DEFAULT
        bind-address = 127.0.0.1

        CHANGE INTO
        bind-address = 0.0.0.0

    3. Restart mysql.services

        sudo systemctl restart mysql

STEP.2: Configure firewall.

    1. List the application configurations that ufw knows how to work   with by typing
        
            sudo ufw app list

    2. Allow 3306 port to access from outside of the server

        sudo ufw allow 3306

    3. check Status

        sudo ufw status

STEP.3: Connect with mysql

    1. Connect mysql with terminal

        mysql -u username -h mysql_server_ip -p
    
**You can also connect with other software like DBEAVER or WorkBench or Xampp**
