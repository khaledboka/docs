#Cartoview

##Docker
- be sure you installed all [requirements](installation.md#installation-requirements)
- Install [Docker][11]
- for windows users please use docker shell to execute the following commands
- Create Cartoview Project which contains required files to run and configure Docker using this command:

	``` python
	django-admin.py startproject --template=https://github.com/cartologic/cartoview-project-template/archive/master.zip --name django.env,uwsgi.ini,.bowerrc <your_project_name>
	```

- Replace ```<your_project_name>``` with the desired name 

- Go to your project Folder
	 ``` python
	 cd <your_project_name>
	 ```
- Open ```docker-compose.yml``` and Look at the port numbers for GeoServer and Postges and change the number before the ```:``` this will be the port on your machine
- If you want to run this project with a domain :
	- change ```django.env```(this is the file of the common django setting variables) file in your Project Folder to be some thing like this:

	!!! tip
		- default database username: ```cartologic``` and password: ```root```
		- default database user in ```postgis.env``` file in your project if want to change it.

	``` python
	DATABASE_URL=postgres://<database_user_name>:<database_password>@postgis:5432/cartoview
	GEOSERVER_PUBLIC_LOCATION=http://<your_domain_or_ip>/geoserver/
	GEOSERVER_LOCATION=http://geoserver:8080/geoserver/
	SITEURL=http://<your_domain_or_ip>
	ALLOWED_HOSTS=['*']
	```
		

- Start Docker images(cartoview,geoserver,postgres) type :

	``` sh
	make run
	```

- create  people table :

	``` sh
	make migrate_people
	```

	!!! bug
		this command will fire an error ignore it

- create rest of  tabels and collect static files :

	``` sh
	make static_db
	```

	!!! success "Success"
		Now you can Access cartoview on ```http://localhost``` or ```http://<your_domain_or_ip>```
	!!! warning "Important"
		Final step Configure Geoserver before uploading layers from [here](http://docs.geonode.org/en/master/tutorials/admin/geoserver_geonode_security/#geoserver-security-backend)

##Deployment notes

- !!! warning "Important"
	Once CartoView is installed is expected to install all apps from the app store automatically
	CartoView will not be able to restart docker when new apps install.
	After you install any new app or app update you will need to restart docker manually until this issue is addressed in the future
	- follow these steps to get apps working on nginx
		- collect static files using this commands in your project folder
			``` sh
			make collect_static
			```
		- restart server now with the following command you should restart server after installing any app
			``` sh
			docker-compose restart cartoview
			```


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
