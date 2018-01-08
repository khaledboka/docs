#Cartoview

##For Developers

- Cartoview Provides  [GeoApp Market](http://www.Cartoview.org) for GIS Developers.

- Develope your own App

	- Create a new empty App from Cartoview App template as follow in your Cartoview project directory 
	``` python
	cd apps
	django-admin.py startapp --template=https://github.com/cartologic/Cartoview-app-template/archive/master.zip <your_App_name>
	```

	- Edit Cartoview_project/apps/apps.yml and add entry for your app or create apps.yml file ,If ou cannot find it ,Add the following lines:
	``` yml
	- name: <app_name>
	  active: true
	  order: 0
	```
	- Add stores using the following command inside Cartoview project directory
	```
	python manage.py loaddata app_stores.json

	```
    
	- Add the new App to the database form Django admin interface

		![New App](img/developers_app.png)

	- Don't forget to check Single instance option if u want to test it for the first time

   ![New App](img/single_instance.PNG)

- Now on Cartoview Apps tab your app will Appear like this

![App Panel](img/apps_panel.PNG)

- Click Explore Button to open App home page

![App Home](img/app_home.PNG)

!!! success
    Congratulations, now you have created your first App on Cartoview
    you can upload it to Cartoview App market to make use of the features
    provided by Cartoview App market


