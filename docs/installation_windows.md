#Cartoview

### This document describes the installation of cartview with geonode Version 2.6.1 on windows

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
