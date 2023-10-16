# Logs Centralizados

## Introducción
El trabajo del administrador de sistemas puede verse simplificado cenralizando los logs. Para ello se pueden llevar todos los logs a un mismo server de diferentes clientes.

## Desarrollo
### Paso 1:
En el archivo /etc/rsyslog.conf debemos añadir la IP del servidor donde mandaremos los logs. 

```bash
#$ActionResumeRetryCount -1    # infinite retries if host is down
# remote host is: name/ip:port, e.g. 192.168.0.1:514, port optional
#*.* @@remote-host:514
*.* @@192.168.0.180:514
# ### end of the forwarding rule ###

```


### Paso 2:
En el servidor editamos el */etc/rsyslog.conf* y descomentamos estos apartados:

```bash
# Provides UDP syslog reception
$ModLoad imudp
$UDPServerRun 514
 
# Provides TCP syslog reception
$ModLoad imtcp
$InputTCPServerRun 514

```

## Paso 3:
En el servidor habilitamos el puerto 514 para el protocolo de red TCP.

```bash
$sudo ufw allow 514/tcp
```

## Paso 4:
Reiniciamos el servicio:

```bash
$sudo systemctl restart rsyslog
```

## Comprobación:

Desde el cliente lanzamos un mensaje mediante el comando logger el servidor.

```bash
$logger HOLA
```

Ahora en el servidor podemos observar en el fichero */var/log/syslog*.

```bash
cat /var/log/syslog
```

## Prueba
