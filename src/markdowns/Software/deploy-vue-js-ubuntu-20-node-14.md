
# Deploy Vue JS Application on Ubuntu 20.04

## Install Node Js

I have chosen node 14.x
You can change to the version of your choice by indicating the version number.
cd ~
curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh

## View the contents of the setup shell script

```
nano nodesource_setup.sh
```
Run the script

```
sudo bash nodesource_setup.sh
```

Install Node using the following command

```
sudo apt install nodejs
```

Check the node version

```
node -v
```
For more information, [visit this page](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)

## Clone the project to the server
```
git clone https://giturl
```

Navigate to project dir

```
cd projectdir
```

## Install Deps

```
npm install
```
## Build App For Production
```
npm run build
```

## Copy Files To Desired Location

Make Dir to serve files from
```
mkdir /var/www/html/projectdir
```

Give permissions

Add your user to the www-data group
```
usermod -a -G www-data administrator
```
Add folder to www-data group

```
chgrp www-data /var/www/html/<projectfolder>
chmod g+rwxs /var/www/html/<projectfolder>
```
```
```
```
cp -R dist/* /var/www/html/projectdir
```

## Configure the nginx server blocks

```
sudo nano /etc/nginx/sites-available/project
```
Enter the following contents

```
server {

listen 80;

listen [::]:80;

root /var/www/html/eac_rde_frontend;

index index.html;

server_name 196.41.38.246;

  

# X-Frame-Options is to prevent from clickJacking attack

add_header X-Frame-Options SAMEORIGIN;

  

# disable content-type sniffing on some browsers.

add_header X-Content-Type-Options nosniff;

  

# This header enables the Cross-site scripting (XSS) filter

add_header X-XSS-Protection "1; mode=block";

  

location / {

root /var/www/html/eac_rde_frontend;

try_files $uri /index.html;

}

location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg)$ {

expires 1d;

}

  

error_log  /var/log/nginx/eac_rde_frontend.log;

}
```


Create a symlink

```
cd /etc/nginx/sites-available
sudo ln -s project /etc/nginx/sites-enabled
```


> Sam