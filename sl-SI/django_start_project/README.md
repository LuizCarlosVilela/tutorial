# Tvoj prvi Django projekt!

> Part of this chapter is based on tutorials by Geek Girls Carrots (https://github.com/ggcarrots/django-carrots).
> 
> Parts of this chapter are based on the [django-marcador tutorial](http://django-marcador.keimlink.de/) licensed under the Creative Commons Attribution-ShareAlike 4.0 International License. Vodič django-marcador tutorial je avtorsko delo Markus Zapke-Gründemann et al.

We're going to create a small blog!

Za začetek ustvarimo nov Django projekt. To bomo naredili s pomočjo programčka, ki smo ga dobili ob namestitvi Djanga. Ustvaril nam bo nekaj imenikov in datotek, ki nam bodo omogočile lažji in hitrejši začetek.

Imena nekaterih imenikov in datotek so Djangu zelo pomembna. Zato datotek, ki jih bomo v nadaljevanju ustvarili, ne preimenuj. Prav tako jih raje ne premikaj. Django ima namreč za veliko datotek v spominu shranjeno, kje približno se nahajajo in kako se jim reče, zato ga lahko s spremembami zmedemo.

> Ne pozabi, da moraš projekt delati znotraj virtualnega okolja. If you don't see a prefix `(myvenv)` in your console, you need to activate your virtualenv. Kako se to naredi smo podrobno pojasnili v poglavjih **Namestitev Djanga** in **Osnove dela z ukazno vrstico**. Typing `myvenv\Scripts\activate` on Windows or `source myvenv/bin/activate` on Mac OS X or Linux will do this for you.

<!--sec data-title="Create project: OS X or Linux" data-id="django_start_project_OSX_Linux" data-collapse=true ces-->

In your Mac OS X or Linux console, you should run the following command. **Don't forget to add the period (or dot) `.` at the end!**

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ django-admin startproject mysite .
    

> Pika je pomembna, ker programu pove, da mora v trenutni imenik (okrajšava zanj je .) namestiti Django).
> 
> **Note** When typing the command above, remember that you only type the part which starts by `django-admin`. The `(myvenv) ~/djangogirls$` part shown here is just example of the prompt that will be inviting your input on your command line.

<!--endsec-->

<!--sec data-title="Create project: Windows" data-id="django_start_project_windows" data-collapse=true ces-->

On Windows you should run the following command. **(Don't forget to add the period (or dot) `.` at the end)**:

{% filename %}command-line{% endfilename %}

    (myvenv) C:\Users\Name\djangogirls> django-admin.exe startproject mysite .
    

> Pika je pomembna, ker programu pove, da mora v trenutni imenik (okrajšava zanj je .) namestiti Django).
> 
> **Note** When typing the command above, remember that you only type the part which starts by `django-admin.exe`. The `(myvenv) C:\Users\Name\djangogirls>` part shown here is just example of the prompt that will be inviting your input on your command line.

<!--endsec-->

`django-admin.py` je program, ki bo zate naredil nekaj imenikov in datotek. Recimo takih:

    djangogirls
    ├── manage.py
    ├── mysite
    │   ├── __init__.py
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── myvenv
    │   └── ...
    └── requirements.txt
    

> **Note**: in your directory structure, you will also see your `venv` directory that we created before.

`manage.py` is a script that helps with management of the site. With it we will be able (amongst other things) to start a web server on our computer without installing anything else.

`settings.py` vsebuje podatke o nastavitvah naše spletne strani.

`urls.py` pove, kateri del spletne strani naj se pokaže, ko v brskalnik vpišemo določen url naslov.

Ostale datoteke zaenkrat prezrimo, saj jih ne bomo rabili spreminjati. Nikakor pa jih ne spreminjaj ali briši!

## Spreminjanje nastavitev

Malce bomo spremenili datoteko `mysite/settings.py`. Odpri jo v urejevalniku programske kode, ki si ga prej namestila.

**Note**: Keep in mind that `settings.py` is a regular file, like any other. You can open it from inside the code editor, using the "file -> open" menu actions. This should get you the usual window in which you can navigate to your `settings.py` file and select it. Alternatively, you can open the file by navigating to the djangogirls folder on your desktop and right-clicking on it. Then, select your code editor from the list. Selecting the editor is important as you might have other programs installed that can open the file but will not let you edit it.

Na naši strani bi bilo dobro imeti pravilen čas in datum. Go to [Wikipedia's list of time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) and copy your relevant time zone (TZ) (e.g. `Europe/Berlin`).

In `settings.py`, find the line that contains `TIME_ZONE` and modify it to choose your own timezone. For example:

{% filename %}mysite/settings.py{% endfilename %}

```python
TIME_ZONE = 'Europe/Berlin'
```

A language code consist of the language, e.g. `en` for English or `de` for German, and the country code, e.g. `de` for Germany or `ch` for Switzerland. If English is not your native language, you can add this to change the default buttons and notifications from Django to be in your language. So you would have "Cancel" button translated into the language you defined here. [Django comes with a lot of prepared translations](https://docs.djangoproject.com/en/2.2/ref/settings/#language-code).

If you want a different language, change the language code by changing the following line:

{% filename %}mysite/settings.py{% endfilename %}

```python
LANGUAGE_CODE = 'de-ch'
```

We'll also need to add a path for static files. (We'll find out all about static files and CSS later in the tutorial.) Go down to the *end* of the file, and just underneath the `STATIC_URL` entry, add a new one called `STATIC_ROOT`:

{% filename %}mysite/settings.py{% endfilename %}

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

When `DEBUG` is `True` and `ALLOWED_HOSTS` is empty, the host is validated against `['localhost', '127.0.0.1', '[::1]']`. This won't match our hostname on PythonAnywhere once we deploy our application so we will change the following setting:

{% filename %}mysite/settings.py{% endfilename %}

```python
ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']
```

> **Note**: If you're using a Chromebook, add this line at the bottom of your settings.py file: `MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'`
> 
> Also add `.amazonaws.com` to the `ALLOWED_HOSTS` if you are using cloud9
> 
> If you are hosting your project on `Glitch.com`, let us protect the Django secret key that needs to remain confidential (otherwise, anyone remixing your project could see it):
> 
> - First, we are going to create a random secret key. Open the Glitch terminal again, and type the following command: {% filename %}.env{% endfilename %} 
>         bash
>         python -c 'from django.core.management.utils import get_random_secret_key; \
>               print(get_random_secret_key())' This should display a long random string, perfect to use as a secret key for your brand new Django web site. We will now paste this key into a 
>     
>     `.env` file that Glitch will only show you if you are the owner of the web site. 
> - Create a file `.env` at the root of your project and add the following property in it: {% filename %}.env{% endfilename %} 
>         bash
>         # Here, inside the single quotes, you can cut and paste the random key generated above
>         SECRET='3!0k#7ds5mp^-x$lqs2%le6v97h#@xopab&oj5y7d=hxe511jl'
> 
> - Then update the Django settings file to inject this secret value and set the Django web site name: {% filename %}mysite/settings.py{% endfilename %} 
>         python
>         SECRET_KEY = os.getenv('SECRET')
> 
> - And a little further, in the same file, we inject the name of your new Glitch website: {% filename %}mysite/settings.py{% endfilename %} 
>         python
>         ALLOWED_HOSTS = [os.getenv('PROJECT_DOMAIN') + ".glitch.me"] The 
>     
>     `PROJECT_DOMAIN` value is automatically generated by Glitch. It will correspond to the name of your project.

## Set up a database

Obstaja veliko različne programske opreme, ki lahko shranjuje podatke za našo stran (taki programski opremi rečemo podatkovna baza). Uporabili bomo privzeto, `sqlite3`.

Kot lahko opazimo v `mysite/settings.py`, je sqlite3 že nastavljena:

{% filename %}mysite/settings.py{% endfilename %}

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

Podatkovno bazo bomo ustvarili z ukazom: `python manage.py migrate` (biti moramo v imeniku `djangogirls`, saj le ta vsebuje datoteko `manage.py`). Pokazati se mora nekaj takšnega:

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py migrate
    Operations to perform:
      Apply all migrations: auth, admin, contenttypes, sessions
    Running migrations:
      Rendering model states... DONE
      Applying contenttypes.0001_initial... OK
      Applying auth.0001_initial... OK
      Applying admin.0001_initial... OK
      Applying admin.0002_logentry_remove_auto_add... OK
      Applying contenttypes.0002_remove_content_type_name... OK
      Applying auth.0002_alter_permission_name_max_length... OK
      Applying auth.0003_alter_user_email_max_length... OK
      Applying auth.0004_alter_user_username_opts... OK
      Applying auth.0005_alter_user_last_login_null... OK
      Applying auth.0006_require_contenttypes_0002... OK
      Applying auth.0007_alter_validators_add_error_messages... OK
      Applying auth.0008_alter_user_username_max_length... OK
      Applying auth.0009_alter_user_last_name_max_length... OK
      Applying sessions.0001_initial... OK
    

Končano! Zdaj lahko zaženemo strežnik in se prepričamo, da naša stran deluje!

## Starting the web server

Biti moraš v imeniku `djangogirls`, saj le ta vsebuje datoteko `manage.py`. Strežnik zaženeš tako, da v ukazno vrstico vpišeš `python manage.py runserver`:

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py runserver
    

If you are on a Chromebook, use this command instead:

{% filename %}Cloud 9{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py runserver 0.0.0.0:8080
    

or this one if you are using Glitch:

{% filename %}Glitch.com terminal{% endfilename %}

    $ refresh
    
    

If you are on Windows and this fails with `UnicodeDecodeError`, use this command instead:

{% filename %}command-line{% endfilename %}

    (myvenv) ~/djangogirls$ python manage.py runserver 0:8000
    

Now you need to check that your website is running. Open your browser (Firefox, Chrome, Safari, Internet Explorer or whatever you use) and enter this address:

{% filename %}browser{% endfilename %}

    http://127.0.0.1:8000/
    

If you're using a Chromebook and Cloud9, instead click the URL in the pop-up window that should have appeared in the upper right corner of the command window where the web server is running. The URL will look something like:

{% filename %}browser{% endfilename %}

    https://<a bunch of letters and numbers>.vfs.cloud9.us-west-2.amazonaws.com
    

or on Glitch:

    https://name-of-your-glitch-project.glitch.me
    

Congratulations! You've just created your first website and run it using a web server! Isn't that awesome?

![Install worked!](images/install_worked.png)

Note that a command window can only run one thing at a time, and the command window you opened earlier is running the web server. As long as the web server is running and waiting for additional incoming requests, the terminal will accept new text but will not execute new commands.

> We reviewed how web servers work in the **How the Internet works** chapter.

To type additional commands while the web server is running, open a new terminal window and activate your virtualenv -- to review instructions on how to open a second terminal window, see [Introduction to the command line](../intro_to_command_line/README.md). To stop the web server, switch back to the window in which it's running and press CTRL+C - Control and C keys together (on Windows, you might have to press Ctrl+Break).

Ready for the next step? It's time to create some content!