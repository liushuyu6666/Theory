# Develop

## project

1. install python

2. create an empty folder

3. create virtual environment in this folder

   ```bash
   py -m venv venv
   ```

   1. the second `venv` here is the name of the virtual environment.

4. activate virtual environment

   ```shell
   venv\Scripts\activate.bat
   ```

5. install `django` under virtual environment,

   ```bash
   (venv) pip install django
   ```

6. start the project

   ```bash
   (venv)django-admin startproject myproject
   ```

   1. `myproject` is the project's name

7. run the **server**

   ```bash
   (venv) python manage.py runserver
   ```

   you will get a new `db.sqlite3`

   





## application

- start the application

  ```bash
  (venv) python manage.py startapp first_app
  ```

- add the name in the `INSTALLED_APPS` under `first_project\settings.py`

## view

view is used to handle with the requests and responses.

- add `functions` in `views.py` of the`first_app`.
- link it in the `urls.py`.



# Architecture

```tex
first_project
      |
      |-- first_project
      |        |
      |        |-- __init__.py: this directory can be treated as a package
      |        |-- settings.py: store settings 
      |        |-- urls.py: URL patterns
      |        |-- wsgi.py: Web Server Gateway Interface.
      |
      |
      |
      |-- first_app
      |        |-- migrations: store db information
      |        |       |-- __init__.py
      |        |-- __init__.py: this directory can be treated as a package
      |        |-- admin.py: register models, work with django admin interface
      |        |-- apps.py: configure
      |        |-- models.py: store models
      |        |-- tests.py: store test functions
      |        |-- views.py: handle with requests and responses
      |
      |-- manage.py: associate commands
      |-- db.sqlite3
```