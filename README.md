# hello_httpd_wsgi

centos7+apache2.4+mod_wsgi4.6+python3.6

1. install httpd mod_wsgi
```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm
yum -y install httpd24u.x86_64 0:2.4.33-3.ius.centos7 \
httpd24u-mod_ssl-2.4.33-3.ius.centos7 \
httpd24u-tools.x86_64 0:2.4.33-3.ius.centos7 \
python36u-3.6.5-1.ius.centos7 \
python36u-devel-3.6.5-1.ius.centos7 \
python36u-libs-3.6.5-1.ius.centos7 \
python36u-mod_wsgi-4.6.2-1.ius.centos7 \
python36u-pip-9.0.1-1.ius.centos7
```

2. vi /etc/httpd/conf.d/hello.conf
```
<VirtualHost *:80>
   ServerName localhost
   ServerAlias localhost
   DocumentRoot /var/www/html
   ErrorLog /var/log/app.error.log
   CustomLog /var/log/app.access.log combined
   WSGIScriptAlias / /var/www/html/app.wsgi
</VirtualHost>
```

3. vi /var/www/html/app.wsgi
```
import sys
sys.path.append('/var/www/html')
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]
```

4. service httpd start

5. django install
```
ln -fs /usr/bin/python3.6 /usr/bin/python3
ln -fs /usr/bin/pip3.6 /usr/bin/pip3

pip3 install --upgrade pip
pip3 install virtualenv
mkdir /var/www/django
cd /var/www/django
virtualenv env
source env/bin/activate
pip3 install django
django-admin startproject hello
```

5. vi /etc/httpd/conf.d/hello.conf
```
<VirtualHost *:80>
   ServerName localhost
   ServerAlias localhost
   DocumentRoot /var/www/html
   ErrorLog /var/log/app.error.log
   CustomLog /var/log/app.access.log combined
   WSGIDaemonProcess django python-path=/var/www/django/hello:/var/www/django/env/lib/python3.6/site-packages
   WSGIProcessGroup django
   WSGIScriptAlias / /var/www/django/hello/hello/wsgi.py
</VirtualHost>
```

6. service httpd restart
