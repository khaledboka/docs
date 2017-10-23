#Cartoview

### This document describes the installation of cartview with geonode Version 2.6.3
!!! warning
	in case of other versions of geonode these steps will not apply .

##Installation Requirements
- Install [Python2.7](https://www.python.org/download/releases/2.7/)

- Install Django
```
sudo apt-get update
sudo apt-get install python-django
```

!!! note
    for more information on installation visit [1.8.7 <= Django <1.9a0](https://pypi.python.org/pypi/Django/1.8.7)

!!! attention "Docker users"
    - you need to install [docker-compose](https://docs.docker.com/compose/install/)




##Install On Ubuntu linux

- Run the following command to download required packages  


		 sudo apt-get update
		 sudo apt-get install python-virtualenv python-dev libxml2 libxml2-dev libxslt1-dev zlib1g-dev libjpeg-dev libpq-dev libgdal-dev git default-jdk


## Database Installation
- install postgreSQL database
	```
	sudo apt-get install postgresql postgresql-contrib
	```


- install postgis ( extension to support geographic objects allowing location queries to be run in SQL. )

	```
	sudo apt-get install postgis
	```



		sudo apt-get install pgadmin3
!!! note
	    you can install pgadmin ( postgreSQL GUI tool using ): ``` sudo apt-get install pgadmin3```

!!! note
		for more Information on postgreSQL visit [postgresql](https://www.postgresql.org/download/linux/ubuntu/)

## Database Configuration

 - you will need to log into a user called postgres created by the installation to manage the database
 ```
 sudo -i -u postgres
 ```
 - change the postgres password
 ```
 psql
 \password
 ```
 - Exit out of the PostgreSQL prompt by typing
 ```
 \q
 ```
 - create two new databases cartoView and cartoview_datastore
 ```
 createdb cartoview
 createdb cartoview_datastore
 ```

!!! note
     you can change the databases name but make sure to change thier names in the local_settings.py file in cartoview project directory
 - add postgis extension to the created databases

 ```
 psql <database-name>
 CREATE EXTENSION postgis;
 ```

!!! note
    for more information on configuring the postgresql visit [postgreSQL](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04)

##install Geoserver

- Install Java 8 (needed GeoServer)
```
sudo apt-add-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

1) Download  [tomcat v9]( https://tomcat.apache.org/download-90.cgi)

2) Extract the tar folder and rename to tomcat

3) Download Geoserver war file [here](http://build.geonode.org/geoserver/latest/geoserver-2.9.x-oauth2.war)

4) Rename the war file to geoserver.war

5) Copy the war file inside webapps directory in tomcat

6) Create tomcat user we will do this by editing the tomcat-users.xml file:


			sudo nano path/to/tomcat/conf/tomcat-users.xml

!!! note
    make sure to change path/to/tomcat in the previous command with your tomcat path


7) Paste the following line
```<user username="admin" password="password"roles="manager-gui,admin-gui"/>
```
just before ```</tomcat-users > ```
at the end of the file and change the username and password

8) Edit context files in host-manager directory

     sudo nano path/to/tomcat/webapps/host-manager/META-INF/context.xml

9) Comment the line ``` <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> ``` by changing it to
				 ```
				 <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
								 allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
				 ```

10) Edit context files in manager directory

    sudo nano path/to/tomcat/webapps/manager/META-INF/context.xml

11) Comment the line ``` <Valve className="org.apache.catalina.valves.RemoteAddrValve"
        allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> ```  by changing it to
				```
				<!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
								allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
				```  

- start tomcat server

   ``` sh path/to/your/tomcat/bin/startup.sh
	 ```

- !!! note
      For more information on installation and Configuration of tomcat visit [tomcat](https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04)
- !!! warning "Important"
    cartview must be up and running

- !!! warning "Important"
	in Production Configure Geoserver before uploading layers from [here](http://docs.geonode.org/en/master/tutorials/admin/geoserver_geonode_security/#geoserver-security-backend)

## Geonode 2.6.3 Installation
 - Follow these setps if you don't have Geonode  installed on your ubuntu system.<br/>

 - Create and activate the python virtual environment

		virtualenv <your_virtual_env_name>
		source <your_virtual_env_name>/bin/activate

 - install pip ( a package management system used install and manage software packages written in Python )

		sudo apt-get install python-pip


 - install geonode 2.6.3


		pip install geonode==2.6.3
		sudo apt-get install python-gdal


 - Create a symbolic link of osgeo in your virtualenv needed by gdal to run properly  


		ln -s /usr/lib/python2.7/dist-packages/osgeo  <your_virtual_env_name>/lib/python2.7/site-packages/osgeo   



!!! note
	    Verify your installation is completed by adding any layer in [Geonode][1]


- Don't Forget [Installation requirements](installation_ubuntu.md#installation-requirements)



##  CartoView Libraries Installation

- install cartview

	```
	pip install cartoview
	```


- Install CartoView_Django Project

	```
	django-admin.py startproject --template=https://github.com/cartologic/cartoview-project-template/archive/master.zip --name django.env,uwsgi.ini,.bowerrc,server.py <your_project_name>
	```

- Go to your Project Folder

	```
	cd <your_project_name>
	```


- Detect changes in ```app_manager``` App

	```
	python manage.py makemigrations app_manager
	```

- create account Table

	```
	python manage.py migrate account
	```



- Create Rest of tables :

	```
	python manage.py migrate
	```

- load default User

	```
	python manage.py loaddata sample_admin.json
	```

- load default oauth app

	```
	python manage.py loaddata json/default_oauth_apps.json
	```

- Test Server (Development)
	- To start Development Server run this Command :

		```
		python manage.py runserver 0.0.0.0:8000
		```

!!! Note "Info"
	 **(Optional)** if want to override any settings variable  (for example to change the
	  database password ,name or hostname ) rename``` local_settings.py.sample``` to ```local_settings.py``` then override settings you want inside ```local_settings.py```


!!! warning
	Don't Forget to Change ```<your_project_name>``` to desired name.

!!! important "Apps From GeoApp Market"
	- to Install apps from [GeoApp Market][2]

	- Load Default Store

		```
		python manage.py loaddata app_stores.json
		```

  	- Install [nodejs](https://nodejs.org/en/) and then install [bower](https://bower.io/) we need them to install app_manager dependencies
	- in this step we will install required files in your project folder type :
		```
		bower install
		```
	- Collect Required files type:
		```

		python manage.py collectstatic --noinput
		```
	- Now you Can Install Apps from [Geo App Market][2]
	- Go to ```apps``` tab and click ```manage apps``` Button and install app you want





##Deployment notes

- !!! warning "Important"
	in Production Configure Geoserver before uploading layers from [here](http://docs.geonode.org/en/master/tutorials/admin/geoserver_geonode_security/#geoserver-security-backend)

- !!! warning "Important"
	Once CartoView is installed is expected to install all apps from the app store automatically
	At the moment CartoView will fully support apache server only
	For nginx deployments, CartoView will be able to detect new apps and get the updates, how ever to apply the updates, web server restart will be required to complete 		the process
	CartoView will not be able to restart nginx when new apps are installed.
	After you install or update apps from the app manager page you will need to restart nginx manually until this issue is addressed in the future
	- follow these steps to get apps working on nginx
		- collect static files using this commands
			```
			python manage.py collectstatic --noinput
			```
		- restart server now you should restart server after installing any app
[1]: http://docs.geonode.org/en/master/tutorials/users/managing_layers/upload.html
[2]: http://www.cartoview.org
[3]: http://demo.cartoview.net
[4]: https://pypi.python.org/pypi/cartoview
[5]: https://github.com/cartologic/cartoview/issues
[6]: http://cartoview.org/app/cartoview_map_viewer/
[7]: http://cartoview.org/app/cartoview_feature_list/
[8]: http://cartoview.org/app/cartoview_geonode_viewer/
[9]: https://twitter.com/ahmednosman
[10]: https://twitter.com/cartoview
[11]: https://www.docker.com/products/docker
