First you need a domain name. You can claim free domain on this site : https://nic.eu.org/
<br>
<br>
Then you need to link the domain name with DNS (your ipaddress where server is present). You can do it free on : https://dns.he.net/

<br>
<br>

Then you have to give your DNS address to Registrar where you have your domain name, so other people cannot use your domain name.

You can go back to "https://nic.eu.org/" site and paste all DNS addresses (e.g. "ns1.he.net") under Nameserver section.

<br>
<br>

Now your domain name is linked with DNS. Now you have to give your IpAddress in your DNS server. 

For that, go to "https://dns.he.net/" site, create new domain name with {your-domain-name}. It will automatically link your domain name with DNS "ns1.he.net"

Now you have to add your PC's Ip address under "A" record in that domain name with name = "ftp.{your-domain-name}" and value = "{your-ip}"

Now you have to add your PC's Ip address under "A" record in that domain name with name = "{your-domain-name}" and value = "{your-ip}"

Now you have to add "CNAME" record with name = "www.{your-domain-name}" and value = "{your-domain-name}", so you can access your website with "www" prefix.

<br>
<br>

Go to your domain name "http://www.{your-domain-name}" in your browser it should work. It will take your ip address's apache2 files. 

So you have to put your website's files in apache2 folder under : "/var/www/html".

<br>
<br>

If you want to link backend with your front end app in apache2 then you have to set proxy and reverse proxy.
Go to /etc/apache2/sites-available/000-default.conf and add below 2 lines :
```
ProxyPass /api http://localhost:8080
ProxyPassReverse /api http://localhost:8080
```
Restart apache2 server.

Now if you use {your-domain-name}/api then it will point to the backend where it is running.

<br>
<br>

If you want to turn on https/ssl in your app, then you have to turn on SSL and add certificate file path.
You can do this by adding below 5 lines in /etc/apache2/sites-available/default-ssl.conf file : 
```
SSLEngine on
SSLCertificateFile     /path/to/certificate/{your-domain-name}.pem 
SSLCertificateKeyFile /path/to/private-key/{your-domain-name}-key.pem
ProxyPass /api http://localhost:8080
ProxyPassReverse /api http://localhost:8080
```

<br>
<br>


Now check below command : 
```
sudo apachectl -t -D DUMP_MODULES | grep ssl
```
If it does not print anything then you have to turn on SSL, by below command : 
```
sudo a2enmod ssl
```
And then run above command, if it prints "mods ssl" then your HTTPS will be started. Go to "https://www.{your-domain-name}" and it should work.

<br>
<br>

To create free certificate, you can install mkcert, it generates free SSL certificates and run below command : 
```
wget https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-amd64
sudo cp mkcert-v1.4.3-linux-amd64 /usr/local/bin/mkcert
sudo chmod +x /usr/local/bin/mkcert 
sudo apt install libnss3-tools
mkcert -install
mkcert {your-domain-name}
```
It will generate ssl certificate for your domain name for free and you can give path of that certificate in above process.
