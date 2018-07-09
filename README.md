"# hello_httpd_wsgi" 

1. yum install httpd python-setuptools mod_wsgi

2. vi /etc/httpd/conf/httpd.conf
<VirtualHost *:80>
   ServerName localhost
   ServerAlias localhost
   ServerAdmin username@example.com
   DocumentRoot /var/www/html
   ErrorLog /var/log/error.log
   CustomLog /var/log/access.log combined
   WSGIScriptAlias / /var/www/html/app.wsgi
</VirtualHost>

3. vi /var/www/html/app.wsgi
import sys
sys.path.append('/var/www/html')
def application(environ, start_response):
    status = '200 OK'
    output = 'Hello World!'
    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)
    return [output]