![image](https://github.com/user-attachments/assets/35354215-a762-4399-adec-5d4d4c5f7248)


________________________________________________________________________________________


ACTIVIDAD 1

#!/bin/bash

# Comprobar el número de parámetros
if [ "$#" -ne 1 ]; then
    echo "Uso: $0 <puerto>"
    exit 1
fi

PUERTO=$1
CONFIG_APACHE="/etc/httpd/conf/httpd.conf" 

# Comprobar si el puerto ya está presente
if grep -q "Listen $PUERTO" "$CONFIG_APACHE"; then
    echo "El puerto $PUERTO ya está presente en la configuración de Apache."
    exit 1
fi

# Añadir el puerto al fichero de configuración
echo "Listen $PUERTO" >> "$CONFIG_APACHE"
echo "Puerto $PUERTO añadido a la configuración de Apache."

________________________________________________________________________________________


ACTIVIDAD 2

#!/bin/bash


if [ "$#" -ne 2 ]; then
    echo "Uso: $0 <IP> <dominio>"
    exit 1
fi

IP=$1
DOMINIO=$2
FICHERO_HOSTS="/etc/hosts"


if grep -q "$DOMINIO" "$FICHERO_HOSTS"; then
    echo "El dominio $DOMINIO ya existe en el fichero hosts."
    exit 1
fi


echo "$IP $DOMINIO" >> "$FICHERO_HOSTS"
echo "Dominio $DOMINIO añadido con la IP $IP al fichero hosts."

________________________________________________________________________________________


ACTIVIDAD 3

#!/bin/bash


if [ "$#" -ne 3 ]; then
    echo "Uso: $0 <título> <cabecera> <mensaje>"
    exit 1
fi

TITULO=$1
CABECERA=$2
MENSAJE=$3
FICHERO_PAGINA="pagina.html"

cat <<EOL > "$FICHERO_PAGINA"
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$TITULO</title>
</head>
<body>
    <h1>$CABECERA</h1>
    <p>$MENSAJE</p>
</body>
</html>
EOL

echo "Página web creada: $FICHERO_PAGINA"

________________________________________________________________________________________


