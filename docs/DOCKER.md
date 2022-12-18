En caso de que se pare el servidor de Tryton en Docker hay que saber como arrancarlo:

Que pasa en caso de re-arranque del servidor Tryton. 
Pues que tenemos que arrancar los container de docker que estan inactivos.  
Primero el contenedor de postgre con el comando docker start “container ID”  y lo mismo para el container de tryton.  

Para Ver el estado de Docker .  
`docker images`= Muestra las imagenes disponibles.    
`Docker ps`  = Muestra los contenedores.  
`Docker Start` = Arranca los contenedores en caso de que se paren.  
Con este comando entramos en el contenedor de postgres >> `docker exec -it tryton-postgres psql -U postgres`  

En caso de que la caguemos y tengamos que reiniciar la base de datos borramos los contenedores con el comando Docker rm y el número ID del contenedor y volvemos a ejecutar los 3 comandos de Docker vistos en <b>Instalar Tryton con Docker.</b> Sera mas rápido ya que las imagenes ya estan instaladas en el servidor. Con esto empezaremos de 0

Para hacer la copia de seguridad  
`sudo docker stop tryton`  
$ `sudo docker exec tryton-postgres pg_dump -C -c -U postgres -O tryton >tryton-backup.sql`  
$ `sudo docker start tryton`  
Una vez echo el Backup se guarda en el directorio en el cual hemos ejecutado Si tienes Tryton ejecutandose en un ordenador remoto, el comando para pasar el fichero que esta en el servidor remoto a nuestro ordenador local es el siguiente comando:  

`scp debian@MiServidorRemoto.com:/home/debian/tryton-backup.sql .`  
Ojo el último punto de la instrucción es necesario
Nos pedira la contraseña de acceso al ordenador remoto

MiContraseñaDeServidorRemoto  

En ese momento la copia de seguridad del ordenador remoto bajara al directorio donde ejecutemos el comando.  

<font color="blue">
Para entrar dentro de Docker como usuario ROOT simplemente ejecutamos 
`docker exec -it -u root IDCONTENEDOR /bin/bash` no pide contraseña
</font>

