# angular_deployment
<!-- GETTING STARTED -->
## Angular Deployment on Linux Server

USE CASE:  frontend deployment for prototype and internal testing

### Prerequisites

* Curl 
  ```sh
  sudo apt-get update
  sudo apt-get install curl 
  ```

* Node v12 or higher & Npm
  ```sh
  curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
  sudo apt-get install -y nodejs
  ```
  Installing from node source will install npm by default

* Angular v7 or higher 
  ```sh
  npm install -g @angular/cli
  ```

* Nginx 
  ```sh
  sudo apt-get install nginx
  ```


### Building the Angular Application


1. Clone the repo
   ```sh
   git clone https://github.com/your_username_/Project-Name.git
   ```
2. cd into web directory

3. Install application dependencies
   ```sh
   npm install   
   ```
4. Build angular app to generate <b>dist</b> folder
   ```sh
   ng build --prod  
   ```

### Hosting the App
1. cd <b>/etc/nginx/sites-available/</b>

2. Write a new server block inside sites-available 
   ```sh
   server {
           listen 3000 default_server;
           listen [::]:3000 default_server;
   
           # change this directory into path to dist file
           root PATH_TO_DIST;
   
           index index.html index.htm index.nginx-debian.html;
   
           # change DOMAIN_NAME into DNS that is routed to the ip 
           server_name DOMAIN_NAME;
   
           location / {
                   try_files $uri $uri/ =404;
           }
   }
   ```

3. Activate the new sites available
   ```sh
   # change NEW_SERVER_BLOCK with serverblock file name
   ln -s /etc/nginx/sites-available/NEW_SERVER_BLOCK /etc/nginx/sites-enabled/
   ```


4. Check syntax error on server block
   ```sh
   nginx -t  
   ```

5. If there is no error, proceed to restart nginx service
   ```sh
   sudo systemctl restart nginx  
   or
   sudo service nginx restart
   ```

### Updating Source Codes and Redeploy
1. cd into project directory

2. Pull latest changes from repository
   ```sh
   git pull      
   ```

3. Build the new changes
   ```sh
   ng build --prod      
   ```
