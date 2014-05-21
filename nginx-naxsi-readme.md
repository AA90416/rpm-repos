simpliwp/rpm-repos - nginx-naxsi Readme.md
===========

[Back to simpliwp/rpm-repos](https://github.com/simpliwp/rpm-repos/blob/master/README.md)


####About nginx-naxsi

The nginx-naxsi rpm package provides a Nginx binary compiled with the Naxsi Web Application Firewall module.


**Resources:**


* Nginx - is a free, open-source, high-performance HTTP server and reverse proxy [http://wiki.nginx.org/Main]()
* Naxsi  -  NAXSI is an open-source, high performance, low rules maintenance WAF (Web Application Firewall) for NGINX 
[https://github.com/nbs-system/naxsi/]()
* Naxsi Basic Setup [https://github.com/nbs-system/naxsi/wiki/basicsetup]()




#####nginx-naxsi Installation

If you already have Nginx installed, you will want to backup your /etc/nginx directory before you install nginx-naxsi.

		mkdir ~/nginx-backups
		tar -czvf ~/nginx-backups/etc-nginx.tar.gz /etc/nginx

Now uninstall your old installation of nginx

		sudo yum remove nginx

You can now either install directly from GitHub 

		sudo yum install  https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm  
		

or add it as a [repo](https://github.com/simpliwp/rpm-repos/blob/master/README.md#how-to-install-a-repo). see [the rpm-repos readme](https://github.com/simpliwp/rpm-repos/blob/master/README.md) for more installation  options.





#####nginx-naxsi Configuration

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

Please see the naxsi project's [basic setup](https://github.com/nbs-system/naxsi/wiki/basicsetup) page.  



#####Rpm Maintainer

Andrew Druffner <andrew@nomstock.com>
