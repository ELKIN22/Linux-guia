
# Guía Completa sobre CRON Jobs en Linux

Los **CRON jobs** en Linux son tareas programadas que se ejecutan automáticamente en intervalos regulares. Son esenciales para la automatización de tareas como respaldos, limpieza de logs y mantenimiento de sistemas.

---

## ¿Qué es CRON?

- **CRON** es un servicio de Unix/Linux para programar tareas automatizadas.
- Administrado por el demonio `cron`, que ejecuta tareas según el cronograma.
- Se utiliza principalmente para ejecutar comandos o scripts en horarios específicos.

---

## Sintaxis de CRON

Una línea en un archivo CRON tiene la siguiente estructura:

\`\`\`plaintext
* * * * * comando
| | | | |
| | | | +--- Día de la semana (0 - 7, donde 0 y 7 son domingo)
| | | +----- Mes (1 - 12)
| | +------- Día del mes (1 - 31)
| +--------- Hora (0 - 23)
+----------- Minuto (0 - 59)
\`\`\`

### Ejemplo:
\`\`\`plaintext
30 2 * * * /usr/bin/php /ruta/a/mi/script.php
\`\`\`
Este cron job ejecuta el comando `/usr/bin/php /ruta/a/mi/script.php` todos los días a las 2:30 AM.

### Comodines comunes:
- `*`: Cada unidad de tiempo (por ejemplo, cada minuto, cada hora).
- `,`: Separador para valores específicos (por ejemplo, `1,15` significa el día 1 y el día 15).
- `-`: Rango de valores (por ejemplo, `1-5` significa de lunes a viernes).
- `/`: Incrementos (por ejemplo, `*/5` significa cada 5 minutos).

---

## Configuración de CRON Jobs

### 1. Crontab del Usuario

Cada usuario puede configurar sus tareas con:

\`\`\`bash
crontab -e
\`\`\`

- Los cron jobs se almacenan en `/var/spool/cron/crontabs/<usuario>`.
- Se ejecutan con los permisos del usuario propietario del crontab.

#### Comandos útiles:
- **Listar tareas del usuario actual:**
  \`\`\`bash
  crontab -l
  \`\`\`
- **Editar tareas del usuario actual:**
  \`\`\`bash
  crontab -e
  \`\`\`
- **Eliminar todas las tareas del usuario:**
  \`\`\`bash
  crontab -r
  \`\`\`

#### Ejemplo:
\`\`\`plaintext
0 3 * * * /usr/bin/backup.sh
\`\`\`
Ejecuta el script `backup.sh` todos los días a las 3:00 AM.

---

### 2. Archivo Global (\`/etc/crontab\`)

El archivo global controla cron jobs del sistema y de todos los usuarios. Es necesario especificar el usuario que ejecutará cada tarea.

- **Ubicación:** `/etc/crontab`

#### Formato:
\`\`\`plaintext
* * * * * usuario comando
\`\`\`

#### Ejemplo:
\`\`\`plaintext
0 2 * * * root /usr/bin/system-update
\`\`\`
Este cron job ejecuta una actualización del sistema diariamente a las 2:00 AM como usuario \`root\`.

---

### 3. Directorios Predefinidos (\`/etc/cron.*\`)

Linux incluye directorios donde puedes colocar scripts que se ejecutarán automáticamente:

- `/etc/cron.hourly/`: Cada hora.
- `/etc/cron.daily/`: Diario.
- `/etc/cron.weekly/`: Semanal.
- `/etc/cron.monthly/`: Mensual.

#### Ejemplo:
Colocar un script en `/etc/cron.daily/limpieza-logs` lo ejecutará una vez al día.

---

### 4. Archivos Específicos de Usuario en `/var/spool/cron/crontabs/`

Archivos que contienen cron jobs configurados para usuarios específicos. Son administrados mediante \`crontab -e\` y no deben editarse directamente.

#### Ejemplo:
El archivo `/var/spool/cron/crontabs/siacolweb` podría contener:
\`\`\`plaintext
0 2 * * * php /home/siacolweb/web/mi-sitio/artisan check:diskspace
\`\`\`
Este cron verifica el espacio en disco a las 2:00 AM con los permisos del usuario \`siacolweb\`.

---

## Casos de Uso Comunes

1. **Respaldo de bases de datos:**
   \`\`\`bash
   0 3 * * * /usr/bin/mysqldump -u user -p'password' dbname > /backup/db-$(date +\%F).sql
   \`\`\`

2. **Limpieza de logs antiguos:**
   \`\`\`bash
   0 0 * * 0 find /var/log -type f -mtime +30 -exec rm {} \;
   \`\`\`

3. **Actualizar paquetes del sistema:**
   \`\`\`bash
   0 4 * * * apt-get update && apt-get upgrade -y
   \`\`\`

4. **Ejecutar un comando de Laravel:**
   \`\`\`bash
   30 1 * * * php /var/www/mi-proyecto/artisan schedule:run
   \`\`\`

---

## Manejo de la Salida y Logs

- **Redirigir salida estándar y errores a un archivo:**
  \`\`\`bash
  * * * * * comando >> /ruta/a/log.txt 2>&1
  \`\`\`

- **Ver logs del servicio CRON:**
  \`\`\`bash
  sudo tail -f /var/log/syslog
  \`\`\`

---

## Variables de Entorno

Los cron jobs heredan un entorno mínimo. Para evitar problemas, es útil definir variables como \`PATH\` al inicio del crontab:

\`\`\`plaintext
PATH=/usr/local/bin:/usr/bin:/bin
\`\`\`

---

## Buenas Prácticas

1. **Evitar conflictos:** Asegúrate de que no haya dos cron jobs accediendo al mismo recurso simultáneamente.
2. **Probar antes de programar:** Ejecuta manualmente los comandos.
3. **Usar scripts:** Coloca tareas complejas en scripts separados.
4. **Monitorizar logs:** Revisa regularmente los logs para identificar problemas.

---

## Ejemplo Completo de un Archivo Crontab

\`\`\`plaintext
# Variables de entorno
MAILTO=admin@example.com
PATH=/usr/local/bin:/usr/bin:/bin

# Tarea 1: Respaldo diario a las 3:00 AM
0 3 * * * /usr/bin/mysqldump -u user -p'password' dbname > /backup/db-$(date +\%F).sql

# Tarea 2: Limpieza semanal de logs los domingos a medianoche
0 0 * * 0 find /var/log -type f -mtime +30 -exec rm {} \;

# Tarea 3: Actualizar el sistema todos los días a las 4:00 AM
0 4 * * * apt-get update && apt-get upgrade -y
\`\`\`

---

## Conclusión

Los CRON jobs son una herramienta poderosa y flexible para la automatización de tareas en Linux. Entender las ubicaciones clave, la sintaxis y las prácticas recomendadas te permitirá utilizarlos de manera eficiente para optimizar tu sistema.
