<samp>

# DEPLOY YOUR DJANGO + POSTGRESS PROJECT TO HEROKU

In order to deploy your django application to heroku, you need to add these requirements added to your `requirements.txt`:
  
~~~~~
python-decouple # for managing environment variables
django-on-heroku   # To setup your django settings ready for the heroku environment 
whitenoise      # To manage static files
dj-database-url # To parse the heroku's Postgres DB url
gunicorn        # To server your django project
psycopg2        # A database connector for postgresql in python
  
~~~~~

## SET UP LOCALY
  
### 1.  Install requirements.

  ```bash

  pip install python-decouple
  pip install whitenoise
  pip install dj-database-url
  sudo apt install django-on-heroku

  ```

### 2.  Configure your `setting.py` file
  
#### a. Add imports
  ... at the top of the `setting.py` file
  ```python
  from dj_database_url import parse as db_url
  from decouple import config
  ```

#### d. Configure database
  Below configurations, allow you to use `postgres` on heroku and `sqlite3` locally on your machine. Replace your database settings in the `setting.py` with the following.
  ```python
  ...

  DATABASES = {
    'default': config(
        'DATABASE_URL',
        default='sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3'),
        cast=db_url
    )
  }
  ...
  ```

#### c. Configure static files
  Make sure below settings are available in your `settings.py` files
  ```python

  STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

  STATIC_URL = '/static/'

  # Extra places for collectstatic to find static files.
  STATICFILES_DIRS = (
      os.path.join(BASE_DIR, 'static'),
  )

  STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

  ```

#### d. Configure django-heroku
  ... at the bottom in the `setting.py` file
  ```python
  import django_on_heroku
  django_heroku.settings(locals())
  ```

## 3. Add a Procfile
Create a file called `Procfile` at the root of your project. This file will be used by heroku to start your project
```bash
web: gunicorn <YOUR DJANGO PROJECT NAME>.wsgi
```

If you want to run djano migrations or any other pre-scripts on startup, use as below;
```bash
release: python manage.py makemigrations
release: python manage.py migrate
web: gunicorn <YOUR DJANGO PROJECT NAME>.wsgi
```
 
## 4. Deploy
Make sure you commit and push your project to github. After that go to ```www.heroku.com``` and create account after creating account you should create your first app and connect your app with your github repository and then you can choose automatic deployment or manual deployment.
  
You are ready to go now, your project can be deployed in heroku. 
You can use heroku-cli or the heroku.io website to ddeploy your app.

### NOTES:

- Heroku supports additional environment variables. To add some, go to settings in your app's dashboard.

- Use config function from python-decouple as below to read the environment variables.

  ```python
  config("ENVIRONMENT_VARIABLE", default=<A DEFAULT VALUE>)
  ```
  
</samp>
