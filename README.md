# Docker-based LAMP Stack
Simple and minimal directory using pre-made to use Docker Compose to run a LAMP stack. Uses latest PHP, Apache, MySQL, and phpMyAdmin.

### Instructions

1. Clone this repo.

2. (Optional) modify the MySQL credentials by editing `docker-compose.yml`.

3. Starting in the cloned directory, run the following:

   ```
   docker compose up -d
   ```

   Unless modified in `docker-compose.yml`, these are the ports you will need:

   - Apache webserver will be running on port 3000
   - phpMyAdmin will be running on port 8080.

4. Check the logs to see if you were successful:
   ```
   docker compose logs -f
   ```

5. (Optional) connect directly to MySQL and change the root password. First, find your MySQL instance's name:
   ```
   docker ps
   ```

   By default all instances are given a base name similar to the directory in which `docker-compose.yml` resides. 
   Start up an interactive mysql session, logging in as root:

   ```
   docker exec -it <mysql instance name> mysql -u root -p
   ```

   Run the following queries in mysql to change the root password:

   ```
   ALTER USER 'root' IDENTIFIED WITH mysql_native_password BY '<new root password>';
   flush privileges;
   ```

6. Build your Apache webserver. It is already configured to use a **bind volume**, mapping the `www` directory in the repo into `/var/www` in the Apache instance. Any changes you make within `www` will are also happening in the instance

7. (Optional) If you plan on reading or writing files from the server using PHP, be sure to provide the correct directory permissions for the `www-data` group (where `<username>` is your local / docker host username):
    ```
    sudo chown -R <username>:www-data www
    ```
Doing this will allow files within `/www/html` to be read/written/executed by either Apache or your local user. Very useful if you want to develop the site on your host machine, but run the server in your docker instance. 

8. When you are done, shut down the stack:
   ```
   docker compose down
   ```

   or if you also want to delete the MySQL database:
   ```
   docker compose down --volumes
   ```

   
