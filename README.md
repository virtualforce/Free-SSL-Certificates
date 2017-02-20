# Free-SSL-Certificates
  It's pretty simple to get free ssl certificates for your specific domain, there is a service called [letsencrypt.org](https://letsencrypt.org/). Below instructions are tested out on Ubuntu 14.04/16.04, 

### Ubuntu 16.04
```
sudo apt-get update
sudo apt-get install letsencrypt
```

### Ubuntu 14.04
```
cd /usr/local/sbin
sudo wget https://dl.eff.org/certbot-auto
sudo chmod a+x /usr/local/sbin/certbot-auto
```
All things go well and there are no complications :) , you have enabled yourself to generate certificates via your machine. You can request certificates as illustrated below,   

### Ubuntu 16.04
```
sudo letsencrypt certonly -a webroot --webroot-path=(Place file where it is accessible) -d example.com
```

### Ubuntu 14.04
```
certbot-auto certonly -a webroot --webroot-path=(Place file where it is accessible) -d example.com
```
** For allowing wildcards add *. before the domain e.g  *.example.com or -d switch to add whatever subdomain's you want to allow certificate chain to add them. **

The above command will produce ACME verification files and you will need to make sure you place them at webroot of your application. For example in rails you can place files in public directory and certbot will verify that you are the owner of the domain and it's a legitimate request for the certificates.

After verification, you will be presented ** fullchain.pem ** and ** privkey.pem ** located at /etc/letsencrypt/live/example.com now you will just need to configure these files into your http entertainer and mix each request will certificate. Below are some basic setup you can go with, 

### Nginx 
```
ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
```

### Apache
For apache just pass --apache switch in the above commands when generating the certificate. 

### Verification
Go to https://www.ssllabs.com/ssltest/analyze.html?d=example.com , replace with your domain example.com. 

### Diffie-Hellman
It's not required to implement Diffie-Hellman group but it's very highly recommended you combine this with you ssl implementation `sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048` and place configuration in your http entertainer.  
