
# Apache-Server-Configuration in RedHat Linu
##### Based on Redhat Enterprise Linux 8

### What is an apache server?
Apache server also known as web server/HTTP server.
In windows machine we also known as IIS Server(Internet Information Services).
It is a free, open source and popular HTTP Server that runs on Unix-like operating 
systems including Linux and also Windows OS.Since its release 20 years ago, 
it has been the most popular web server. Apache Server mainly used by an organisation to deploy 
or provide web pages for their client machine within a LAN/internet environment.

### Packages & Demons
- Package -http,https
- Daemon -httpd
- Port -http 80, https 443
- Page name - index.html
- Logs -/var/log/httpd/error_log
* or  /var/log/httpd/access_log


### Apache Important Files and Directories
- The default server root directory (top level directory containing configuration files)
``` 
    /etc/httpd
```
- The main Apache configuration file
``` 
    /etc/httpd/conf/httpd.conf
```
- Additional configurations can be added in
```
    /etc/httpd/conf.d/
```
- Apache virtual host configuration file
```
    /etc/httpd/conf.d/vhost.conf
```
- Configurations for modules 
```
    /etc/httpd/conf.modules.d/
```
- Apache default server document root directory (stores web files) 
```
    /var/www/html
```
## Configuration
#### Step: 01
1. First check your machine ip and host name
```
    #ip a
    #hostname
```
2. Edit host file
```
    #vim /etc/hosts
```
3. Add a line in host file
```
Server ip 	server name
Example: 
192.169.126.129  server1.sifat.com
Then save & exit.
```
#### Step: 02
1. Install httpd package
```
    #yum install httpd* -y
```
2. Start httpd services
```
    #systemctl start httpd
    #systemctl enable httpd
```
3. Add httpd service on firewall 
```
    #firewall-cmd –-permanent —-add -service=httpd
```
4. Reload firewall
```
    #firewall-cmd —-reload
```
#### step: 03
1. Open apache server configuration file
```
    #vim /etc/httpd/conf/httpd.conf
```
2. Set line number 
```
    :set nu
```
3. Line number 34
The default server root directory (top level directory containing configuration files)
```
    /etc/httpd

you can change the root directory if want
```

4. Line number 45
```
Apache server port number. 
For security purposes you can change the port number.
```
5. Line number 59
```
Include conf.modules.d/*.conf 
This is the default modules configuration file
*  means any file with conf extension include this file
```
6. Line number 69 & 70 apache user & group
```
    User apache 
    Group apache
```

7. Line number 89
```
ServerAdmin	 root@localhost
This is the responsible server admin 
You have to change the default address root@localhost by server admin email address
Example:
ServerAdmin   support@sifat.com
```

8. Line number 122
```
DocumentRoot "/var/www/html" 
This is the default Apache default server document root directory (stores web files). 
```
9. Line number 167
```
DirectoryIndex index.html
This is a directory index. If you want to serve other types of files on the Apache server, you need to add more extensions.

Example:
DirectoryIndex  index.html  index.py (for python) index.php etc .
```
Now save & exit.

#### Step: 04
1. Go to default server document root directory (stores web files):
/var/www/html
```
    # cd /var/www/html
```
2. Now create a web file that  you want to share with html extension.
"index.html" & open it by vim editor
```
	#vim index.html
```
3. Write into this file following content 
```
 <html>
<title>Apache Server 1</title>
	<body>
	<marquee>welcome to my new website </marquee>
	<p>This is my first apache web server </p>
</body>
</html>
```
Now save & exit

#### Step 05
Go to any web browser and search
```
http//:server IP address or server name
Example:
http//:192.168.126.129
or
http//:server1.sifat.com
```

## Virtual hosting
Apache server also supports virtual hosting, allowing you to use one  apache web server to serve multiple websites.
If you want to host multiple websites or web apps on the same web server, you're probably going to be using virtual hosts.
It uses one IP address to host multiple websites.

### Configuration virtual host

#### Step 01

1. Create document directory
```
    #mkdir /test
```
2. Now change selinux context of test folder
```
    #semanage fcontext -a -t httpd_sys_content_t /test
    #restorecon -Rvv /test
```

3. Now , if you want to share two websites then create two folders.
We create two folders ict and math
```
    #mkdir /test/ict
    #mkdir /test/math
```

5. Inside theses folder(ict & math), create two files with html extension.
```
    # echo  “Welcome to the ICT  department website” > ict.html
    # echo  “Welcome to the Mathematics  department website” > math.html
```
#### Step 02

1. Go to apache virtual host configuration file 
```
    #cd /etc/httpd/conf.d/
```
2. Now create two configuration files with .conf extension.
```
    # vim  ict.conf
```
3. Write into this file following content
```
<Directory /test/ict>		[document directory path]
        Require all granted		[security for the directory]
        AllowOverride none
</Directory>

<VirtualHost 192.168.126.129:80>	[server IP or * :port]
        DocumentRoot	 /test/ict
        DirectoryIndex	 ict.html
        ServerAdmin	 root@ict.sifat.com		 [server admin always root]
        ServerName	 ict.sifat.com
        ServerAlias	 ict.sifat.com
        CustomLog 	"/var/log/httpd/error_log" combined
        ErrorLog 	"/var/log/httpd/access_log"
</VirtualHost>
```

## Again 
1. Open this file
```
    # vim  math.conf
```
2. Write into this file following content
```
<Directory /test/math>		[document directory path]
        Require all granted		[security for the directory]
        AllowOverride none
</Directory>

<VirtualHost 192.168.126.129:80>	[server IP or * :port]
        DocumentRoot	 /test/math
        DirectoryIndex	 math.html
        ServerAdmin 	root@math.sifat.com	[server admin always root]
        ServerName	 math.sifat.com
        ServerAlias	 math.sifat.com
        CustomLog 	"/var/log/httpd/error_log" combined
        ErrorLog 	"/var/log/httpd/access_log"
</VirtualHost>
```

#### Step 03

1. Edit /etc/hosts  file
```
#vim /etc/hosts
 Add following two lines 
192.168.126.129	ict.sifat.com 
192.168.126.129	math.sifat.com 
```
2. Now restart the httpd service
```
    #systemctl   restart httpd
```

####  Step 04

Go to any web browser and search
```
http//:server IP address or server name
Example:
http//:ict.sifat.com
http//:math.sifat.com
```
### Thank You

##### If you have any feedback, please reach out to me at mdsifathossain100@gmail.com
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/md-sifat-hossain-461274184/)
[![twitter](https://img.shields.io/badge/twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/ict_all)

[@sifat](https://github.com/MdsifatHossain)
