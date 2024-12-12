
# Redirecciones en Linux: `stdout`, `stderr` y `/dev/null`

En Linux, las redirecciones son una forma de controlar cómo se maneja la salida estándar (stdout) y los errores estándar (stderr) de un comando.

## Tipos de flujo estándar
1. **`stdout` (Salida estándar)**  
   Es el flujo donde los comandos envían su salida normal.  
   Descriptor: `1`.

2. **`stderr` (Error estándar)**  
   Es el flujo donde los comandos envían mensajes de error.  
   Descriptor: `2`.

3. **`/dev/null`**  
   Es un "pozo negro" donde puedes redirigir cualquier salida que desees descartar. Lo que se envía allí se elimina automáticamente.

---

## Sintaxis básica de redirecciones
- `>`: Redirige la salida a un archivo, sobrescribiendo el contenido del archivo existente.
- `>>`: Redirige la salida a un archivo, agregando al contenido existente.
- `2>`: Redirige los errores a un archivo.
- `&>`: Redirige tanto stdout como stderr a un archivo.

---

## Ejemplos prácticos

### 1. Redirigir la salida estándar (`stdout`)
```bash
ls > salida.txt
```
- **Descripción**: Redirige la lista de archivos del comando `ls` al archivo `salida.txt`.  
- Si el archivo `salida.txt` no existe, se crea. Si ya existe, su contenido se sobrescribe.

---

### 2. Redirigir los errores estándar (`stderr`)
```bash
ls no_existe 2> errores.txt
```
- **Descripción**: Intenta listar un archivo inexistente y redirige el mensaje de error al archivo `errores.txt`.

---

### 3. Redirigir stdout y stderr al mismo archivo
```bash
ls > salida_errores.txt 2>&1
```
- **Descripción**: Combina la salida estándar y los errores estándar en un único archivo.

---

### 4. Descartar la salida o los errores con `/dev/null`
- **Eliminar stdout**:
```bash
comando > /dev/null
```
- **Eliminar stderr**:
```bash
comando 2> /dev/null
```
- **Eliminar ambos**:
```bash
comando > /dev/null 2>&1
```
- **Caso de uso**: Utilizado cuando no necesitas ver la salida ni los errores, por ejemplo, para comandos en segundo plano.

---

### 5. Agregar contenido a un archivo en lugar de sobrescribir
```bash
echo "Nuevo log" >> log.txt
```
- **Descripción**: Agrega la línea "Nuevo log" al archivo `log.txt`.

---

### 6. Ejecutar un comando solo si no hay errores
```bash
comando && echo "Éxito"
```
- **Descripción**: Muestra "Éxito" solo si el comando no produce errores (es decir, devuelve un código de salida `0`).

### 7. Ejecutar un comando en caso de errores
```bash
comando || echo "Error"
```
- **Descripción**: Muestra "Error" si el comando falla (es decir, devuelve un código de salida distinto de `0`).

---

## Casos de uso comunes

### 1. Guardar logs  
Ejecutar un script y guardar su salida para análisis posterior:  
```bash
./script.sh > salida.log 2> errores.log
```

### 2. Automatización en cron jobs  
Evitar que cron genere demasiadas salidas:  
```bash
./backup.sh > /dev/null 2>&1
```

### 3. Debugging  
Separar errores de la salida estándar para identificar problemas:  
```bash
comando > salida.log 2> errores.log
```

### 4. Eliminar mensajes no deseados  
Ejecutar un comando ignorando los errores:  
```bash
comando 2> /dev/null
```
