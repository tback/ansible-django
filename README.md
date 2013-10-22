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
- rename this to `inventory` 
- put all sensitive information in this folder
 
