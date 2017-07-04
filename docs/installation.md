#Cartoview
##Installation Requirements
- Install [Python2.7](https://www.python.org/)

- Install [1.8.7 <= Django <1.9a0](https://pypi.python.org/pypi/Django/1.8.7)

	!!! attention "Docker users"
	    - you need to install [docker-compose](https://docs.docker.com/compose/install/)




##Install On Ubuntu linux
- Follow these setps if you don't have Geonode  installed on your ubuntu system.<br/>

- Run the following command to download required packages  


		 sudo apt-get update
		 sudo apt-get install python-virtualenv python-dev libxml2 libxml2-dev libxslt1-dev zlib1g-dev libjpeg-dev libpq-dev libgdal-dev git default-jdk



- Install Java 8 (needed by latest GeoServer 2.9)
```
sudo apt-add-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```
## Geonode Installation

 - Create and activate the virtualenv

		virtualenv <your_virtual_env_name>
		source <your_virtual_env_name>/bin/activate

 - install pip

		sudo apt-get install python-pip


 - install geonode


		pip install geonode
		sudo apt-get install python-gdal


 - Create a symbolic link in your virtualenv


		ln -s /usr/lib/python2.7/dist-packages/osgeo  <your_virtual_env_name>/lib/python2.7/site-packages/osgeo   



!!! note
	    Verify your installation is completed by adding any layer in [Geonode][1]


- Don't Forget [Installation requirements](installation.md#installation-requirements)

## Database Installation and Configuration
- install postgreSQL database
  ```
  sudo apt-get install postgresql postgresql-contrib
  ```


- install postgis

	 ```
	 sudo apt-get install postgis
	 ```

- install pgadmin



		 sudo apt-get install pgadmin3

!!! note
		 for more Information on postgreSQL visit [postgresql]( https://www.postgresql.org/download/linux/ubuntu/)

- database Configuration
	- open pgadmin
	- create two new databases cartoView and cartoview_datastore
	- add postgis extension to the created databases by running the following sql query
```
		create extension postgis
```

##  CartoView Libraries Installation

- install cartview

	```
	pip install cartoview
	```


- Install CartoView_Django Project

	``` sh
	django-admin.py startproject --template=https://github.com/cartologic/cartoview-project-template/archive/master.zip --name django.env,uwsgi.ini,.bowerrc,server.py <your_project_name>
	```

- Go to your Project Folder

	``` sh
	cd <your_project_name>
	```


- Detect changes in ```app_manager``` App

	``` sh
	python manage.py makemigrations app_manager
	```

- create account Table

	``` sh
	python manage.py migrate account
	```



- Create Rest of tables :

	``` sh
	python manage.py migrate
	```

- load default User

	``` sh
	python manage.py loaddata sample_admin.json
	```

- load default oauth app

	``` sh
	python manage.py loaddata json/default_oauth_apps.json
	```

- Test Server (Development)
	- To start Development Server run this Command :

		``` sh
		python manage.py runserver 0.0.0.0:8000
		```

!!! Note "Info"
	 **(Optional)** if want to override any settings variable  (for example to change the
	  database password ,name or hostname ) rename``` local_settings.py.sample``` to ```local_settings.py``` then override settings you want inside ```local_settings.py```


!!! warning
	Don't Forget to Change ```<your_project_name>``` to desired name.

!!! important "Apps From Geo App Market"
	- to Install apps from [Geo App Market][2]

	- Load Default Store

		``` sh
		python manage.py loaddata app_stores.json
		```

  	- Install [nodejs](https://nodejs.org/en/) and then install [bower](https://bower.io/) we need them to install app_manager dependencies
	- in this step we will install required files in your project folder type :
		```sh
		bower install
		```
	- Collect Required files type:
		```sh
		python manage.py collectstatic --noinput
		```
	- Now you Can Install Apps from [Geo App Market][2]
	- Go to ```apps``` tab and click ```manage apps``` Button and install app you want

##install Geoserver
- Tomcat installation [tomcat]( https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-8-on-ubuntu-16-04)
- Download Geoserver war file [here](http://build.geonode.org/geoserver/latest/geoserver-2.9.x-oauth2.war)
- rename the war file to geoserver.war
- copy the war file inside webapps directory in tomcat
- restart tomcat server

- !!! warning "Important"
    cartview must be up and running 

- !!! warning "Important"
	in Production Configure Geoserver before uploading layers from [here](http://docs.geonode.org/en/master/tutorials/admin/geoserver_geonode_security/#geoserver-security-backend)

##Install On Windows

- Install [Python2.7](https://www.python.org/)
	- Make Sure to add the Python in the Path, as this is not setup by default
	- check add python.exe to PATH
		![python setup](img/python.png)
		![python setup](img/python2.png)
- Install Django 1.8.7 open cmd and type:

	```sh
	pip install django==1.8.7
	```

**We recommend to use Docker**

- Follow [Docker Instructions](docker.md#docker)


##Existing GeoNode Users
Check GeoNode and Cartoview version compatibility in [PYPI][4] then install Cartoview

- Requirements:
	- GeoNode == 2.5.15

	!!! attention
		We will Support more version of Geonode Soon!!

- install cartoview libraries

	``` sh
	pip install cartoview == <version>
	```

- Create Cartoview Project

	``` sh
	django-admin.py startproject --template=https://github.com/cartologic/cartoview-project-template/archive/master.zip --name django.env,uwsgi.ini,.bowerrc <your_project_name>
	```

- Go to your Project Folder

	``` sh
	cd <your_project_name>
	```

- detect Changes in app_manager

	``` sh
	python manage.py makemigrations app_manager
	```

- create account table

	``` sh
	python manage.py migrate account
	```



- create rest of database tables
	``` sh
	python manage.py migrate
	```

- Collect static Files

	``` sh
	python manage.py collectstatic --noinput
	```

- Now Development Server :
	``` sh
	python manage.py runserver 0.0.0.0:8000
	```
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
			``` sh
			python manage.py collectstatic --noinput
			```
		- restart server now you should restart server after installing any app
[1]: https://github.com/GeoNode/geonode
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
