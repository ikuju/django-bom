# BOM

BOM is a simple Django app to manage a bill of materials. It is
a level up from an excel spreadsheet including an indented
bill of materials, octopart price match, and more. It is written in Python 2.7 and Django 1.11.14 with future plans to support Python 3 and Django 2.

An example of the app in use can be seen [here](https://www.indabom.com).

If you already have a django project, you can skip to [Add Django Bom To Your App](#add-django-bom-to-your-app), otherwise [Start From Scratch](#start-from-scratch).

## Table of contents
   * [Start From Scratch](#start-from-scratch)
   * [Add Django Bom To Your App](#add-django-bom-to-your-app)
   * [Customize Base Template](#customize-base-template)
   * [Octopart Integration](#octopart-integration)
   
## Start From Scratch
1. To start from scratch we recommend setting up a virtual environment
```
virtualenv mysite
cd mysite
source bin/activate
```

2. From here install django, and set up your project.
```
pip install django==1.11.14
django-admin startproject mysite
cd mysite
python manage.py createsuperuser # make a user for your development environment
```

3. Continue on to [Add Django Bom To Your App](#add-django-bom-to-your-app).

## Add Django Bom To Your App
django-bom is a [reusable django application](https://docs.djangoproject.com/en/1.11/intro/reusable-apps/). If you don't already have a django project, you can follow some quick steps below to get up and running, or read about creating your first django app [here](https://docs.djangoproject.com/en/1.11/intro/tutorial01/). Note that django-bom currently supports python 2.7.

```
pip install django-bom
```

1. Add "bom" to your INSTALLED_APPS setting like this::

```
INSTALLED_APPS = [
    ...
    'bom',
]
```

2. Update your URLconf in your project urls.py like this::

```
url(r'^bom/', include('bom.urls')),
```

And don't forget to import include:

```
from django.conf.urls import include
```

3. Update your settings.py to add the bom context processor `'bom.context_processors.bom_config',` to your TEMPLATES variable, and create a new empty dictionary BOM_CONFIG.

```
TEMPLATES = [
    {
        ...
        'OPTIONS': {
            'context_processors': [
                ...
                'bom.context_processors.bom_config',
            ],
        },
    },
]
```

and

```
BOM_CONFIG = {}
```

4. Run `python manage.py migrate` to create the bom models.

5. Start the development server `python manage.py runserver` and visit http://127.0.0.1:8000/admin/
   to manage the bom (you'll need the Admin app enabled).

6. Visit http://127.0.0.1:8000/bom/ to begin.

## Customize Base Template
The base template can be customized to your pleasing. Just add the following configuration to your settings.py:

```
BOM_CONFIG = {
    'base_template': 'base.html',
}
```

where `base.html` is your base template.

## Octopart Integration
For part matching, make sure to add your Octopart api key to your settings.py in 
the BOM_CONFIG dictionary.
```
BOM_CONFIG = {
    'octopart_api_key': 'supersecretkey',
}
```
You can get an Octopart api key [here](https://octopart.com/api/home).
