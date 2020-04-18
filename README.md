# django-gunicorn-nginx-setup-doc
document and template files for publishing django app via nginx and gunicorn

# Packages :
get virtualenv

    sudo apt install python3-venv

get pip

    sudo apt install python3-pip
# Directory :
commands in next steps are written assuming your directory is your django
project directory.

# Virtual Env. :
create virtualenv in same directory with manage.py

    python3 -m venv venv

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

check pip and python

    which python

should be .../venv/bin/python

    python -V

should be version 3.X

    which pip

should be .../venv/bin/python

upgrade pip

    pip install --upgrade pip

check version

    pip -V

should be 20.X or later

get required python packages to virtual env.

    pip install -r requirements.txt

to test django project

    python manage.py makemigrations
    python manage.py migrate
    python manage.py collectstatic
    python manage.py runserver 0.0.0.0:8000

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

# Nginx :
get nginx

    sudo apt install nginx

configure your nginx for your socket, you can edit site-name file and copy it.

    cp [path of this directory]/site-name /etc/nginx/sites-available/[site-name]

link the nginx site file to enable path

    sudo ln -s /etc/nginx/sites-available/[site-name] /etc/nginx/sites-enable

test any syntax error on nginx site file

    sudo nginx -t

restart nginx

    sudo systemctl restart nginx

open your ports for nginx if required

    sudo ufw allow 'Nginx Full'

done...