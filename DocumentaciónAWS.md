Actividad AWS - Documentación 

Lo primero de todo hay que actualizar con 

sudo apt update
sudo apt upgrade -y

![image](https://github.com/user-attachments/assets/8107e54f-604d-4e5e-aee2-1b68e67d2695)

Una vez todo actualizado instalo el servidor de Apache con 
sudo apt install apache2 -y
![image](https://github.com/user-attachments/assets/33a8768d-fa04-4486-bbe8-9a923f2b4512)

Lo siguiente es instalar MySQL con
sudo apt install mysql-server -y
![image](https://github.com/user-attachments/assets/2f24e870-aaa9-45ae-b1bb-677e94400c63)

Para instalar la autenticación MySQL en Apache se utiliza el siguiente comando
sudo apt install libapache2-mod-auth-mysql -y

![image](https://github.com/user-attachments/assets/f17117d5-8f30-4c6b-ab2c-cc2298878bd9)

Ahora hay que editar el archivo de configuración de Apache para incluir la autenticación MySQL con el siguiente comando 
sudo nano /etc/apache2/apache2.conf

![image](https://github.com/user-attachments/assets/a646365b-2919-498f-a504-d5d4929fc08b)

Debemos agregar la siguiente  directiva

<Directory /var/www/html>
    AuthType Basic
    AuthName "Restricted Content"
    AuthBasicProvider dbd
    AuthDBDUserPWQuery "SELECT password FROM users WHERE username = %s"
    Require valid-user
</Directory>

![image](https://github.com/user-attachments/assets/1670866e-e965-4f74-b7f1-0e9e432e717c)

El siguiente paso es crear certificado autofirmado y activar el módulo SSL
sudo a2enmod ssl
sudo a2enmod headers

![image](https://github.com/user-attachments/assets/36d69638-f657-44c0-b544-2e8601d82114)
![image](https://github.com/user-attachments/assets/f71aa89f-27c6-41e3-9d02-4ad1a73a0075)

El siguiente paso es crear un directorio para almacenar los certificados SSL.
sudo mkdir /etc/apache2/ssl

![image](https://github.com/user-attachments/assets/385067bd-f983-4558-b6c3-aaf362d33d7f)

Para generar el certificado autofirmado se debe de ingresar el siguiente comando
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt

![image](https://github.com/user-attachments/assets/b1aa7274-8e1f-4271-b971-07597a3270ef)

Ahora debemos configurar el apache para usar el certificado SSL con
sudo nano /etc/apache2/sites-available/default-ssl.conf

Actualizo las siguientes líneas con la ruta de tu certificado y clave:
SSLCertificateFile /etc/apache2/ssl/apache.crt
SSLCertificateKeyFile /etc/apache2/ssl/apache.key

![image](https://github.com/user-attachments/assets/d10bf4bb-1fa6-4098-82d9-a02bcbacbe39)

Guardo con CTRL X habilito el sitio SSL
sudo a2ensite default-ssl

Por último instalamos el módulo mod_authn_dbd para que todo funcione correctamente
sudo apt-get install libaprutil1-dbd-mysql
![image](https://github.com/user-attachments/assets/6eef7596-0c03-4306-aa70-d1ca329aa983)

Y se habilitan los módulos 
sudo a2enmod authn_dbd
sudo a2enmod dbd

![image](https://github.com/user-attachments/assets/052d68ea-b801-4e80-84bb-f23b649f3b99)

Compruebo el estado de apache por si diese algún error
sudo apache2ctl configtest
![image](https://github.com/user-attachments/assets/f91d2ab9-23e4-4f54-aa52-d3e170758f4a)


Después de ver que todo esté correcto, se reinicia apache
sudo systemctl restart apache2

![image](https://github.com/user-attachments/assets/edeff031-5039-4ba9-bbcd-88f6630a1c80)

Y hasta aquí la práctica.









