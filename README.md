# django-gunicorn-nginx-setup-doc
document and template files for publishing django app via nginx and gunicorn

# Packages :
get virtualenv

    sudo apt install python3-virtualenv

get pip

    sudo apt install python3-pip
# Directory :
commands in next steps are written assuming your directory is your django
project directory.

# Virtual Env. :
create virtualenv in same directory with manage.py

    python3 -m virtualenv venv

in some cases when creating virtualenv you get error, then try this

    export LC_ALL="en_US.UTF-8"
    export LC_CTYPE="en_US.UTF-8"

and run last command again to create virtualenv.

# Setting Django Project :
first of all, you must be sure your django project is ready to publish
you better double check DEBUG flag is False in settings.py
and the other things, maybe useful;
    python manage.py check --deploy

activate virtual env.

    source venv/bin/activate

get required python packages to virtual env.

    pip3 install -r requirements.txt

to test django project

    python3 manage.py makemigrations
    python3 manage.py migrate
    python3 manage.py collectstatic
    python3 manage.py runserver 0.0.0.0:8000

maybe you will need

    sudo ufw allow 8000

than you can check from browser another where.
if it works you can stop serving "Ctrl + C"

# Gunicorn :
be sure still using virtual env. and if you are not using, activate it.
get gunicorn

    pip3 install gunicorn

try it

    gunicorn --bind 0.0.0.0:8000 [your-django-project-name].wsgi

if it works than you can create a service that keeps your gunicorn service
active. you can edit gunicorn.service.temp file and than copy it.

    cp [path of this directory]/gunicorn.service.template /etc/systemd/system/gunicorn.service

activate it

    sudo systemctl start gunicorn
    sudo systemctl enable gunicorn

be sure it works

    sudo systemctl status gunicorn
