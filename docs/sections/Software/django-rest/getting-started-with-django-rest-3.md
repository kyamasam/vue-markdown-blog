# In this tutorial you will learn how to :
1. Set up cors headers 



## Setting up cors headers

### Installing the cors headers library

```
python -m pip install django-cors-headers
```

### Add the library to installed apps

```
INSTALLED_APPS = (
    ...
    'corsheaders',
    ...
)
```

### Add the following middleware classes to setting to ensure the application can listen in on responses.

Ensure that the middleware classes are as high as possible on the `MIDDLEWARE` array

```python
  MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
]
```


### Adding settings 

```python
CORS_ALLOW_ALL_ORIGINS = True # If this is used then `CORS_ALLOWED_ORIGINS` will not have any effect
CORS_ALLOW_CREDENTIALS = True
CORS_ALLOWED_ORIGINS = [
    'http://localhost:5432',
] # If this is used, then not need to use `CORS_ALLOW_ALL_ORIGINS = True`
CORS_ALLOWED_ORIGIN_REGEXES = [
    'http://localhost:5432',
]
```

For more information, checkout [this stack overflow question](https://stackoverflow.com/questions/35760943/how-can-i-enable-cors-on-django-rest-framework)

And the official documentation [here](https://github.com/adamchainz/django-cors-headers#configuration)
