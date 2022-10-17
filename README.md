# aws-lightsail-bitnami-lamp
hosting lamp site on aws lightsail -step by step instructions
### Prerequisites
AWS account . lightsail costs $ neverthless cheap

- Step 1: create lamp instance. change AWS region and av zone according to your choice. -The closer your instance is to your users, the less latency they will experience.
![AWS Lightsail](/images/img1.png?raw=true "AWS Lightsail")

download the default or custom keypair - .pem file

- Step 2: Once the instance is up connect to it using SSH option on screen 
![SSH ](/images/img2.png?raw=true "SSH")
run the following command to save the bitnami application admin default password
#### cat bitnami_application_password
above command runs from home dir. if you are in another sub dir run the command as follows 
#### cat $HOME/bitnami_application_password.


- Step 3: Verify the default bitnami lamp is up and running visiting the public ip. this ip changes when the instance is rebooted. 
![lamp ](/images/img3.png?raw=true "lamp")
- so you can also opt to attach a static ip address in order to map to your own domain. under the Networking tab, choose Create static IP, and then attach to the instance .

- Step 4: Map domain to the static ip of lamp instance 
- You can choose to transfer DNS record to aws lightsail by following the instructions below
https://lightsail.aws.amazon.com/ls/docs/en_us/articles/lightsail-how-to-create-dns-entry
OR you can simply add the ip to the DNS record where your domain is currently registered (google, cloudflare, godaddy etc.,)
- Step 5: Create an SSL for apache - run following commands to create the certificate and add to your apache conf
#### a. generate private key
- sudo openssl genrsa -out /opt/bitnami/apache2/conf/server.key 2048
####  b. create a self signed cert
- sudo openssl x509 -in /opt/bitnami/apache2/conf/cert.csr -out /opt/bitnami/apache2/conf/server.crt -req -signkey /opt/bitnami/apache2/conf/server.key -days 365
#### c. backup private key (with encrypted pw)
- sudo openssl rsa -des3 -in /opt/bitnami/apache2/conf/server.key -out privkey.pem
#### d. Regenerate the key without password protection from this file 
- sudo openssl rsa -in privkey.pem -out /opt/bitnami/apache2/conf/server.key




