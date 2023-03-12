# Django-part1-maharat.ma
Let’s pause for a moment to examine the default project structure Django has provided for us. You examine this visually if you like by opening the new directory with your mouse on the Desktop. The `.venv` directory may not be initially visible because it is “hidden” but nonetheless still there.

```
├── django_project
│   ├── __init__.py
|   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── .venv/
```

The `.venv` directory was created with our virtual environment but Django has added a `django_project` directory and a `manage.py` file. Within `django_project` are five new files:

-   `__init__.py` indicates that the files in the folder are part of a Python package. Without this file, we cannot import files from another directory which we will be doing a lot of in Django!
-   `asgi.py` allows for an optional [Asynchronous Server Gateway Interface](https://asgi.readthedocs.io/en/latest/specs/main.html) to be run
-   `settings.py` controls our Django project’s overall settings
-   `urls.py` tells Django which pages to build in response to a browser or URL request
-   `wsgi.py` stands for [Web Server Gateway Interface](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface) which helps Django serve our eventual web pages.

The `manage.py` file is not part of `django_project` but is used to execute various Django commands such as running the local web server or creating a new app.

Let’s try out our new project by using Django’s lightweight built-in web server for local development purposes. The command we’ll use is `runserver` which is located in `manage.py`.

```
# Windows
(.venv) > python manage.py runserver

# macOS
(.venv) % python3 manage.py runserver
```

If you visit `http://127.0.0.1:8000/` you should see the following image:

![Django welcome page](https://djangoforbeginners.com/images/00_welcome40.png)

Note that the full command line output will contain additional information including a warning about `18 unapplied migrations`. Technically, this warning doesn’t matter at this point. Django is complaining that we have not yet “migrated” our initial database. Since we won’t actually use a database in this chapter the warning won’t affect the end result.

However, since warnings are still annoying to see, we can remove it by first stopping the local server with the command `Control+c` and then running `python manage.py migrate`.

```
# Windows
> python manage.py migrate


Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```

What Django has done here is create a SQLite database and migrated its built-in apps provided for us. This is represented by the new file `db.sqlite3` in our directly.

```
├── django_project
│   ├── __init__.py
|   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── db.sqlite3  # new
├── manage.py
└── .venv/
```

## Model-View-Controller vs Model-View-Template

Over time the **Model-View-Controller (MVC)** pattern has emerged as a popular way to _internally_ separate the data, logic, and display of an application into separate components. This makes it easier for a developer to reason about the code. The MVC pattern is widely used among web frameworks including Ruby on Rails, Spring (Java), Laravel (PHP), ASP.NET (C#) and many others.

In the traditional MVC pattern there are three major components:

-   Model: Manages data and core business logic
-   View: Renders data from the model in a particular format
-   Controller: Accepts user input and performs application-specific logic

Django only loosely follows the traditional MVC approach with its own version often called **Model-View-Template (MVT)**. This can be initially confusing to developers with previous web framework experience. In reality, Django’s approach is really it is a 4-part pattern that also incorporates URL Configuration so something like _MVTU_ would be a more accurate description.

The Django MVT pattern is as follows:

-   Model: Manages data and core business logic
-   View: Describes _which_ data is sent to the user but not its presentation
-   Template: Presents the data as HTML with optional CSS, JavaScript, and Static Assets
-   URL Configuration: Regular-expression components configured to a View

This interaction is fundamental to Django yet **very confusing** to newcomers so let’s map out the order of a given HTTP request/response cycle. When you type in a URL, such as `https://djangoproject.com`, the first thing that happens within our Django project is a URL pattern (contained in `urls.py`) is found that matches it. The URL pattern is linked to a single view (contained in `views.py`) which combines the data from the model (stored in `models.py`) and the styling from a template (any file ending in `.html`). The view then returns a HTTP response to the user.


## Create An App

Django uses the concept of projects and apps to keep code clean and readable. A single top-level Django project can contain multiple apps. Each app controls an isolated piece of functionality. For example, an e-commerce site might have one app for user authentication, another app for payments, and a third app to power item listing details. That’s three distinct apps that all live within one top-level project. How and when you split functionality into apps is somewhat subjective, but in general, each app should have a clear function.

To add a new app go to the command line and quit the running server with `Control+c`. Then use the `startapp` command followed by the name of our app which will be `note`.

```
# Windows
(.venv) > python manage.py startapp note
```

If you look visually at the `helloworld` directory Django has created within it a new `pages` directory containing the following files:

```
├── note
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
```

Let’s review what each new `pages` app file does:

-   `admin.py` is a configuration file for the built-in Django Admin app
-   `apps.py` is a configuration file for the app itself
-   `migrations/` keeps track of any changes to our `models.py` file so it stays in sync with our database
-   `models.py` is where we define our database models which Django automatically translates into database tables
-   `tests.py` is for app-specific tests
-   `views.py` is where we handle the request/response logic for our web app

Notice that the model, view, and url from the MVT pattern are present from the beginning. The only thing missing is a template which we’ll add shortly.

Even though our new app exists within the Django project, Django doesn’t “know” about it until we explicitly add it to the `django_project/settings.py` file. In your text editor open the file up and scroll down to `INSTALLED_APPS` where you’ll see six built-in Django apps already there. Add `note` at the bottom.

```
# django_project/settings.py
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "note",  # new
]
```

If you have Black installed in your text editor on “save” it will change all the single quotes `''` here to double quotes `""`. That’s fine. As noted previously, Django has plans to adopt Black Python formatting fully in the future.