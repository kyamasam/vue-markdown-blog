### Getting Started with Django Rest Part 1
# Create the project directory
```
mkdir myProject
cd myProject
```

# Create a virtual environment to isolate our package dependencies locally
```
virutalenv venv # On windowsuse  `python3 -m venv venv`
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
```
# Install Django and Django REST framework into the virtual environment
```
pip install django
pip install djangorestframework
```
# Set up a new project with a single application
```
django-admin startproject myProject .  # Note the trailing '.' character
django-admin startapp core
cd ..
```
# Add the application to installed apps in settings.py
```python
INSTALLED_APPS = [
   ...
    'core'
    ...
]
```

# Migrate changes to the database to ensure that django default tables are created
```
python manage.py migrate
```

# Create a new admin user that you can use.
```
python manage.py createsuperuser --username admin

```

Follow the prompts and set a password. 


# Setting up documentation before the project stats

For this tutorial we will be using DrfYsag - a documentation generator for django

## Setting up DrfYsag
### Install the package
```
pip install -U drf-yasg

```

### Add the package to installed apps in `settings.py`

```python 
INSTALLED_APPS = [
   ...
   'django.contrib.staticfiles',  # required for serving swagger ui's css/js files
   'drf_yasg',
   ...
]
```

### Adding new urls to the core urls ie `myProject/urls.py`

```python
...
from django.urls import re_path
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

...

schema_view = get_schema_view(
   openapi.Info(
      title="My Django Project",
      default_version='v1',
      description="This is a django rest api ",
      terms_of_service="https://www.google.com/policies/terms/",
      contact=openapi.Contact(email="contact@myproject.com"),
      license=openapi.License(name="BSD License"),
   ),
   public=True,
   permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
   path('swagger<format>/', schema_view.without_ui(cache_timeout=0), name='schema-json'),
   path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
   path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
   ...
]

```

According to the DrfYSag Docs, this exposes the following endpoints:


- A JSON view of your API specification at `/swagger.json`
- A YAML view of your API specification at `/swagger.yaml`
- A swagger-ui view of your API specification at `/swagger/`
- A ReDoc view of your API specification at `/redoc/`

Not that you access this by typing your server url eg `http://localhost:8000/swagger` or the respective url that you would like to access.


## Running the project
```
python manage.py runserver
```

In case you get an errror like the following

```
...
django.core.exceptions.ImproperlyConfigured: Application labels aren't unique, duplicates: staticfiles
```

Ensure that there are no duplicates in the `INSTALLED_APPS` section of the settings file.



