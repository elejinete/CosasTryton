# Servidor HTTPS para acceder a Tryton desde Internet.

##Instalar Nginx
Lo primero que hemos hecho ha sido instalar Nginx que es super fácil porque ya viene por defecto en la mayorí­a de distribuciones Linux. En la mayorí­a de distribuciones basadas en Debian lo haremos con el siguiente comando(Primero hacemos un 'update' para actualizar la lista de paquetes):
## Actualizamos lista de paquetes
apt-get update
## Instalamos Nginx
`apt-get install -y nginx`
Una vez instalamos hemos arrancado el servicio para comprobar que estaba funcionando:
`service nginx start`
Después hemos parado el servicio porque con el servicio Nginx corriendo no podrí­amos contratar el certificado de seguridad:
`service nginx stop`

Contratar el certificado de seguridad SSL con Let's encrypt
Para contratar el certificado de seguridad primero deberemos instalar "certbot" que es la herramienta para gestionar los certificados de seguridad (Esta herramienta también viene por defecto en la mayorí­a de distribuciones Linux): 
`apt-get install -y certbot`
Y ahora que ya tenemos la herramienta instalada podemos solicitar el certificado con un solo comando y sin que se nos pida nada adicional (Ideal para automatizar procesos de devops):

	certbot certonly \
	    -d midominio.com \
	    --noninteractive \
	    --standalone \
	    --agree-tos \
	    --register-unsafely-without-email
Como se comenta en el ví­deo, certbot permite varios métodos para pedir el certificado, en nuestro caso vamos a usar `--standalone` que básicamente crear un servidor web interno para validar el dominio (Ese es el motivo por el que no podemos tener arrancado Nginx).
También le ponemos le activamos el modo no interactivo y le configuramos todo lo que nos va a pedir el script por defecto. De esta manera podemos usar este script para automatizar procesos de aprovisionamiento.

###Configurar Nginx
Ahora que ya tenemos nuestros certificados ya podemos configurarlos donde sea, en este caso, en un servidor Nginx. Para ellos modificamos el fichero '/etc/nginx/sites-available/default' que contiene el 'site' por defecto en Nginx:

		server {
			listen 80 default_server;
			return 301 https://$host$request_uri;
			}
		server {
			listen 443 ssl;
			ssl_certificate     /etc/letsencrypt/live/test2.albertcoronado.com/fullchain.pem;
			ssl_certificate_key /etc/letsencrypt/live/test2.albertcoronado.com/privkey.pem;
			ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
			ssl_ciphers         HIGH:!aNULL:!MD5;
			root /var/www/html;
			index index.html index.htm index.nginx-debian.html;
			location / {
				try_files $uri $uri/ =404;
				}
			}
		
Aquí­, básicamente lo que hacemos en configurar para que todas las llamadas por el puerto 80(http) sean redirigidas al puerto 443(https). El puerto 443, queda configurado como estaba el puerto 80 por defecto pero aí±adiendo las lí­neas que configuran el certificado SSL.
Acordaos que cada vez que hacéis cambios en este archivo hay que reiniciar el servicio de Nginx. 
###Renovar los certificados de Let's Encrypt
Los certificados de Let's Encrypt caducan cada seis meses, para renovarlos solo hay que ejecutar el siguiente comando:
`sudo certbot renew`
OJO previamente tenemos que tener el servidor parado por lo cual
`sudo systemctl stop nginx`
Recordad que se debe ejecutar este comando con el Nginx parado para que "certbot" pueda validar que realmente el dominio es vuestro.

		sudo certbot certonly \
		-d *******mydominio.com \
		--noninteractive \
		--standalone \
		--agree-tos \
		--register-unsafely-without-email
						    
-
	Texto a cargar en el fichero default de nginx

		server {
			listen 80 default_server;
			return 301 https://$host$request_uri;
			}
		server {
			listen 443 ssl;
			ssl_certificate     /etc/letsencrypt/live/tryton.rdiautomatizacion.com/fullchain.pem;
			ssl_certificate_key /etc/letsencrypt/live/tryton.rdiautomatizacion.com/privkey.pem;
			ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
			ssl_ciphers         HIGH:!aNULL:!MD5;
		 #       root /var/www/html;
		 #       index index.html index.htm index.nginx-debian.html;
		#        location / {
		#                try_files $uri $uri/ =404;
		#                }
		 location / { proxy_pass http://127.0.0.1:8000;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header Host $host; }
			}
