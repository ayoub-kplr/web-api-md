## Étape 1: Créer un projet Django "Hello world"

1.  Ouvrez votre terminal et créez un nouveau dossier pour votre projet Django en tapant `mkdir nom_du_projet` (où `nom_du_projet` est le nom que vous souhaitez donner à votre projet).
2.  Accédez au nouveau dossier en tapant `cd nom_du_projet`.
3.  Ensuite, créez un environnement virtuel en tapant `python -m venv env` dans votre terminal.
4.  Activez votre environnement virtuel en tapant `source env/bin/activate` dans votre terminal (sur Windows, utilisez plutôt `env\Scripts\activate.bat`).
5.  Installez Django en tapant `pip install Django` dans votre terminal.
6.  Créez un nouveau projet Django en tapant `django-admin startproject hello_world` dans votre terminal (où `hello_world` est le nom que vous souhaitez donner à votre projet).
7.  Accédez au répertoire de votre projet en tapant `cd hello_world`.

## Étape 2: Créer une application Django "Hello world"

1.  Pour créer une application Django, tapez `python manage.py startapp hello` dans votre terminal (où `hello` est le nom que vous souhaitez donner à votre application).
2.  Ouvrez le fichier `settings.py` dans votre projet `hello_world`.
3.  Ajoutez votre nouvelle application à la liste des applications en ajoutant `'hello',` à la liste `INSTALLED_APPS`.
4.  Ouvrez le fichier `views.py` dans le dossier de l'application `hello`.
5.  Ajoutez le code suivant pour définir une vue simple qui affichera "Hello, world!" :


``from django.http import HttpResponse``
``def hello_world(request):``
    ``return HttpResponse("Hello, world!")``


6.  Ouvrez le fichier `urls.py` dans le dossier de l'application `hello`.
7.  Ajoutez le code suivant pour lier l'URL `/hello/` à la vue `hello_world` :
``from django.urls import path``
``from . import views``

``urlpatterns = [``
   `` path('hello/', views.hello_world, name='hello'),``
``]``

8.  Ouvrez le fichier `urls.py` dans le dossier du projet `hello_world`.
9.  Ajoutez le code suivant pour inclure les URL de votre application `hello` :
	from django.urls import include, path

``urlpatterns = [``
    ``path('', include('hello.urls')),``
``]``

10.  Enregistrez tous les fichiers et lancez votre serveur de développement en tapant `python manage.py runserver` dans votre terminal.
11.  Ouvrez votre navigateur et accédez à l'adresse `http://127.0.0.1:8000/hello/`. Vous devriez voir "Hello, world!" affiché dans votre navigateur.

