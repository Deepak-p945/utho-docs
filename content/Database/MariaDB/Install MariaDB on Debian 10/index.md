---
slug: Install MariaDB on Debian 10
description: "Want to replace MySQL? Read through this guide, which explains how to install MariaDB on Debian 10."
keywords: ["mariadb", "Debian 10", "debian", "database", "mysql"]
modified: 2024-07-10
modified_by:
  name: Utho
published: 2024-07-10
title: "Installing MariaDB on Debian 10"
title_meta: "How to Install MariaDB on Debian 10"
relations:
    platform:
        key: how-to-install-mariadb
        keywords:
            - distribution: Debian 10
tags: ["debian","mariadb","database"]
authors: ["Pawan Kumar"]
---

MariaDB, established in 2009 by one of MySQL's original developers after Oracle's acquisition of MySQL during the Sun Microsystems merger, serves as a comprehensive alternative to the MySQL database management system. It functions as a full drop-in replacement and is actively developed and maintained by the MariaDB Foundation and community contributors, committed to its GNU GPL software status.

{{< note >}}
This guide assumes you are using a non-root user. Commands that require elevated privileges are prefixed with sudo. If you're unfamiliar with the sudo command, refer to our Users and Groups guide for more information.
{{< /note >}}

## Before You Begin

If you haven't already, create a Utho account and Compute Instance. Check out our Getting Started with Utho and Creating a Compute Instance guides for detailed instructions.

Follow our Setting Up and Securing a Compute Instance guide. This will help you update your system, configure your hostname, set the timezone, create a limited user account, and secure SSH access.

    To check your hostname run:

        hostname
        hostname -f

    The first command should show your short hostname, and the second should show your fully qualified domain name (FQDN) if you have one assigned.

## Install and Setup MariaDB

Install MariaDB using the package manager.

    sudo apt install mariadb-server

By default, MariaDB binds to localhost (127.0.0.2). If you need to connect to a remote database using SSH, refer to our MySQL remote access guide, which is also applicable to MariaDB.

{{< note >}}
Allowing unrestricted access to MariaDB on a public IP not advised but you may change the address it listens on by modifying the `bind-address` parameter in `/etc/mysql/my.cnf`. If you decide to bind MariaDB to your public IP, you should implement firewall rules that only allow connections from specific IP addresses.
{{< /note >}}

### MariaDB Client

The standard tool for interacting with MariaDB is the `mariadb` client, which installs with the `mariadb-server` package. The MariaDB client is used through a terminal using the `mysql` command.

### Root Login

1.  Log into MariaDB as the root user:

        sudo mysql -u root -p

1.  When prompted for login credentials, hit enter. By default MariaDB will authenticate you via the **unix_socket plugin** and credentials are not required.

    You'll then be presented with a welcome header and the MariaDB prompt as shown below:

    {{< output >}}
MariaDB [(none)]>
{{</ output >}}

1.  To generate a list of commands for the MariaDB prompt, enter `\h`. You'll then see:

    {{< output >}}
General information about MariaDB can be found at
http://mariadb.org

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.

For server side help, type 'help contents'

MariaDB [(none)]>
{{</ output >}}

### Securing the Installation

1. After accessing MariaDB as the root user of your database, enable the **mysql_native_password**
plugin to enable root password authentication:

        USE mysql;
        UPDATE user SET plugin='mysql_native_password' WHERE user='root';
        FLUSH PRIVILEGES;
        exit;

1.  Run the `mysql_secure_installation` script to address several security concerns in a default MariaDB installation:

        sudo mysql_secure_installation

During the setup process, you'll be prompted to change the MariaDB root password, remove anonymous user accounts, disable root logins outside of localhost, and remove test databases. It's recommended to respond with yes to these options for enhanced security. For more details about the script, refer to the MariaDB Knowledge Base.

## Using MariaDB

### Create a New MariaDB User and Database

1.  Login to the database again. This time, if you set a password above, enter it at the prompt.

        sudo mysql -u root -p

1. In the example below, `testdb` is the name of the database, `testuser` is the user, and `password` is the user's password. You should replace `password` with a secure password:

        CREATE DATABASE testdb;
        CREATE user 'testuser'@localhost IDENTIFIED BY 'password';
        GRANT ALL ON testdb.* TO 'testuser' IDENTIFIED BY 'password';

    You can shorten this process by creating the user *while* assigning database permissions:

        CREATE DATABASE testdb;
        GRANT ALL ON testdb.* TO 'testuser' IDENTIFIED BY 'password';

1.  Then exit MariaDB:

        exit;

### Create a Sample Table

1.  Log back in as `testuser`, entering the password when prompted:

        sudo mysql -u testuser -p

1.  Create a sample table called `customers`:

        USE testdb;
        CREATE TABLE customers (customer_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, first_name TEXT, last_name TEXT);

    - This creates a table with a `customer_id` field of the type `INT` for integer.
      - This field is auto-incremented for new records and used as the primary key.
    - Two other fields are created, `first_name` and `last_name` for storing the customer's name.

1.  View the new table:

        SHOW TABLES;

    {{< output >}}
+------------------+
| Tables_in_testdb |
+------------------+
| customers        |
+------------------+
1 row in set (0.00 sec)
{{</ output >}}

1.  Add some data:

        INSERT INTO customers (first_name, last_name) VALUES ('John', 'Doe');

1.  View the data:

        SELECT * FROM customers;

    {{< output >}}
+-------------+------------+-----------+
| customer_id | first_name | last_name |
+-------------+------------+-----------+
|           1 | John       | Doe       |
+-------------+------------+-----------+
1 row in set (0.00 sec)
{{</ output >}}

1.  Then exit MariaDB:

        exit;

## Reset the MariaDB Root Password

If you forget your root MariaDB password, it can be reset.

1.  Stop the current MariaDB server instance, then restart it with an option to not ask for a password:

        sudo systemctl stop mariadb

1.  Then execute the following command which will allow the database to start without loading the grant tables or networking.

        sudo systemctl set-environment MYSQLD_OPTS="--skip-grant-tables --skip-networking"

1.  Restart MariaDB:

        sudo systemctl start mariadb

1.  Login to the MariaDB server with the root account, this time without supplying a password:

        sudo mysql -u root

1.  Use the following commands to reset root's password. Replace `password` with a strong password:

        FLUSH PRIVILEGES;
        UPDATE mysql.user SET password = PASSWORD('password') WHERE user = 'root';

1.  Update the authentication methods for the root password:

        UPDATE mysql.user SET authentication_string = '' WHERE user = 'root';
        UPDATE mysql.user SET plugin = '' WHERE user = 'root';
        exit;

1.  Revert the environment settings to allow the database to start with grant tables and networking:

        sudo systemctl unset-environment MYSQLD_OPTS

1.  Then restart MariaDB:

        sudo systemctl start mariadb

1.  You should now be able to log into the database with your new root password:

        sudo mysql -u root -p