# Guide to set-up PostgreSQL on remote Debian Server
## Table of contents
1. Connectiong to the Debian Server
    ```
    ssh -p <port> <username>@<server address>
    # example = ssh -p 2222 root@nonsense.ddns.net
    # Welcome to fish, the friendly interactive shell
    # Type help for instructions on how to use fish
    # root@debian12 ~#
    ```

2. Installing PostgreSQL
  * Install PostgreSQL   
    ```
    sudo apt-get update
    sudo apt-get install postgresql
    ```
  * Start service
    ```
    sudo service postgresql start
    ```
  * Login PostgreSQL
    ```
    sudo -u postgres psql
    ```
        
3. Configuring PostgreSQL for remote connection
  * Create user
    ```
    CREATE USER your_user WITH PASSWORD 'your_password';
    ```
  * Create database
    ```
    CREATE DATABASE your_database;
    ```
  * Grant new user permissions to the database
    ```
    GRANT ALL PRIVILEGES ON DATABASE your_database TO your_user;
    ```
  * Exit PostgreSQL
    ```
    \q
    ```
  * Edit PostgreSQL configuration file to allow remote connections:
    ```
    sudo nano /etc/postgresql/{version}/main/postg
    ```
    - Make sure the following line is uncommented and the value is '*':
      ```
      listen_addresses = '*'
      ```
    - Save and close the file. Next, open the pg_hba.conf file:
      ```
      sudo nano /etc/postgresql/{version}/main/pg_hba.conf
      # add line here in the end file
      host    all             all             0.0.0.0/0               md5
      ```
      Note: {version} = postgresql version installed on your device
  * Restart PostgreSQL to apply changes
      ```
      sudo service postgresql restart
      ```
      - Check to see if you can connect:
        ```
        psql -h localhost -U your_user -d your_database
        ```
        Note: If you connecting from other device, change `localhost` to address ip of server.
        Remember to provider password when asked.
