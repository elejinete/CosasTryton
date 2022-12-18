I am starting to experiment with the docker image. While I am doing so, I will write step-by-step instructions.  
I have no previous experience of Docker and little knowledge of Tryton, so I will need help.  
The following is a starting point. Would you like me to put this somewhere official?
 
##Como usar la imagen de Docker Tryton para instalar un servidor de Tryton

[Pagina oficial para de como  instalar Tryton con Docker](https://hub.docker.com/r/tryton/tryton/) Previamente hay que tener instalado Docker en el ordenador. 

Descargamos la imagen de Tryton `sudo docker pull tryton/tryton` con sudo
 
“Start a PostgreSQL instance”  
`sudo docker run --name tryton-postgres -e POSTGRES_PASSWORD=mysecretpassword -e POSTGRES_DB=tryton -d postgres`  
“Setup the database”  
`sudo docker run --link tryton-postgres:postgres -it tryton/tryton trytond-admin -d tryton --all`  

Ejecutando la preparacion de la base de datos nos pide el mail y la contraseña:  
	◦ "admin" email for "tryton": your_email@blah.com  
	◦ “admin” password for “tryton”: a_strong_password  
	
Arrancamos Tryton 
`sudo docker run --name tryton -p 8000:8000 --link tryton-postgres:postgres -d tryton/tryton`

Podemos conectarnos en este punto a Tryton http://localhost:8000/  
    • No muestra un cuadro de entrada a Tryton en el Navegador
	◦ Database: tryton  
	◦ User name: admin  
	◦ Pulsar login  
    • Nos pide la password. 
    • Metemos la password introduccida en la creacion de la base de datos.
      
Arrancar a Tryton cron instance  
No tengo claro para que sirve este arranque ya que parece que sin el funciona Tryton

This is the dedicate cron part of the server (http://manpages.ubuntu.com/manpages/bionic/man1/trytond-cron.1.html).
`sudo docker run --name tryton-cron --link tryton-postgres:postgres -d tryton/tryton trytond-cron -d `


Arrancando los contenedores Docker despues de un rearranque del ordenador.  
Con el comando ls vemos los contenedores Docker:  
sudo docker container ls --all  
OR
sudo docker ps --all  
Para arrancar Tryton necesitamos arrancar los 3 contenedores asociados a el:  

`sudo docker start tryton-postgres tryton tryton-cron`  
El orden es importante primero arranca el servidor Postgres luego el servidor Tryton y por último Cron.  


###Resumen de los comandos usados para crear nuestro servidor Tryton con Docker

	sudo docker pull tryton/tryton

	sudo docker run --name tryton-postgres -e POSTGRES_PASSWORD=xxxxxxx -e POSTGRES_DB=tryton -d postgres

	sudo docker run --link tryton-postgres:postgres -e DB_PASSWORD=xxxxxxx -it tryton/tryton trytond-admin -d tryton –all

	sudo docker run --name tryton -p 8000:8000 --link tryton-postgres:postgres -e DB_PASSWORD=xxxxxxx -d tryton/tryton

Ya te puedes conectar a Tryton en este punto

	sudo docker run --name tryton-cron --link tryton-postgres:postgres -d tryton/tryton trytond-cron -d tryton



