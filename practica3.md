# ACL - Access Control List
A la hora de dar permisos a usuarios y grupos a veces queremos dar diferentes permisos a grupos sobre un directorio/ichero. De la manera convencional no podemos, pero con ACL si.

En esta practica utilizaremos el comando **setfacl** para dar permisos a usuarios y grupos en diferentes carpetas.

## ESCENARIO
Tenemos las siguientes carpetas y ficheros:
```bash
.
└── COMPARTIDO_DE_GRUPOS
    ├── ESO1
    │   ├── a1.txt
    │   ├── a2.txt
    │   └── a3.txt
    ├── ESO2
    │   ├── b1.txt
    │   ├── b2.txt
    │   └── b3.txt
    ├── STUDENTS
    │   ├── d1.txt
    │   ├── d2.txt
    │   └── d3.txt
    └── TEACHERS
        ├── c1.txt
        ├── c2.txt
        └── c3.txt
```

Tendremos cuatro **grupos**:

- ESO1
- ESO2
- TEACHERS
- STUDENTS

Tendremos los siguientes **usuarios**:

- ST1 - Perteneciente a los grupos STUDENTS y ESO1.
- ST2 - Perteneciente a los grupos STUDENTS y ESO2.
- TE1 - Perteneciente al grupo TEACHERS.
- TE2 - Perteneciente al grupo TEACHERS.

Pautas:

En las carpetas ESO1 y ESO2 los profesores podrán leer y escribir pero los alumnos de dichos curso solo podrán leer el contenido. 

En la carpeta de los profesores solo el profesor TE1 podrá escribir y leer, los demás profesores solo podrán leer.

Por último, en la carpeta de los estudiantes los profesores podrán leer y escibir pero los estudiantes solo leerán.

## PASOS

### PASO 1 - COMPROBACIÓN
Debemos comprobar si tenemos instalado el ACL.

```bash
$tune2fs -l /dev/sda3 | grep acl

Default mount options:    user_xattr acl
```

### PASO 2 - PERMISOS

Una vez tengamos los usuarios en los respectivos grupos, pasaremos a dar permisos en los directorios.

Para ello utilizaremos el comando **setfacl** el cual nos permitira crear una lista de control de acceso en los directorios y ficheros.

```bash
# En la carpeta ESO1 y ESO2 los profesores podrán leer y escribit mientras que solo los alumnos de ese curso podrán leer.

$setfacl -Rdm g:TEACHERS:rwx, g:ESO1:rx ./ESO1

$setfacl -Rdm g:TEACHERS:rwx, g:ESO2:rx ./ESO2
```

```bash
# En la carpeta TEACHERS los profesores podrán leer mientras que el profe 1 podrá leer y escribir.

$setfacl -Rdm g:TEACHERS:rx, u:T1:rwx ./TEACHERS
```

```bash
# En la  carpeta STUDENTS los estudiantes leerán mientras que los profesores podrán leer y escribir.

$setfacl -Rdm g:TEACHERS:rwx, g:STUDENTS:rx ./STUDENTS
```

Para ver si hemos asignado correctamente los permisos en los directorios podemos utilizar el siguiente comando:

```bash
$getfacl DIRECTORIO 
```

### PASO 3 - COMPROBACIONES

```bash
# S1 no puede ver dentro del directorio ESO2.
```

```bash
# Si creo una carpeta en ESO1 llamada MATH, los alumnos de ESO1 pueden acceder a dicha carpeta.
```

```bash
# El profesor T2 no puede editar dentro de la carpeta de los profesores.
```