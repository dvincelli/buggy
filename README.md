# buggy
A minimal django app to repro a bug  involving django-cacheops and django-tracking2

```
virtualenv env
source env/bin/activate
pip install -r requirements.txt
python manage.py runserver 0.0.0.0:5000
```

visit http://127.0.0.1:5000/ once and refresh. You will receive a stack trace (also available at http://dpaste.com/1M9N1JZ )

```
Environment:


Request Method: GET
Request URL: http://192.168.100.10:5000/

Django Version: 1.7.4
Python Version: 2.7.8
Installed Applications:
('cacheops',
 'django.contrib.admin',
 'django.contrib.auth',
 'django.contrib.contenttypes',
 'django.contrib.sessions',
 'django.contrib.messages',
 'django.contrib.staticfiles',
 'main',
 'tracking')
Installed Middleware:
('django.contrib.sessions.middleware.SessionMiddleware',
 'django.middleware.common.CommonMiddleware',
 'django.middleware.csrf.CsrfViewMiddleware',
 'django.contrib.auth.middleware.AuthenticationMiddleware',
 'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
 'tracking.middleware.VisitorTrackingMiddleware',
 'django.contrib.messages.middleware.MessageMiddleware',
 'django.middleware.clickjacking.XFrameOptionsMiddleware')


Traceback:
File "/data/virtualenv/buggy/lib/python2.7/site-packages/django/core/handlers/base.py" in get_response
  204.                 response = middleware_method(request, response)
File "/data/virtualenv/buggy/lib/python2.7/site-packages/tracking/middleware.py" in process_response
  131.         visitor = self._refresh_visitor(user, request, now)
File "/data/virtualenv/buggy/lib/python2.7/site-packages/tracking/middleware.py" in _refresh_visitor
  60.             visitor = Visitor.objects.get(pk=session_key)
File "/data/virtualenv/buggy/lib/python2.7/site-packages/cacheops/query.py" in get
  398.         return self.get_queryset().inplace().get(*args, **kwargs)
File "/data/virtualenv/buggy/lib/python2.7/site-packages/cacheops/query.py" in get
  295.         return qs._no_monkey.get(qs, *args, **kwargs)
File "/data/virtualenv/buggy/lib/python2.7/site-packages/django/db/models/query.py" in get
  353.             return clone._result_cache[0]

Exception Type: AttributeError at /
Exception Value: 'list' object has no attribute '_result_cache'
```
