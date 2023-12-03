# **LAMP en tres niveles**
#### Marcos de la Marta Núñez

<br />
<br />

# Índice.
1. [Introducción.](#introducción)
2. [VPC.](#VPC)
3. [Instancias.](#Instancias)
   * [Servidores Apache.](#servidores-apache)
   * [MySQL.](#Mysql)
   * [Balanceador.](#balanceador)


# Introducción.
 Para establecer la infraestructura, requeriremos la implementación de cuatro máquinas. Una de ellas se designará como el balanceador de carga, mientras que las dos restantes fungirán como servidores web. La última máquina desempeñará el rol de nuestra base de datos. La configuración del balanceador se realizará en la subred pública, mientras que tanto los servidores como la base de datos se desplegarán en la subred privada.
<br />

En mi caso el dirrecionamiento va a ser el siguiente:
- Subred-Privada (Servidores Apache y servidor base de datos): **192.168.1.128/25** 
- Subred-Public (Balanceador): **192.168.1.0/25**
# VPC
## Balanceador.
### En primer lugar creamos la VPC (Virtual Private Cloud).
![](img/vpc.png)

### Depues dentro de esa VPC que hemos creado, cremaos dos subresdes una subred que va a tener salida a internet en la que estara elñ balanceador y otra subred privada para nuestros servidor de Backend.
![](img/subred1.png)

![](img/subred2.png)
<br />
<br />

# Instancias


### Una vez creada las dos subredes procedemos a crear las instancias. <br />
### Primero crearemos la instancia balanceador.
![](img/balanceador.png)

### Ahora creamos las instancia para los servidores Apache
![](img/apache.png)

![](img/apache.png)

### Por ultimo creamos las instancia del servidor MYSQL.

![](img/DDBB.png)

### Para que nuestra maquina balanceador tenga salida a internet le demos asignar una IP elastica a la intancia.

### Primero creamos una puerta de enlace de internet y la asociamos a la VPC.
![](img/puerta.png)

### Ahora asociamos la IP elastica a la instacia del balanceador. 

![](img/puertainternet.png)
<br />
<br />

## Configuracion balanceador

### Primero debemos editar la tabla de enrutamiento de la VPC y le añadimos una nueva regla para permitir la conexion a la puerta de enlace de internet
![](img/segur.png)

### Primero instalamos apache y activamos los modulos proxy_balancer
### Depues editamos el archivo balanceador.conf que está en el directorio /etc/apache2/sites-available:

![](img/balancerconf.png)

### Ahora procedemos a instalar certbot y creamos un certificado para el fichero que acabamos de crear


![](img/cerbot.png)
<br />
<br />

## Servidores Apache


### Para que nuestra instancias de la red privada tenga acceso a internet debemos crear una gateway nat.
![](img/nat.png)

### Despues creamos una tabla de enrutamniento para nuestra subred de la base de datos y le añadimos una ruta para que se conecte a nuestra gateway nat.

![](img/tablarutas.png)


### Una vez nuestras intancias tienen conexion a internet procedemos a configurar el servidort apache

### Primero instalo la pila LAMP.
![](img/instaapache.png)

### Hacemos una copia del archivo de configuracion de apache, y editamos el DocumentRoot.
![](img/confi.png)

### Ahora activamos la configuracion que acabamos de crear.
![](img/enable.png)

### Creamos la carpeta donde vamos a alojar nuestra pagina y descargamos los ficheros desde github.
![](img/git.png)

### Despues configuramos el fichero config.php y ponemos la ip y el nombre de nuestra base de datos.

![](img/confphp.png)

### Por ultimo reiniciamos el servicio de apache y instalamos el cliente de mariadb.

![](img/mariadbclient.png)


## Mysql

### Primero instalamos el servidor mariadb.
![](img/installmaria.png)

### Editamos el archivo 50-server.cnf para modificar el bind-address.

![](img/50server.png)

### Cargamos el script de la base de datos, creamos el usuario y le damos los permisos para poder usar la base de datos de la aplicación.

![](img/basedatos.png)

### Comprobamos que hay conexion desde nuestro servidor apache a la base de datos.
![](img/compro.png)