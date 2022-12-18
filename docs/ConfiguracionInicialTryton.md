Con los contenedores de Docker en run ya podemos conectarnos al servidor mediante la IP y el puerto 8000.

Inmediatamente Tryton comienza el asistente de la instalación.

Instalamos lo módulos
Configuramos ojo creamos la Moneda el factor de redondeo 0.01 digitos 2


Es probable que si no instalamos los países con sus provincias al importar los terceros nos puede dar error al intentar ligar con la base de datos.
Una vez Instalado los contenedores podemos instalar los paises con el siguiente comando.
docker exec --interactive --tty tryton /entrypoint.sh python3 -m trytond.modules.country.scripts.import_countries -d tryton
También hay que cargar los idiomas por ejemplo Spanish para poder asignar a los usuarios su idioma regional.
Para ello en Administration>Users>Localization>Languages
Buscamos el idioma Clikamos y ejecutamos el translation quizás para poder seleccionar el nuevo lenguaje hay que logearse con el usuario en cuestión y el el menu Administration>User seleccionamos nuestro propio usuario y en la pestaña Preferences abrimos el list Languajes y selecionamos Spanish.
La moneda la podemos instalar a mano ya que solo usamos euros.

Despues de editar hay que iniciar los periodos Fiscal years.
Para ello tenemos que asignar un nombre al periodo un día de inicio y final 1 de enero y 31 de diciembre
En la pestaña Secuences hay que crear Post Move Secuence, Supplier Invoice Sequence, Supplier Credit Note Sequence, Customer Invoice Sequence, Customer Credit Note Sequence.
Una vez creadas estas secuencias se nos mostrarán en Administration > Secuences
> Secuences Strict Aqui podemos personalizar el número que nos da a las facturas emitidas.

Posteriormente a iniciar los periodos ejecutar el asistente para incorporar el plan general.

Notas en 
Moneda el factor de redondeo 0.01 digitos 2
En >User Interface > Actions > Reports Podemos descargar y subir los modelos de OpenOffice.
Importante cuando generamos una factura consolildada el modelo report usado en ese momento se congela para esa factura consolidada. Esto quiere decir que al cambiar el report invoice solo se aplicara a nuevas facturas.

En >Administracion > Secuences> Customer Secuence y Suplier Secuence definir los números para empezar las facturas.

