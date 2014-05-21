simpliwp/rpm-repos
===========



####About

This GitHub project provides an RPM repository of custom installation packages created for production use.


####Available Repos




- **[simpliwp-centos6]**  
	- name=SimpliWP CentOS $basearch
	- baseurl=https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/rpms/



- **[simpliwp-centos6-source]**
	- name=SimpliWP CentOS $basearch
	- baseurl=https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/srpms/  

- **[simpliwp-centos6-debug]** 
	- name=SimpliWP CentOS $basearch  
	- baseurl=https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/debug/  



		

##How to Install A Repo


**Create a repository file**

	sudo nano /etc/yum.repos.d/simpliwp.repo

**and add a section the following lines to it and save it:**

		[simpliwp-centos6]
		name=SimpliWP CentOS - $basearch
		baseurl=https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/rpms/
		enabled=1
		gpgcheck=0
		#gpgkey=file:///path/to/you/RPM-GPG-KEY


**Now install an rpm from it**

	sudo yum install nginx-naxsi


##How to Install an RPM Without Adding the Repo

**Method 1 - Install RPM Using GitHub URL**

	sudo yum install  https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm


**Method 2 - Download RPM and install locally**

**Download**   


	wget https://github.com/simpliwp/rpm-repos/raw/centos6/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm


*OR*

	wget -O nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm https://github.com/simpliwp/rpm-repos/blob/centos6/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm?raw=true

**Install**

	sudo yum install nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm

**Method 3 - Download Branch and Install Locally**

	wget https://github.com/simpliwp/rpm-repos/archive/centos6.tar.gz
	tar -xsvf centos6.tar.gz
	sudo yum install ./rpm-repos-centos6/x86_64/rpms/nginx-naxsi-1.6.0-1.el6.ngx.x86_64.rpm


###Available Packages

The following packages are available in the simpliwp-repos repository.

* nginx-naxsi  [Installation and Configuration](https://github.com/simpliwp/rpm-repos/blob/master/nginx-naxsi-readme.md)


#####Repo Maintainer

Andrew Druffner <andrew@nomstock.com>
