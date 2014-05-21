nginx-naxsi
===========

An RHEL Repository providing the Nginx Web Server with Naxsi Web Application Firewall


####About


This project provides a repository and a rpm for an installation of Nginx with Naxsi.


* Nginx - is a free, open-source, high-performance HTTP server and reverse proxy [http://wiki.nginx.org/Main]()
* Naxsi  -  NAXSI is an open-source, high performance, low rules maintenance WAF (Web Application Firewall) for NGINX 
[https://github.com/nbs-system/naxsi/]()
* Naxsi Basic Setup [https://github.com/nbs-system/naxsi/wiki/basicsetup]()
* 


###How to Install nginx-naxsi

####Nginx already installed

If you already have Nginx installed, you will want to backup your /etc/nginx directory before you install nginx-naxsi.

		mkdir ~/nginx-backups
		tar -czvf ~/nginx-backups/etc-nginx.tar.gz /etc/nginx

Now uninstall your old installation of nginx

		sudo yum remove nginx

Now follow the 'How to Install nginx-naxsi rpm' to complete your installation. 


####How to Install nginx-naxsi rpm


**Method 1 - Install RPM Using GitHub URL**

sudo yum install  https://github.com/simpliwp/nginx-naxsi/raw/master/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm


**Method 2 - Download RPM and install locally**

**Download**   


	wget https://github.com/simpliwp/nginx-naxsi/raw/master/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm


*OR*

	wget -O nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm https://github.com/simpliwp/nginx-naxsi/blob/master/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm?raw=true

**Install**

	sudo yum install nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm

**Method 3 - Download Master Branch and Install Locally**

	wget https://github.com/simpliwp/nginx-naxsi/archive/master.tar.gz
	tar -xsvf master.tar.gz
	sudo yum install ./nginx-naxsi-master/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm
 

**Method 4 - Install from Remote Repository**


Create a repository file

	sudo nano /etc/yum.repos.d/simpliwp-repos.repo

and add the following lines and save it:

		[simpliwp-nginx-naxsi]
		name=Optional Local Packages - $basearch
		baseurl=https://github.com/simpliwp/nginx-naxsi/raw/master/x86_64/rpms/
		enabled=1
		gpgcheck=0
		#gpgkey=file:///path/to/you/RPM-GPG-KEY


now you can install it from the repo

	sudo yum install nginx-naxsi



####Configuration

Setup your Nginx configuration and make sure its working properly before proceeding. If you had a previous configuration files that you backed up in the previous step, restore them.

Once Nginx is working properly without naxsi being enabled, edit your nginx.conf file to include the naxsi core rules:

	sudo nano /etc/nginx/nginx.conf

place the following line just inside the http block:

	include /etc/nginx/naxsi_core.rules;

e.g.: 

	http {


		#naxsi core rules
		include /etc/nginx/naxsi_core.rules;




Edit your virtual host's server block to include the basic ruleset for the root location:

e.g.: 	

		sudo nano  /etc/nginx/conf.d/mysite.com.conf

include the ruleset within the location block for the document root:

		...
		
		location / {
		#add naxsi rules
		include           /etc/nginx/naxsi-mysite.rules;

		....


edit the naxsi-mysite.rules and place in learning mode


	sudo nano /etc/nginx/naxsi-mysite.rules

uncomment the 'LearningMode' directive

		LearningMode; #Enables learning mode
		
		SecRulesEnabled;
		 #SecRulesDisabled;
		DeniedUrl "/RequestDenied";
		
		## check rules
		CheckRule "$SQL >= 8" BLOCK;
		CheckRule "$RFI >= 8" BLOCK;
		CheckRule "$TRAVERSAL >= 4" BLOCK;
		CheckRule "$EVADE >= 4" BLOCK;
		CheckRule "$XSS >= 8" BLOCK;


Once the ruleset has been added, mmake sure that a 500 error page exists and that you are pointing to it using a line like this : 


        location /RequestDenied {
                return 500;                                                                                                           
        }


Note that the 'RequestDenied' text must match the DeniedUrl in the ruleset file.


While in Learning Mode, the ruleset will log any violations of its rules. With LearningMode disabled (commented out)t and SecRulesEnabled, any ruleset violation with be directed to /RequestDenied, which will kick out a 500 server error. 



####WordPress Support


Naxsi provides rulesets for WordPress,DokuWiki and other configurations. See the GitHub project [naxsi-rules](https://github.com/nbs-system/naxsi-rules )for more information. Simply download or copy and paste their rulesets and use in place of the naxsi-mysite.rules file provided with this package.


####More Information

For more, please see the naxsi's [basic setup](https://github.com/nbs-system/naxsi/wiki/basicsetup) page.
