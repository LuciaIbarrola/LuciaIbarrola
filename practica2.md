# Política de contraseñas

## INTRODUCCIÓN

La seguridad de nuestro sistema se puede ver afectada y amenazada por un incorrecto uso de las conraseñas. Para ello disponemos de las políticas de contraseñas. éstas refuerzan la seguridad del sistema.

El objetivo de la práctica es **entender y configurar las directrices de seguridad**. Para ello utilizaremos el módulo *pw_password* (PAM - Plugable Authntication Module).

## DESARROLLO

**PASO 1** - Comprobar instalación.

Debemos comprobar si tenemos instalados *libpam-pwquality* y *libpequality-tools*. De no tenerlos los instalamos:

```bash
$sudo apt install libpam-pwquality libpwquality-tools
```
Ahora tenemos el fchero **/etc/security/pwquality.conf**. En él estableceremos unos mínimos para la hora de configurar una contraseña.

**PASO 2** - Editamos el fichero pwquality.conf 

Esto es lo primero que vemos al entrar al fichero, todo está comentado.

```bash
# Configuration for systemwide password quality limits
# Defaults:
#
# Number of characters in the new password that must not be present in the
# old password.
# difok = 1
#
# Minimum acceptable size for the new password (plus one if
# credits are not disabled which is the default). (See pam_cracklib manual.)
# Cannot be set to lower value than 6.
# minlen = 8
#
#... Continúa ...

```
Vamos a trabajar entorno a la **pwscore**, puntuación de la contraseña. Los parámetros que establezcamos darán más o menos puntuación. Una contraseña más corta podría ser aceptable si es más compleja en otras formas.

**Ejemplo 1:**

Queremos que la contraseña como mínimo tenga 10 carácteres.

```bash
minlen = 10
```

Si la contraseña tiene algún numero, la puntuación será de 2.

```bash
dcredit = 2
```

Y por cada letra en mayúscula se le darán 3 créditos.

```bash
ucredit = 3
```

**Ejemplo 2:**

Ahora daremos más créditos por caráteres especiales, 

## COMPROBACIÓN

Para comprobar la puntuación de una contraseña debemos escribir el comando *pwscore* y la contraseña que queremos probar.

**Comprobación ejemplo 1:**

```bash
$echo "Mananajz" | pwscore
Password quality check failed:
 The password is shorter than 9 characters

$echo "mananadsjz" | pwscore
5

$echo "maNanadsjz" | pwscore
13

$echo "maNanadsj4z" | pwscore
34
```

Cada vez tenemos una puntuación más alta, lo que equivale a una contraseña más segura.