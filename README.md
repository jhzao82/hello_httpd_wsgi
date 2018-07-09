# hello_httpd_wsgi

centos7+apache2.4+mod_wsgi+python3.6

1. install httpd mod_wsgi
```
yum -y install https://centos7.iuscommunity.org/ius-release.rpm
yum -y install httpd24u-2.4.33-3.ius.centos7 \
python36u-3.6.5-1.ius.centos7 \
python36u-mod_wsgi-4.6.2-1.ius.centos7 \
python36u-pip-9.0.1-1.ius.centos7
```

2. vi /etc/httpd/conf/httpd.conf
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
