# Practica-_9-

## Preparación del Entorno:
### Configuramos un archivo docker-compose.yaml para definir dos servicios:

Apache con PHP para alojar dos sitios web.
Bind9 como servidor DNS para resolver los dominios.

### Configuración del DNS (Bind9):

Creamos archivos de configuración para que el DNS resuelva:
40k.asircastelao.int y celta.asircastelao.int → IP del Apache (172.29.4.2).Montamos estas configuraciones en el contenedor Bind9 usando volúmenes.

### Configuración del Servidor Web (Apache):

Definimos dos Virtual Hosts:
Uno para cada dominio, apuntando a directorios separados dentro del servidor web.
Usamos la directiva DirectoryIndex para establecer la página principal por defecto.

### Creación de Red y Asignación de IP Fijas:

Creamos una red personalizada apache_subnet con un rango definido.
Asignamos IP fija a cada contenedor:
Apache: 172.29.4.2
Bind9: 172.29.4.3

### Modificación de /etc/systemd/resolved.conf

Para que nuestro sistema utilice el servidor DNS configurado en Docker (Bind9), fue necesario modificar el archivo de configuración de systemd-resolved:
- Abrimos el archivo con permisos de administrador: `sudo nano /etc/systemd/resolved.conf`
- Modificamos la línea para apuntar al servidor DNS de Docker con el puerto 54: `DNS=172.29.4.3#54`
- Guardamos los cambios y reiniciamos el servicio para aplicar la configuración: `sudo systemctl restart systemd-resolved`
*Esta configuración asegura que todas las consultas DNS de tu sistema se resuelvan a través del Bind9 del contenedor, permitiendo resolver los dominios configurados*