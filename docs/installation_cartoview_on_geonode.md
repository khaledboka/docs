# Install Cartoview inside an existing GeoNode Installation on Ubuntu 16

> ### This Guid shows how to install Cartoview inside and existing GeoNode installation, If you don't have a GeoNode Installation please follow [GeoNode Docs](http://docs.geonode.org/en/master/index.html#)

## Contents:

### 1. [Install Cartoview Django Package](#install-cartoview-django-package)
### 2. [Edit some code at Cartoview](#edit-some-code-at-cartoview)
### 3. [Edit GeoNode `settings.py`](#edit-geonode-settingspy)
### 4. [Migrate and load default data of Cartoview](#migrate-and-load-default-data-of-cartoview)
### 5. [Generate static files required by Cartoview](#generate-static-files-required-by-cartoview)

---

## Install Cartoview Django Package

1. Download the latest stable release of [Cartoview](https://github.com/cartologic/cartoview/releases), Then extract it

2. Make sure the python virtualenv that has GeoNode installed in is activated

3. Navigate to the extracted directory of Cartoview
```shell
cd <cartoview-v1.8.x>
```

4. Install Cartoview from source code
```shell
pip install -e .
```

## Edit some code at Cartoview
1. Navigate to Cartoview directory where the first step installed from

2. Open the `views.py` in the directory `app_manager`

3. Replace the methode `view_app` at the class `StandardAppViews` by the following code
```python
class StandardAppViews(AppViews):
    .
    .
    .
    def view_app(self, request, instance_id, template=None, context={}):
        if template is None:
            template = self.view_template
        instance = get_object_or_404(AppInstance, pk=instance_id)
        context.update({
            "map_config": None,
            "instance": instance,
            "app_name": self.app_name
        })
        return render(request, template, context)
    .
    .
    .
```

## Edit GeoNode `settings.py`
1. Navigate to GeoNode main installation Directory, then navigate to geonode directory which has settings file
```sh
cd <geonode2.8.x>/geonode
```

2. Append the following lines to `settings.py` file
```python
# You can add the following line at the top of the file where all imports
import cartoview

PROJECT_DIR = os.path.dirname(os.path.abspath(__file__))
BASE_DIR = os.path.dirname(PROJECT_DIR)

cartoview_settings_path = os.path.join(
    os.path.dirname(cartoview.__file__), 'settings.py')
execfile(cartoview_settings_path)
```

## Migrate and load default data of Cartoview
1. Make sure the python virtualenv that has GeoNode installed in is activated

2. Navigate to the directory of GeoNode where `manage.py` file
```sh
python manage.py check
python manage.py makemigrations
python manage.py loaddata app_stores.json
```

## Generate static files required by Cartoview

> ### Install [NodeJS](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions) and [BowerJS](https://bower.io/#install-bower) globally on your system

1. Make a temperory directory where the files will be generated in
```sh
cd ~ && mkdir cartoview_static && cd cartoview_static
```

2. Create `.bowerrc` file
```sh
touch .bowerrc
```

3. copy the following lines into `.bowerrc` file and save
```json
{
    "directory": "./vendor/"
}
```

4. Create `bower.json` file
```sh
touch bower.json
```

5. copy the following lines into `.bowerrc` file and save
```json
{
  "name": "cartoview",
  "authors": [
    "Cartologic"
  ],
  "description": "cartoview client scripts",
  "main": "",
  "license": "MIT",
  "homepage": "http://cartoview.net",
  "private": true,
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "static/vendor/",
    "test",
    "tests"
  ],
  "dependencies": {
    "ol3": "ol3-bower#^3.18.2",
    "angular": "#1.5.9",
    "angular-animate": "#1.5.9",
    "angular-aria": "#1.5.9",
    "angular-messages": "#1.5.9",
    "angular-material": "#1.1.1",
    "angular-resource": "#1.5.9",
    "angular-resource-tastypie": "#1.0.4",
    "normalize-css": "#5.0.0",
    "angular-bootstrap": "#2.4.0",
    "angular-drag-and-drop-lists": "#2.1.0",
    "ng-image-appear": "#1.11.3",
    "lf-ng-md-file-input": "#1.5.1",
    "angular-img-fallback": "#0.2.0"
  }
}
```

6. Install the required packages locally inside the temporary directory
```sh
bower install
```

7. Copy the generated `vendor` directory to the `static` directory inside GeoNode

```sh
cp -r vedor/ <path_to_your_geonode_installation>/geonode/static/
```

8. Navigate to geonode installation directory where you will find `manage.py` file, then run:
```sh
python manage.py collectstatic --no-input
```
