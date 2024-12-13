# Guía detallada de permisos en Linux

En Linux, cada archivo y directorio tiene un conjunto de permisos que controlan quién puede leer, escribir o ejecutar el contenido. Estos permisos se asignan a tres entidades: el propietario, el grupo y otros usuarios. Esta guía detalla cómo funcionan los permisos, cómo cambiarlos y cómo interpretarlos.

## Estructura de los permisos

Los permisos de un archivo se representan en forma de 10 caracteres alfanuméricos, como en el siguiente ejemplo:

```
drwxr-xr--
```

### Desglose de los caracteres
1. **Primer carácter**: Indica el tipo de archivo:
   - `-` : Archivo regular.
   - `d` : Directorio.
   - `l` : Enlace simbólico.
   - `b` : Archivo de dispositivo de bloque.
   - `c` : Archivo de dispositivo de caracteres.

2. **Siguientes 9 caracteres**: Representan los permisos divididos en tres grupos de tres:
   - **Propietario**: `rwx`
   - **Grupo**: `r-x`
   - **Otros**: `r--`

### Significado de cada carácter
- `r` (read): Permiso de lectura.
- `w` (write): Permiso de escritura.
- `x` (execute): Permiso de ejecución.
- `-`: Sin permiso.

Por ejemplo:
- `rwx`: Leer, escribir y ejecutar.
- `rw-`: Leer y escribir, sin ejecutar.
- `r--`: Solo lectura.

## Propietario, Grupo y Otros

- **Propietario**: Es el usuario que creó el archivo. Puede cambiar los permisos.
- **Grupo**: Un conjunto de usuarios que pueden compartir permisos sobre el archivo.
- **Otros**: Todos los demás usuarios.

## Modificación de permisos

Los permisos pueden modificarse utilizando el comando `chmod`. Este comando permite asignar permisos tanto en formato simbólico como octal.

### Uso de chmod con formato simbólico
```bash
chmod [entidad][operador][permiso] archivo
```
- **Entidad**:
  - `u`: Propietario.
  - `g`: Grupo.
  - `o`: Otros.
  - `a`: Todos (propietario, grupo y otros).
- **Operador**:
  - `+`: Agregar permiso.
  - `-`: Quitar permiso.
  - `=`: Asignar permiso.
- **Permiso**:
  - `r`: Lectura.
  - `w`: Escritura.
  - `x`: Ejecución.

Ejemplo:
```bash
chmod u+rwx archivo.txt  # Da permisos completos al propietario
chmod g-w archivo.txt   # Quita permisos de escritura al grupo
chmod o= archivo.txt    # Quita todos los permisos a otros
```

### Uso de chmod con formato octal

El formato octal utiliza números para representar combinaciones de permisos:
- `4`: Lectura (`r`)
- `2`: Escritura (`w`)
- `1`: Ejecución (`x`)
- `0`: Sin permisos

Ejemplo:
- `7` (4+2+1): Leer, escribir y ejecutar.
- `6` (4+2): Leer y escribir.
- `5` (4+1): Leer y ejecutar.

Para establecer permisos:
```bash
chmod 764 archivo.txt
```
Esto asigna:
- `7` (rwx): Propietario.
- `6` (rw-): Grupo.
- `4` (r--): Otros.

## Cambio de propietario y grupo

El propietario y grupo de un archivo o directorio pueden cambiarse con los comandos `chown` y `chgrp`.

### Cambiar el propietario
```bash
chown nuevo_propietario archivo
```
Ejemplo:
```bash
chown maria archivo.txt
```

### Cambiar el grupo
```bash
chgrp nuevo_grupo archivo
```
Ejemplo:
```bash
chgrp desarrolladores archivo.txt
```

### Cambiar propietario y grupo simultáneamente
```bash
chown nuevo_propietario:nuevo_grupo archivo
```
Ejemplo:
```bash
chown maria:desarrolladores archivo.txt
```

### Uso de opciones recursivas
Para cambiar permisos, propietario o grupo en un directorio y su contenido, use la opción `-R`:
```bash
chmod -R 755 directorio
chown -R maria:desarrolladores directorio
```

## Visualización de permisos

Para ver los permisos de un archivo o directorio, use `ls` con la opción `-l`:
```bash
ls -l archivo.txt
```
Esto muestra:
```
-rw-r--r-- 1 maria desarrolladores 1024 dic 13 10:00 archivo.txt
```
- `-rw-r--r--`: Permisos.
- `1`: Número de enlaces.
- `maria`: Propietario.
- `desarrolladores`: Grupo.
- `1024`: Tamaño en bytes.
- `dic 13 10:00`: Fecha de última modificación.

## Notas adicionales

- **Umask**: Determina los permisos por defecto para nuevos archivos.
  - Para ver la umask actual:
    ```bash
    umask
    ```
  - Para establecer una umask específica:
    ```bash
    umask 022
    ```
- **SUID, SGID y Sticky Bit**: Permisos especiales para archivos y directorios. Se configuran con `chmod` usando dígitos adicionales (e.g., `chmod 4755 archivo`).

Con esta guía, podrás gestionar permisos en Linux de manera eficiente y segura.
