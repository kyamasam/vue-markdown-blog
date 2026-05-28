# Full Step-by-Step Installation Guide on Ubuntu server 20 
Preparing the System 
Update System Packages: 

Swtich to opt dir
```sh

cd /opt/
```

```sh
sudo apt update 
sudo apt upgrade

```
Install Required Dependencies: 

```sh
sudo apt install curl git
```
Installing Docker and Docker Compose 
Install Docker: 

```sh
curl -fsSL https://get.docker.com -o get-docker.sh 
sudo sh get-docker.sh
```
Install Docker Compose: 
```sh
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose 
sudo chmod +x /usr/local/bin/docker-compose 

```

## Configuring DNS Settings 
Before installing Mailcow, configure DNS settings for your domain. The full and detailed DNS Setup can be found in Mailcow’s Official Documentation. 

# Name              Type       Value
mail                IN A       serverip (eg 185.12.123.1)
autodiscover        IN CNAME   mail.example.org. (your ${MAILCOW_HOSTNAME})
autoconfig          IN CNAME   mail.example.org. (your ${MAILCOW_HOSTNAME})
@                   IN MX 10   mail.example.org. (your ${MAILCOW_HOSTNAME})

Get the mailcow code
```
git clone https://github.com/mailcow/mailcow-dockerized 
cd mailcow-dockerized

```
Generate Configuration File: 

./generate_config.sh

When prompted, enter your domain (e.g., mail.example.com). 
then your timezone 
Your timezone eg Africa/Nairobi


Start Mailcow: 

sudo docker-compose up -d

in the event you face any erros in this step,

switch to root user

```sh
sudo su

```

```sh
cd /opt/mailcow-dockerized/
./update.sh
```

This should ensure that everything is upto date

Verifying the Installation 
Check if all containers are running: 

```sh
sudo docker-compose ps 

```

Initial Configuration Settings 
After the installation, you need to perform initial configuration: 

Access Mailcow UI: Open a web browser and go to https://mail.example.com/admin/ You will be greeted by the Mailcow UI. 


Log in to Admin Panel: 

Default credentials are usually ```admin``` for username and ```moohoo``` for password. 

Change Admin Password: 

Go to ‘System -> Configuration’ -> ‘Access’ -> 'edit' and change the admin password. 

Configure DKIM and DMARC: 

In Mailcow UI, navigate to `System` -> `Configuration` -> `Options` ‘ARC/DKIM keys’. 

enter your domain name eg domain.com
leave selector as dkim
 
If you have an issue, 

run the following command 

```sh
openssl genrsa -out rsa.private 1024
cat rsa.private
```

Generate a new key and add the displayed DKIM record to your DNS settings. 

For DMARC, add a DMARC TXT record in your DNS settings (e.g., v=DMARC1; p=none; rua=mailto:postmaster@mail.example.com). 

