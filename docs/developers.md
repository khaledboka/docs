#Cartoview

##For Developers

- **Cartoview Provides a [GeoApp Market][2] for GIS Developers.**

- **Develope your own App**

	- Create a new empty app from cartoview app template as follow in your cartoview project directory
	``` python
	django-admin.py startapp --template=https://github.com/cartologic/cartoview-app-template/archive/master.zip <your_App_name>
	```

	- Edit cartoview_project/apps/apps.yml and add entry for your app
	``` yml
	- name: <app_name>
	  active: true
	  order: 0
	```
	- Add the App to the database form django admin interface

		![New App](img/developers_app.png)

	- **Don't forget to check Single instace option if u want to test it for the first time**

		![Single Instance](img/single_instance.PNG)

	- Now on Cartoview in Apps tab your app will Appear Some thing like this

		![App Panel](img/apps_panel.PNG)

	- Click Explore Button will open App home page

		![App Home](img/app_home.PNG)

	- Make Changes to your App

	- Now Zip your App and Upload it to [GeoApp Market][2]

	- Login on [GeoApp Market][2] and click ```MYAPPS``` tab

	- Click ```Submit New App``` Button and Fill Required Info

	- Now Users can install your app

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
