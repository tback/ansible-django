# Ansible Django Deployment playbook.

This playbook works with (or will aim to work with) the following technologies:

* Django
* Git
* uWsgi
* Nginx
* PostgreSQL
* Celery
* Haystack
* Redis

I have provided a simple shortcut command `play`

```
./play production all
./play production webservers
./play production dbservers
./play production webservers --tags=pip
```

An example inventory is included, you should :
* rename this to `inventory`.
* move it to your django project source tree (see below).
* put all sensitive information in this folder.


## Django Project Layout

In your django project, you should organise your settings module like so :
* git_root
  * deployment/
    * inventory
  * requirements/
    * base.list
    * live.list
    * test.list
    * local.list
  * application/
    * __init__.py
    * manage.py
    * base/
      * __init__.py
      * wsgi.py
      * settings/
        * modules/
          * __init__.py
        * __init__.py
        * default.py
        * local.py
        * live.py


### project_root/manage.py

```
#!/usr/bin/env python
#
# file: project_root/manage.py
#
import os
import sys

if __name__ == "__main__":
  os.environ.setdefault("DJANGO_SETTINGS_MODULE", "base.settings.local")
  from django.core.management import execute_from_command_line
  execute_from_command_line(sys.argv)
```

### project_root/base/wsgi.py
```
"""
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "base.settings.live")
from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()

```

### project_root/base/settings/live.py

```
from .default import *
from .modules.db import *

DEBUG = False
TEMPLATE_DEBUG = DEBUG

...

```

### project_root/base/settings/local.py

```
from .default import *


DEBUG = True
TEMPLATE_DEBUG = DEBUG

DATABASES = {
  'default': {
    'ENGINE': 'django.db.backends.sqlite3',
    'NAME': 'db.sqlite3',
  }
}
...

```