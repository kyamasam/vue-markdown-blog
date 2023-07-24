### Getting Started with Django Rest Part 1

## 1. Configuring environment variables in  your project

We will be using the python package `python-dotenv`

### Why do we need to store our environment variables in an environment variable file?
Environment variables are used to store secrets that are otherwise dangerous to share while using version control systems. They are also useful when deploying the application to different environments eg, `production` and `development`. These environments typically have different configurations settings eg on the database. 

Having an environment variable ensure that the application can typically have different settings for each environment. 

### What should go into the .env file?
Basically all configurations that are otherwise dangerous to share on version control systems

Eg Database credentials, api keys 

### Installing the package

```
pip install python-dotenv

```
### Creating the file 
In the root folder of the project, open the terminal and type the following
```
touch .env
```

Enter the following variables and any other variables that are useful in your project
You can then copy your secret key from settings.py and paste it here.
```
SECRET_KEY = 'YOUR SECRET KEY'
ENVIRONMENT = 'dev'
```
### Loading the environment variables in the settings file

```
import os
from dotenv import load_dotenv
load_dotenv()  # loads the configs from .env

SECRET_KEY = str(os.getenv('SECRET_KEY'))
## get debug settings from env
if ENVIRONMENT == 'dev':
    DEBUG = True
else:
    DEBUG = False
```

Delete your the original application secret key variable from the settings file


### Ensuring that the .env file is not committed to vcs (IMPORTANT)
Create a new file .env.example and add all the variables with empty strings in place of secrets
```
SECRET_KEY = ''
ENVIRONMENT = 'dev'
DEBUG = 'True'
```

### Add the .env file to .gitignore
If you do not have a .gitignore file, you can generate one [here]('https://www.toptal.com/developers/gitignore') 
Enter the following tags and any other that match your development environment

`Django`, `Visual StudioCode`, `Pycharm`

Copy the resulting .gitignore contents and place them in your .gitignore file

### Crating a .gitignore file. (Skip if you already have a .gitignore file)
``` 
touch .gitignore
```

Ensure that .env is one of the entries in your .gitignore file


