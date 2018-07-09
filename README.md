# hello_httpd_wsgi

centos+apache+mod_wsgi+python

1. yum install httpd mod_wsgi

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
