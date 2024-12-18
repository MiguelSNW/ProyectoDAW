PRÁCTICA PRIMER TRIMESTRE DESPLIEGUE DE APLICACIONES WEB - MIGUEL ÁNGEL GRANDE SÁNCHEZ

![image](https://github.com/user-attachments/assets/84cbe262-e34c-428e-bdc8-6183ae81387d)


REALIZACIÓN DE LA TAREA


SERVIDOR 1 - APACHE
_____________________________________________________________________________________________

Lo primero que voy a hacer es actualizar el sistema con

sudo apt update

![image](https://github.com/user-attachments/assets/6b499451-fbe7-42d8-85f9-8943d029d6b2)

para actualizarlo, se debe colocar

sudo apt upgrade -y

![image](https://github.com/user-attachments/assets/bd06721f-d49b-4527-b632-7325b8799fee)

A continuación, empezaremos el proyecto instalando apache2 con el siguiente comando

sudo apt install apache2 -y

![image](https://github.com/user-attachments/assets/8a98cb2f-6a54-4dbe-9f3d-aa6c7c92224c)


seguidamente, instaleré php y algunas extensiones más que me vendrían bien para la realización del proyecto

sudo apt install php libapache2-mod-php php-mysql php-curl php-xml php-mbstring -y

![image](https://github.com/user-attachments/assets/3e74050f-2a74-478a-9079-5d5aed03b128)

Una vez instalado todo lo anterior, configuraré el archivo hosts para que los dominios centro.intranet y departamentos.centro.intranet apunten a mi servidor local.

sudo nano /etc/hosts

![image](https://github.com/user-attachments/assets/b4b72d92-ab27-4d3c-9334-c5a5bff9007d)


le añadiré las siguientes líneas al final del archivo y guardaré con CTR X

127.0.0.1 centro.intranet
127.0.0.1 departamentos.centro.intranet

![image](https://github.com/user-attachments/assets/3a3d1e7c-68d0-4cdf-a8c0-611d39aa4c9a)


Lo siguiente es configurar un Virtual Hosts en apache, empezando con crear los directorios donde se alojarán los archivos de wordpress y la aplicación python

sudo mkdir -p /var/www/centro.intranet
sudo mkdir -p /var/www/departamentos.centro.intranet

![image](https://github.com/user-attachments/assets/9a24643a-14fd-42ab-9dd7-8df0663bddf3)

Continúo asignándole los permisos adecuados a los directorios:

sudo chown -R $USER:$USER /var/www/centro.intranet
sudo chown -R $USER:$USER /var/www/departamentos.centro.intranet

![image](https://github.com/user-attachments/assets/2d58a1be-2ec4-4a8f-819e-cfa866ad7aae)

Creé un archivo de configuración de Virtual Hosts para cada dominion con

sudo nano /etc/apache2/sites-available/centro.intranet.conf
y agrego la siguiente directiva 
<VirtualHost *:80>

    ServerName centro.intranet
    
    DocumentRoot /var/www/centro.intranet
    
    <Directory /var/www/centro.intranet>
    
        AllowOverride All
        
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/centro.intranet-error.log
    
    CustomLog ${APACHE_LOG_DIR}/centro.intranet-access.log combined
    
</VirtualHost>


![image](https://github.com/user-attachments/assets/2972f3da-b077-4068-a760-bf95d817c813)

Para departamentos.centro.intranet

sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
y agrego la siguiente directiva

<VirtualHost *:80>

    ServerName departamentos.centro.intranet
    
    DocumentRoot /var/www/departamentos.centro.intranet
    
    <Directory /var/www/departamentos.centro.intranet>
    
        AllowOverride All
        
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/departamentos.centro.intranet-error.log
    
    CustomLog ${APACHE_LOG_DIR}/departamentos.centro.intranet-access.log combined
    
</VirtualHost>


![image](https://github.com/user-attachments/assets/0bf617ce-1b14-4013-b633-ce902085af9a)

El siguiente paso es habilitar los archivos VirtualHosts recién creados con

sudo a2ensite centro.intranet.conf
sudo a2ensite departamentos.centro.intranet.conf

![image](https://github.com/user-attachments/assets/bb00456a-bcf2-4d6d-a1e6-4e8ea24d38e2)

Para WordPress, requiere que el módulo de reescritura de Apache este activado, para hacerlo coloqué

sudo a2enmod rewrite

![image](https://github.com/user-attachments/assets/34c3c5db-b059-4f91-91b2-2e8e0ad9c4d8)

Y reinicio apache con 

sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/8223e6f0-6f26-40ea-9f80-5365a78bb448)

El siguiente paso es la instalación de Wordpress, para descargarlo coloqué en el terminar

cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz

![image](https://github.com/user-attachments/assets/4b2d849b-fe1b-41aa-b1e2-cdff5635d4f2)

El siguiente paso es mover wordpress al directorio correspondiente (centro.intranet) con

sudo mv /tmp/wordpress/* /var/www/centro.intranet/

![image](https://github.com/user-attachments/assets/c0aefbfa-e97d-4da7-9fb3-3731cb72f843)

Asigno los permisos adecuados con 

sudo chown -R www-data:www-data /var/www/centro.intranet
sudo chmod -R 755 /var/www/centro.intranet

![image](https://github.com/user-attachments/assets/f2d2a61d-a778-4c2c-8d10-349c8252b715)

Lo siguiente es instalar una base de datos para wordpress

sudo apt install mysql-server -y

![image](https://github.com/user-attachments/assets/c8380ad0-9bb5-447a-ba78-64fcb05347c6)

Ahora deberé de meterme dentro de la consola Mysql para crear una base de datos y un usuario para wordpress

sudo mysql

![image](https://github.com/user-attachments/assets/fac38887-71f1-41b0-be0f-9a4fee572a11)

Dentro de la consola mysql, coloqué lo siguiente

CREATE DATABASE wordpress;

![image](https://github.com/user-attachments/assets/9849c1de-0c5d-4f7c-91e6-b1cd102c7d56)


CREATE USER 'root'@'localhost' IDENTIFIED BY 'root';

![image](https://github.com/user-attachments/assets/989bf9a0-a0d1-4d67-a780-1aa37931c7ff)


GRANT ALL PRIVILEGES ON wordpress.* TO 'root'@'localhost';

![image](https://github.com/user-attachments/assets/5b78ed0e-77e8-4b6c-92a0-f918352d8199)


FLUSH PRIVILEGES;

![image](https://github.com/user-attachments/assets/c714374f-04a9-4c3a-a432-dcfe6acf18b6)


EXIT;

![image](https://github.com/user-attachments/assets/1982e2f2-d3ea-4e3f-9a04-37fc23aadd32)

Ahora debo configurar WordPress, lo primero es copiar el archivo de configuración con

cd /var/www/centro.intranet
cp wp-config-sample.php wp-config.php

![image](https://github.com/user-attachments/assets/ea059846-3b3b-4fda-ad11-fe95c7f1ac18)

Y edito el archivo wp-config.php para agregar la información a la base de datos

![image](https://github.com/user-attachments/assets/71e1ffaa-7b2b-47d8-91a8-6197adb970d9)

Hay que buscar las siguientes líneas y actualizar con mis datos

define('DB_NAME', 'wordpress');

define('DB_USER', 'root');

define('DB_PASSWORD', 'root');

define('DB_HOST', 'localhost');


![image](https://github.com/user-attachments/assets/8487ba4a-9942-484d-ba07-a6bf422b71fd)

Y de momento, se deja Wordpress.

Empiezo con la configuración de la aplicación de Python, para ello debo utilizar el módulo mod_wsgi. Para ello debo empezar instalando el mod_wsgi con

sudo apt install apache2 libapache2-mod-wsgi-py3 -y

![image](https://github.com/user-attachments/assets/bcf41ef2-b6f2-4db5-a84e-c11db3bce56e)

Ahora debo crear un directorio para mi aplicación, haré una muy simple que devuelva el típico 'Hello World'.
Accedo al directorio departamentos.centro.intranet y creo un archivo llamado app.wsgi.

sudo mkdir -p /var/www/departamentos.centro.intranet
sudo nano /var/www/departamentos.centro.intranet/app.wsgi

![image](https://github.com/user-attachments/assets/1442e253-6630-431f-9e70-1f62d10339c3)

Una vez dentro, agregaré el contenido descrito anteriormente:

def application(environ, start_response):

    status = '200 OK'
    
    headers = [('Content-type', 'text/plain; charset=utf-8')]
    
    start_response(status, headers)
    
    return [b'Hello World']
    

![image](https://github.com/user-attachments/assets/5f5219c4-171d-4a87-a465-0e11652c4e1c)

Ahora hay que terminar de configurar apache2 para que aparezca mi aplicación en la web

sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf

<VirtualHost *:80>

    ServerName departamentos.centro.intranet
    
    WSGIScriptAlias / /var/www/departamentos.centro.intranet/app.wsgi
    
    <Directory /var/www/departamentos.centro.intranet>
    
        Require all granted
        
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/departamentos.centro.intranet-error.log
    
    CustomLog ${APACHE_LOG_DIR}/departamentos.centro.intranet-access.log combined
    
</VirtualHost>

![image](https://github.com/user-attachments/assets/6dadee65-793c-4a2f-8ea2-3db4f958bf2b)

Por último, habilito el sitio y reinicio Apache

sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/b4c566ac-01b1-4573-a98d-cee7b2a7975f)

Para comprobar que todo está correcto hasta ahora, me meteré en el navegador de firefox y pondré las dos direcciones a ver que sucede

http://departamentos.centro.intranet
http://centro.intranet

![image](https://github.com/user-attachments/assets/bba993c1-03fc-44ce-849a-21d17f31b495)

![image](https://github.com/user-attachments/assets/b400fbe9-ee49-4544-a798-4702285ba760)

Como se puede comprobar, se abre la aplicación de Python y en la otra página se abre WordPress, perfecto. El siguiente paso es proteger el acceso
a la aplicación Python mediante un login, lo primero que se debe de hacer es crear un archivo que contenga la contraseña para acceder al archivo Python.
Se empieza instalando apache2-utils y activando el módulo mod_auth_basic

sudo apt install apache2-utils -y

![image](https://github.com/user-attachments/assets/959529f6-8ff9-4ec5-b60a-e092d2a0f743)
![image](https://github.com/user-attachments/assets/8356e47e-ca2d-4e7d-b917-267bf107f9cc)


se crea un archivo de contraseña con

sudo htpasswd -c /etc/apache2/.htpasswd usuario  (ESTO CREA EL USUARIO "usuario" CON LA CLAVE QUE NOSOTROS QUERAMOS)

![image](https://github.com/user-attachments/assets/57b97ecf-f1e7-4992-98c5-bc6c0c838213)

Ahora, dentro del archivo de configuración donde se encuentra el archivo python se deben escribir las siguientes líneas dentro de <VirtualHost>

AuthType Basic

AuthName "Restricted Access"

AuthUser File /etc/apache2/.htpasswd

Require valid-user


![image](https://github.com/user-attachments/assets/7eabd1ac-dbd4-4783-ab79-e014c9ba7f30)



Se reinicia apache
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/b023f119-432a-45f7-9ab4-b6399d0daacb)



__________________________________________________________________________________________


Una vez realizado los pasos anteriores, comencé a instalar AWStats

sudo apt install awstats -y

![image](https://github.com/user-attachments/assets/80f2a334-0043-4df5-a3bc-298171ae230f)

Una vez instalado, hay que configurarlo, accediendo a él con

sudo nano /etc/awstats/awstats.conf

![image](https://github.com/user-attachments/assets/e1e74a21-9d56-4838-9395-e961f91773d7)

Dentro de este archivo, debo asegurarme que las líneas estén configuradas con mi sitio web

SiteDomain="centro.intranet"
HostAliases="centro.intranet www.centro.intranet"

![image](https://github.com/user-attachments/assets/732f6801-88ff-4b59-a3ce-c09dbadb1f96)


Por último, hay que agregar un script de actualización para awstats, se realiza con

sudo nano /etc/cron.d/awstats

Dentro del archivo debemos introducir la siguiente línea

0 * * * * www-data /usr/lib/cgi-bin/awstats.pl -update -config=centro.intranet

![image](https://github.com/user-attachments/assets/35677b42-bf3b-4ae9-b045-6e7560cf73f0)

Ahora accedo a Awstats con la siguiente dirección en el navegador:
http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet
![image](https://github.com/user-attachments/assets/0744f370-04d1-4bde-a6f1-357ddba06cb7)


__________________________________________________________________________________________


Y listo, ahora poseemos dos direcciones en nuestro VirtualHosts, una (http://centro.intranet) que posee Wordpress,

![image](https://github.com/user-attachments/assets/2281b2e3-debf-42f4-bfb3-e08985dd344d)


y otra (http://departamentos.centro.intranet) que posee una autenticación para acceder a un archivo Python.

![image](https://github.com/user-attachments/assets/956cf8aa-5ded-423a-8188-a305c47751c7)

Si inicio sesión, me saldrá mi archivo python.

![image](https://github.com/user-attachments/assets/892d6eb0-edcb-4f76-9cb1-2161e8da2fcc)

Y Acceder a Awstats mediante

http://centro.intranet/cgi-bin/awstats.pl?config=centro.intranet
![image](https://github.com/user-attachments/assets/3c96e95a-66a7-455e-9c62-9d636f71d843)



SERVIDOR 2 - Nginx
_________________________________________________________________________________________________

El primer paso de todos es instalar el servidor Nginx

sudo apt install nginx

![image](https://github.com/user-attachments/assets/346d464f-9e92-4904-a519-c4f0218f3872)

El siguiente paso es configurar nginx para el dominio y el puerto que me indica en el ejercicio con

sudo nano /etc/nginx/sites-available/servidor2.centro.intranet

![image](https://github.com/user-attachments/assets/effc0868-8bd4-4d0a-a2aa-c2e03a850559)

Debo agregar lo siguiente 

server {

    listen 8080;
    
    server_name servidor2.centro.intranet;
    
    root /var/www/servidor2;
    
    index index.php index.html index.htm;

    location / {
    
        try_files $uri $uri/ =404;
        
    }
    location ~ \.php$ {
    
        include snippets/fastcgi-php.conf;
        
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        
        include fastcgi_params;
        
    }
    
   location ~ /\.ht {
   
        deny all;
        
    }
    
}

![image](https://github.com/user-attachments/assets/1aad2244-e226-4c20-918f-2171be43c725)


Seguimos con la creación del directorio raíz para el sitio web

sudo mkdir -p /var/www/servidor2

y sus respectivos permisos

sudo chown -R www-data:www-data /var/www/servidor2
sudo chmod -R 755 /var/www/servidor2

![image](https://github.com/user-attachments/assets/11e671e6-efcd-48dc-81c4-a5e52c692f77)

Y habilito la configuración del sitio 

sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/

![image](https://github.com/user-attachments/assets/750ee614-538b-448b-b7cc-0a0bf4780980)

Y pruebo la configuración por si da errores con 

sudo nginx -t

![image](https://github.com/user-attachments/assets/78aa9260-4b46-4b0f-889b-b5c415ef22a4)


Reinicio nginx

sudo systemctl restart nginx

![image](https://github.com/user-attachments/assets/744dbee9-684d-4c3f-88d3-9032ffe4cc88)

Lo siguiente es instalar PHP y PHP-FPM sencillamente con 

sudo apt install php php-fpm php-mysql


![image](https://github.com/user-attachments/assets/b49e2f81-24ee-40d4-99d9-715525e6ff48)

Y me aseguro de si está en ejecución

sudo systemctl start php8.3-fpm
sudo systemctl enable php8.3-fpm

![image](https://github.com/user-attachments/assets/5701c1fe-16bf-41f0-9880-a72f777eda7b)


Una vez hecho esto, toca instalar phpMyAdmin con 
sudo apt install phpmyadmin
y durante la instalación nos da a elegir un servidor web (NGINX NO APARECERÁ) así que elegiré apache2

![image](https://github.com/user-attachments/assets/43ef0640-9503-4f3d-b474-5f20663c80d1)

Y configuraré phpmyadmin creando un enlace simbólico en el directorio de nginx 

sudo ln -s /usr/share/phpmyadmin /var/www/servidor2/phpmyadmin

![image](https://github.com/user-attachments/assets/9dc7590d-3eec-4491-9301-ccca07cabc1e)

A continuación, hare que nginx pueda servir archivos de phpmyadmin agregando "location" dentro de server{}

location /phpmyadmin {

    alias /usr/share/phpmyadmin;
    
    index index.php index.html index.htm;
    
    location ~ ^/phpmyadmin/(.*\.php)$ {
    
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        
        fastcgi_index index.php;
        
        include fastcgi_params;
        
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        
    }
    
}

![image](https://github.com/user-attachments/assets/fc8cf9c3-675f-43fd-9923-91ff7d2a022e)


Una vez colocado "location" guardo y reinicio nginx

sudo systemctl restart nginx

![image](https://github.com/user-attachments/assets/57b3eeb7-355f-4ab9-a78b-362a078b2b2b)

Por último, configuraré el archivo hosts y meteré un php de prueba para ver que todo funciona correctamente

sudo nano /etc/hosts

y agrego la ruta

127.0.0.1 servidor2.centro.intranet

![image](https://github.com/user-attachments/assets/b28bfe71-8bfc-493d-9a53-ca847fd09acf)

Crearé el archivo de prueba de php 

sudo nano /var/www/servidor2/helloworld.php



Y haré otro Hello World igual al del archivo PYTHON

![image](https://github.com/user-attachments/assets/2bf47186-6d14-4dfd-9a66-d28c05e04c33)

Listo, con esto e introduciendo en el navegador http://servidor2.centro.intranet:8080/helloworld.php debería de funcionar mi helloworld.php

![image](https://github.com/user-attachments/assets/1207fc3d-5b8b-4f1c-81b7-9de49b2421c8)

Y lo mismo si quiero acceder simplemente a phpMyadmin, me dirijo a http://servidor2.centro.intranet:8080/phpmyadmin/

![image](https://github.com/user-attachments/assets/ec657801-5681-4b31-8e60-5fc7d2414ab8)

Con esto, se da por terminado mi proyecto de primer trimestre de DAW

_______________________________________________________________________________________

TRABAJO REALIZADO POR MIGUEL ÁNGEL GRANDE SÁNCHEZ
