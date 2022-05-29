# Práctica:Despliegue de un servicio de monitorización con zabbix

**COMO DESCARGAR ZABBIX**

**1. Creo una máquina virtual  de Ubuntu y una vez creada inserto en la terminal el siguiente comando:**

wget https://repo.zabbix.com/zabbix/4.2/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.2-1+bionic_all.deb
 
![imagen](/imagenes/imagen1.png)
 
**2. Lo instalo con el comando:**
 
sudo dpkg -i zabbix-release_4.2-1+bionic_all.deb
 
![imagen](/imagenes/imagen2.png)
 
**3. Actualizo repositorio con el comando:**
 
sudo apt update
 
![imagen](/imagenes/imagen3.png)
 
**4. Instalo los paquetes necesarios para el servidor Zabbix con el comando:**
 
sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-agent
 
![imagen](/imagenes/imagen4.png)
 
![imagen](/imagenes/imagen4b.png)
 
**5. Conecto al servidor de bases de datos con el comando y dejo la contraseña en blanco:**
 
sudo mysql -uroot -p
 
![imagen](/imagenes/imagen5.png)
 
**6. Creamos la Base de datos con el comando:**
 
create database zabbix character set utf8 collate utf8_bin;
 
![imagen](/imagenes/imagen6.png)
 
**7. Creo al usuario zabbix con contraseña password y le damos todos los privilegios sobre la base de datos zabbix:**

grant all privileges on zabbix.* to zabbix@localhost identified by 'password';

**8. Cierro mysql:**

quit;
 
![imagen](/imagenes/imagen7y8.png)
 
**9. Importamos el esquema inicial y los datos, utilizando la contraseña antes creada:**

zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
Nos pedirá la contraseña del usuario zabbix.
 
![imagen](/imagenes/imagen9.png)
 
**10. Editamos el fichero con el comando  sudo nano /etc/zabbix/zabbix_server.conf y modificamos las siguientes propiedades:**
 
**DBUser=zabbix**
**DBPassword=password**
 
![imagen](/imagenes/imagen10.png)
 
**11. Editamos el fichero con el comando sudo nano /etc/zabbix/apache.conf para especificar nuestra zona horaria:**
 
**<IfModule mod_php5.c>**
    **php_value max_execution_time 300**
    **php_value memory_limit 128M**
    **php_value post_max_size 16M**
    **php_value upload_max_filesize 2M**
    **php_value max_input_time 300**
   **php_value max_input_vars 10000**
    **php_value always_populate_raw_post_data -1**
    **php_value date.timezone Europe/London            <--- añadir esta línea**
**</IfModule>**
**<IfModule mod_php7.c>**
   **php_value max_execution_time 300**
    **php_value memory_limit 128M**
    **php_value post_max_size 16M**
    **php_value upload_max_filesize 2M**
    **php_value max_input_time 300**
    **php_value max_input_vars 10000**
    **php_value always_populate_raw_post_data -1**
    **php_value date.timezone Europe/London            <--- añadir esta línea**
**</IfModule>**
 
![imagen](/imagenes/imagen11.png)
 
**12. Reinicio el servidor y el agente Zabbix, y el servidor web Apache:**
 
sudo systemctl restart zabbix-server zabbix-agent apache2

**13. Configuro los servicios Zabbix para que se inicie automáticamente en el arranque:**
 
sudo systemctl enable zabbix-server zabbix-agent apache2
 
![imagen](/imagenes/imagen12y13.png)
 
**14. Accedo a la interfaz de Zabbix para administrarlo a través de  la siguiente dirección http://localhost/zabbix en el navegador:**
 
 **Doy clic en Next step 2 veces**
 
![imagen](/imagenes/imagen14a.png)
 
![imagen](/imagenes/imagen14b.png)
 
**Pongo la contraseña y doy en Next step 3 veces**
 
![imagen](/imagenes/imagen14c.png)
 
**Doy clic en Finish**
 
![imagen](/imagenes/imagen14d.png)
 
**Edito con el comando sudo nano /usr/share/zabbix/conf/zabbix.conf.php el fichero**
 
**<?php**
**global $DB;**
 
**$DB['TYPE'] = 'MYSQL';**
**$DB['SERVER']          = 'localhost';**
**$DB['PORT']            = '0';**
**$DB['DATABASE']        = 'zabbix';**
**$DB['USER']            = 'zabbix';**
**$DB['PASSWORD']        = 'password';**
 
**$DB['SCHEMA'] = ' ';**
 
**$ZBX_SERVER         = 'localhost';**
**$ZBX_SERVER_PORT    = '10051';**
**$ZBX_SERVER_NAME    = 'zabbix;**
 
**$IMAGE_FORMAT_DEFAULT     = IMAGE_FORMAT_PNG;**
 
![imagen](/imagenes/imagen14e.png)
 
**IMPORTANTE: En Username pongo *Admin* y en Password pongo *zabbix***
 
![imagen](/imagenes/imagen14f.png)
 
**MONITORIZACIÓN (No conseguí monitorizar los 2 primeros)** 
 
**Los recursos de hardware (CPU, RAM, disco duro) de otra máquina virtual (Windows/Linux).**
 
**Algún servicio de red (SAMBA/SMB, FTP, SSH, HTTP, ...).**

**La página web del instituto.**

**Voy a la parte de Host y doy clic en Application**

![imagen](/imagenes/imagen15.png)
 
**Doy clic en Create application, en Name pongo WEBSITE y doy clic en Add**

![imagen](/imagenes/imagen16a.png)
 
![imagen](/imagenes/imagen16b.png)
 
**Doy clic en Web scenarios y en Create web scenario**

![imagen](/imagenes/imagen17.png)
 
![imagen](/imagenes/imagen17b.png)
 
**Relleno los campos necesarios y voy a Step, le di clic en Add y relleno los campos, una** **vez hecho todo le doy a Add**

![imagen](/imagenes/imagen18.png)
 
**En Monitoring le doy clic en Web y en IESDPM**

![imagen](/imagenes/imagen19a.png)
 
![imagen](/imagenes/imagen19b.png)

