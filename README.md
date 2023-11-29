# **LAMP en tres niveles**
#### Marcos de la Marta Núñez

<br />
<br />

## VPC

### Primero creamos VPCs.
![](img/vpc.png)

### Ahora cremaos dos subresdes una para los servidores Apache y otra para nuestro servidor de BBDD.
![](img/subred1.png)

![](img/subred2.png)
<br />
<br />

## Instancias

### Una vez creada las dos subredes procedemos a crear las instancias. <br />
### Primero creamos la instancia para el balanceador
![](img/balanceador.png)

### Ahora creamos las instancia para los servidores Apache
![](img/apache.png)

![](img/apache.png)

### Por ultimo creamos las instancia del servidor MYSQL

![](img/DDBB.png)

### A continuacion creamos dos tarrjetas de red con la sub red de la BBDD para asignarla a los servidores APACHE.

![](img/interfaces.png)

### Depues le demos asignar una IP elastica a la intancia del balanceador.

### Primero creamos una puerta de enlace de internet y la asociamos a la VPC.
![](img/puerta.png)

### Ahora asociamos la IP elastica a la instacia del balanceador. 

![](img/puertainternet.png)
<br />
<br />

## Configuraciion balanceador

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


