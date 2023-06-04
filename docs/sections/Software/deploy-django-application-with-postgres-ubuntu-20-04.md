# Deploy A Django Application on Ubuntu 20.04 and Postgres

## Step 1 — Installing Packages from the Ubuntu Repositories
    sudo apt update

	sudo apt install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx curl

## Install Postgres

    sudo apt install postgresql postgresql-contrib
More on this [here](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart)

## Creating users

    sudo -u postgres psql
    create database dbname;
    create user myuser with encrypted password 'password';
    grant all privileges on database mydb to myuser;

Quit the console

    \q

more on this [here](https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e)

## Creating a virtual environment

    sudo -H pip3 install --upgrade pip
    sudo -H pip3 install virtualenv
Create project folder

    cd /var/www
Clone your project from source control
```
git clone https://giturl
cd project_folder

```    
Add Virtual Environment


    virtualenv venv
    source venv/bin/activate
Install Required packages

    pip install -r requirements.txt

Set credentials in settings.py

Make migrations
``` 
python manage.py makemigrations
python manage.py migrate
 ```

Create admin user

```
python manage.py createsuperuser

```

Collect static files
```
python manage.py collectstatic

```

Test that the project runs correctly
```
sudo ufw allow 8000
python manage.py runserver 0.0.0.0:8000
```
Visit your server's ip address:8000

Install and configure gunicorn
```
pip  install  gunicorn
```

In the event where you face a permission issue while installing a package,

```
sudo chown <currentuser:currentuser> -R <virtualenvdir>
```
Make sure to replace currentuser with your username and virtualenvdir with your correct virtual environment directory

## Create a socket to listen to connection and manage application boot

```
sudo nano /etc/systemd/system/gunicorn.socket
```
Add the following content

```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target
```

## Create a systemd service file

```
sudo nano /etc/systemd/system/gunicorn.service

```
Add the following content
```
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=currentuser
Group=www-data
WorkingDirectory=/home/sammy/myprojectdir
ExecStart=/home/sammy/myprojectdir/myprojectenv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          myproject.wsgi:application

[Install]
WantedBy=multi-user.target
```

Grant your user account access to the service

```
sudo chown <currentuser:currentuser> /etc/systemd/system/gunicorn_rde.service
```

Add your user to the www-data group
```
usermod -a -G www-data administrator
```
Add folder to www-data group

```
chgrp www-data /var/www/<projectfolder>
chmod g+rwxs /var/www/<projectfolder>
```
> The first command changes the group ownership of the folder to that of
> the webserver. The second command gives members of the  `www-data`
> group read, write, enter-directory rights, and the group  `s`  flag
> will ensure that any files that get created inside that directory take
> `www-data`  as the group - so if you create a file as  `myuser`  the
> `www-data`  user will have access.



[Learn more here](https://askubuntu.com/questions/244406/how-do-i-give-www-data-user-to-a-folder-in-my-home-folder)

## Start the socket

```
sudo systemctl start gunicorn.socket
```

Enable the socket so that when a connection is made to it, it will start the gunicorn.service

    sudo systemctl enable gunicorn.socket

Confirm that the socket started successfully by running
```
sudo systemctl status gunicorn.socket
``` 

Confirm that the socket file exists within the run dir

    file /run/gunicorn.sock

If you need to make any changes to your .service files, ensure to reload the edits with
```
systemctl daemon-reload
```

## Configuring Nginx

```
nano /etc/nginx/sites-available/projectname
```
```
server {
    listen 80;
    server_name 196.41.38.246;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /var/www/eac_rde_backend;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn_rde.sock;
    }
}
```

Create a symbolic link to the sites-enabled dir

```
sudo ln -s /etc/nginx/sites-available/projectname /etc/nginx/sites-enabled
```
Test your Nginx configuration for syntax errors:

```
sudo nginx -t
```

If no errors are reported, go ahead and restart Nginx:

```
sudo service nginx restart
```

Delete the 8000 rule in the firewall settings since we no longer need it

```
sudo ufw delete allow 8000
```

Allow traffic via the 80 and 443 ports:

```
sudo ufw allow 'Nginx Full'
```

> Sam
> 