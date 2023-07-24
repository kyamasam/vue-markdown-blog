# Using Token authentication with django

In this tutorial, you will learn how to implement authentication using JWT token authentication.

Add the following to installed apps

```python
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
````

Run `python manage.py migrate` 

