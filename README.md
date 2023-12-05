# **LAMP en tres niveles**
#### Marcos de la Marta Núñez

<br />
<br />

# Índice.
1. [Introducción](#introducción)
2. [VPC](#vpc)
3. [Instancias](#instancias)
   * [Servidores Apache](#servidores-apache)
   * [MySQL](#mysql)
   * [Balanceador](#balanceador)


# Introducción
Para establecer la infraestructura, requeriremos la implementación de cuatro máquinas. Una de ellas se designará como el balanceador de carga, mientras que las dos restantes fungirán como servidores web. La última máquina desempeñará el rol de nuestra base de datos. La configuración del balanceador se realizará en la subred pública, mientras que tanto los servidores como la base de datos se desplegarán en la subred privada.
<br />

En mi caso, el direccionamiento va a ser el siguiente:
- Subred-Privada (Servidores Apache y servidor base de datos): **192.168.1.128/25** 
- Subred-Public (Balanceador): **192.168.1.0/25**
# VPC
## Balanceador
### En primer lugar, creamos la VPC (Virtual Private Cloud).
![](img/vpc.png)

### Después, dentro de esa VPC que hemos creado, creamos dos subredes. Una subred que va a tener salida a internet, en la que estará el balanceador, y otra subred privada para nuestros servidores de Backend.
![](img/subred1.png)

![](img/subred2.png)
<br />
<br />

# Instancias


### Una vez creadas las dos subredes, procedemos a crear las instancias. <br />
### Primero, crearemos la instancia balanceador.
![](img/balanceador.png)

### Ahora creamos las instancias para los servidores Apache
![](img/apache.png)

![](img/apache.png)

### Por último, creamos las instancias del servidor MYSQL.
![](img/DDBB.png)

### Para que nuestra máquina balanceador tenga salida a internet, le debemos asignar una IP elástica a la instancia.

### Primero creamos una puerta de enlace de internet y la asociamos a la VPC.
![](img/puerta.png)

### Ahora, asociamos la IP elástica a la instancia del balanceador. 
![](img/puertainternet.png)
<br />
<br />

## Configuración balanceador

### Primero, debemos editar la tabla de enrutamiento de la VPC y le añadimos una nueva regla para permitir la conexión a la puerta de enlace de internet.
![](img/segur.png)

### Primero, instalamos Apache y activamos los módulos proxy_balancer.
### Después, editamos el archivo balanceador.conf que está en el directorio /etc/apache2/sites-available:
![](img/balancerconf.png)

### Ahora, procedemos a instalar certbot y creamos un certificado para el fichero que acabamos de crear.
![](img/cerbot.png)
<br />
<br />

## Servidores Apache

### Para que nuestras instancias de la red privada tengan acceso a internet, debemos crear una gateway nat.
![](img/nat.png)

### Después, creamos una tabla de enrutamiento para nuestra subred de la base de datos y le añadimos una ruta para que se conecte a nuestra gateway nat.
![](img/tablarutas.png)

### Una vez nuestras instancias tienen conexión a internet, procedemos a configurar el servidor Apache.

### Primero, instalo la pila LAMP.
![](img/instaapache.png)

### Hacemos una copia del archivo de configuración de Apache, y editamos el DocumentRoot.
![](img/confi.png)

### Ahora activamos la configuración que acabamos de crear.
![](img/enable.png)

### Creamos la carpeta donde vamos a alojar nuestra página y descargamos los archivos desde GitHub.
![](img/git.png)

### Después, configuramos el fichero config.php y ponemos la IP y el nombre de nuestra base de datos.
![](img/confphp.png)

### Por último, reiniciamos el servicio de Apache e instalamos el cliente de MariaDB.
![](img/mariadbclient.png)

## MySQL

### Primero, instalamos el servidor MariaDB.
![](img/installmaria.png)

### Editamos el archivo 50-server.cnf para modificar el bind-address.
![](img/50server.png)

### Cargamos el script de la base de datos, creamos el usuario y le damos los permisos para poder usar la base de datos de la aplicación.
![](img/basedatos.png)

### Comprobamos que hay conexión desde nuestro servidor Apache a la base de datos.
![](img/compro.png)
