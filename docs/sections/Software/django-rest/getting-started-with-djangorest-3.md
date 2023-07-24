### Adding Database Settings to .env file

In your env file, add the following. Ill be using postgresql for my testing database. 

```
PG_NAME = ''
PG_USER = ''
PG_PASSWORD = ''
PG_HOST = ''
PG_PORT = 5432
```

Add the following in your settings file
```
if os.environ.get('ENVIRONMENT') == 'testing':
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'URL': os.environ.get('PG_URL'),
            'NAME': os.environ.get('PG_NAME'),
            'USER': os.environ.get('PG_USER'),
            'PASSWORD': os.environ.get('PG_PASSWORD'),
            'HOST': os.environ.get('PG_HOST'),
            'PORT': os.environ.get('PG_PORT'),
        }
    }
else:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }

```

### Adding static files settings

In order to tell django where to store static files, Add the following to the `settings.py` file

```
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')

MEDIA_URL = "media/"
MEDIA_ROOT = os.path.join(BASE_DIR, 'media/')

```


### Adding urls 

in the `urls.py` file under `myProject/myProject/urls.py`
add the following lines


```python

from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

```