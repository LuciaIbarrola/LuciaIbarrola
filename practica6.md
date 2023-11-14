# ENDURECIMIENTO DE APACHE

## Hardening 1 - Esconder versión de Apache y la información del sistema operativo.

Punto de partida:
![Alt text](image-6.png)

Vamos al /etc/httpd/conf/httpd.conf y añadios al final:
![Alt text](image-5.png)

```bash
$systemctl restart apache2
```

Quedaría:
![Alt text](image-7.png)

## Hardening 2 - Desactivar listado de directorios de Apache.

```bash
# Para probarlo necesitamos crear:
$ mkdir -p /var/www/html/test
$ cd /var/www/html/test
$ sudo touch app.py main.py
```

Vemos lo siguiente en el navegador:
![Alt text](image-8.png)

Ahora accedemos al archivo: /etc/apache2/apache2.conf y añadimos esto al final del archivo:

![Alt text](image-9.png)

Y nos dará este resultado:

![Alt text](image-10.png)

## Hardening 4 - Usar HTTPS.

lucia@lucia-VirtualBox:~/practice-csr$ sudo cp carlu-server.key /etc/ssl/private/

Por último en el ficjero /etc/apache2/sites-available/default-ssl.conf editamos estas dos líneas:

- SSLCertificateFile      /etc/ssl/certs/carlu-server.crt

- SSLCertificateKeyFile /etc/ssl/private/carlu-server.key

Ahora tenemos que activar eñ sitio apache con el comando:

```bash
sudo a2ensite default-ssl
systemctl reload apache2
```
```bash 
sudo a2enmod ssl
systemctl restart apache2
```

## Hardening 5 - Enable HTTP Strict Transport Security (HSTS) for Apache

![Alt text](image-12.png)


## Hardening 6 - Enable HTTP/2 on Apache

```bash
$ apache2 -v
$ sudo a2enmod http2
```

Añadir línea Protocols:
![Alt text](image-13.png)

```bash
 $ sudo systemctl restart apache2
```

Comprobar:

```bash
$ curl -I --http2 -s https://domain.com/ | grep HTTP
```

## Hardening 8 - Disable the ServerSignature Directive in Apache

Comentar:

![Alt text](image-14.png)

```bash
$ sudo systemctl restart apache2
```

## Hardening 9 - Set the ‘ServerTokens’ Directive to ‘Prod’

En apache2.conf

![Alt text](image-16.png)

## Hardening 11 - Disable Unnecessary Modules

Ver:

```bash
$ apache2ctl -M
```

## Hardening 14 - Limit File Upload Size in Apache.

Para limitar añadimos la línea en el archivo de configuración de Apache:

![Alt text](image-17.png)

## Hardening 16 - Run Apache as a Separate User and Group.

```bash
$ sudo groupadd apachegroup
$ sudo useradd -g apachegroup apacheuser
```

Ponemos los usuarios en el archivo de configuración de apache:

![Alt text](image-18.png)

```bash
$ sudo chown -R apacheuser:apachegroup /var/www/html
```

Restart Apache.

## Hardening 17 - Protect DDOS Attacks and Hardening

![Alt text](image-19.png)

![Alt text](image-20.png)

![Alt text](image-21.png)

![Alt text](image-22.png)