Para poder configurar que la plataforma Tryton nos envié mails hay que editar el fichero trytond.conf y añadir entre las etiquetas de mail la instrucción uri con los parámetros de conexión y la contraseña del mail a usar.
Para verificar si funciona hay que rearrancar el servidor y reiniciar el navegador de la aplicación. Ambas cosas.  
`[email]`  
`uri = smtp+tls://tryton@MiServidorCorreo.com:ContraseñaDeLaCuentaCorreo@mail.MiServidorCorreo.com:465`
`from = tryton@MiServidorCorreo.com`  
`[email]`  
`uri = smtp+tls://<username>:<password>@<your_fqdn_server>:587`  
`from = <address>@<domain.ext>`  

