# Comandos en Linux

Este documento describe los diferentes tipos de comandos, parámetros, argumentos y herramientas de ayuda disponibles en Linux.

## Tipos de comandos
En Linux, los comandos se dividen en tres tipos:

1. **Internos (built-in)**: Son comandos que forman parte del shell y no requieren un programa externo para ejecutarse.
2. **Externos**: Son comandos que requieren un programa o binario externo para ejecutarse.
3. **Script shell**: Son comandos personalizados escritos en scripts (archivos con código shell).

---

## Parámetros y argumentos en comandos
Los comandos en Linux pueden aceptar parámetros y argumentos para modificar su comportamiento.

### Ejemplos:
- `ls --color=auto`: Activa los colores en la salida del comando `ls`.
- `ls -l`: Muestra más información detallada sobre los archivos.
- `ls -la`: Muestra también los archivos ocultos.

---

## Herramientas de ayuda en Linux
Para obtener información sobre cómo usar un comando o entender su propósito, puedes usar las siguientes herramientas:

### `help`
Sirve para mostrar información sobre un comando interno del shell.

Ejemplo:
```bash
help "comando"
```

### `--help`
Muestra una breve descripción y las opciones disponibles de un comando.

Ejemplo:
```bash
ls --help
```

### `man`
Muestra el manual detallado de un comando.

Ejemplo:
```bash
man "comando"
```

### `whatis`
Muestra una breve descripción del propósito de un comando.

Ejemplo:
```bash
whatis "comando"
```

### `info`
Proporciona información más detallada sobre un comando.

Ejemplo:
```bash
info "comando"
```

---

## Comandos adicionales y atajos

### Comandos ejecutados simultáneamente
Puedes ejecutar varios comandos en una misma línea separándolos con punto y coma (`;`).

Ejemplo:
```bash
comando1 ; comando2
```

### Atajos
- **Ctrl + R**: Busca comandos en el historial de comandos.

### Creación de alias
Puedes crear alias para comandos utilizando el siguiente formato:
```bash
alias nombre="comando"
```
Esto permite ejecutar un comando personalizado al escribir `nombre`.

---

## Sistema de ficheros

### Archivos ocultos
Los archivos ocultos son aquellos cuyo nombre comienza con un punto (`.`), como `.config`. Para listarlos, puedes usar:
```bash
ls -a
```

### Crear directorios
Para crear un directorio, utiliza el comando `mkdir`:
```bash
mkdir "nombre_carpeta"
```

### Sensibilidad a mayúsculas
En Linux, los nombres de ficheros y directorios distinguen entre mayúsculas y minúsculas. Por ejemplo, `Archivo` y `archivo` son diferentes.

---

## Editores de texto
- **Pico**: Editor de texto sencillo.
- **Nano**: Otro editor de texto popular y fácil de usar.

---

## Comandos para gestionar ficheros

### `file`
Determina el tipo de un fichero.

Ejemplo:
```bash
file "fichero"
```

### `cat`
Muestra el contenido de un fichero.

Ejemplo:
```bash
cat "fichero"
```

### `more`
Muestra el contenido de un fichero de forma paginada.

Ejemplo:
```bash
more "fichero"
```

---

## Directorios principales en Linux
Los directorios principales en Linux tienen funciones específicas y organizan los archivos del sistema.

### Principales directorios:
- `/`: Raíz del sistema de archivos. Contiene todos los archivos y directorios.
- `/home`: Contiene los directorios personales de los usuarios.
  - Ejemplo: `/home/usuario`
- `/etc`: Archivos de configuración del sistema y de programas.
  - Ejemplo: `/etc/passwd` almacena información de usuarios.
- `/var`: Almacena datos variables, como logs y archivos temporales.
  - Ejemplo: `/var/log/syslog` para registros del sistema.
- `/tmp`: Archivos temporales utilizados por aplicaciones y usuarios.
  - Ejemplo: Archivos creados durante ejecuciones breves.
- `/bin` y `/sbin`: Contienen comandos básicos para usuarios (`bin`) y administradores (`sbin`).
  - Ejemplo: `/bin/ls`.

### ¿Cuándo manipularlos?
- Se manipulan al configurar el sistema, instalar software o gestionar archivos personales.

---

## Inodos, soft links y hard links

### Inodos
Un inodo es una estructura de datos que almacena información sobre un archivo o directorio, como permisos, tamaño y ubicación en disco.

Ejemplo para ver inodos:
```bash
ls -i "archivo"
```

### Hard links
Un hard link es un enlace directo a un inodo. Si eliminas el archivo original, el hard link seguirá funcionando.

Ejemplo:
```bash
ln archivo_origen hard_link
```

### Soft links (enlaces simbólicos)
Un soft link es un apuntador a un archivo o directorio, similar a un acceso directo. Si se elimina el archivo original, el enlace simbólico se rompe.

Ejemplo:
```bash
ln -s archivo_origen soft_link
```

---

## Wildcards (comodines)
Los comodines se usan para realizar operaciones con patrones en nombres de archivos.

### Principales comodines:
- `*`: Representa cualquier cantidad de caracteres.
  - Ejemplo: `ls *.txt` lista todos los archivos con extensión `.txt`.
- `?`: Representa un solo carácter.
  - Ejemplo: `ls archivo?.txt` busca `archivo1.txt`, `archivoA.txt`, etc.
- `[ ]`: Representa un rango de caracteres.
  - Ejemplo: `ls archivo[1-3].txt` busca `archivo1.txt`, `archivo2.txt`, `archivo3.txt`.

### Ejemplo de uso:
Copiar todos los archivos que comiencen con "doc":
```bash
cp doc* /destino
```

