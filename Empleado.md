SISTENCIA EMPLEADO.
ATTENDANCE EMPLOYER

Para crear un nuevo empleado con la opción de logerse y poder fichar la presencia en el trabajo.
1- Creamos el empleado como tercero.
2- Asignamos este tercero como empleado en empresas nuevo empleado 
3- Administration / Users creamos un nuevo usuario de Login y en la parte inferior lo ligamos con la empresa y con el empleado. Asignamos la contraseña. 

Funcionamiento a nivel de empleado.


Configurar usuarios con menos privilegios
I have the same problem.
I have tried several ways and finally I have done the following:
first I created a User only to used to attendance application.
For that, we register a new login user and assign it to a new party (Employer) with the same name. When you login on user you have a generic tree of icons that you can see. This is because some items in the tree do not have a group assigned, and that is what a new user can see (not group assigned then you can see the icon with user without privileges) .

Second I need to clear items tree to the user.

So I create a group in >Administration > User > Groups called for example “standard user”
I go to “User Interface” > Menu and selecting the initial icons of the tree Party, product etc. and change to empty group to the new created group “Standard User”. Once all items the items without group are assigned to “Standard User” the user without group, have an empty session without an item tree.

Third I need assigned to user attendance icon
I create a new group to manage the access to my user for that I duplicate Shift+Crtl+D the group (attendance administration) group and name the new group like as (employer attendance)
I assign in Administration > User > Users the new user to the group created (employer attendance). Also I need to assigned on Administration > User Interface > Menu Icon attendance to (group attendance employer) Now have two groups (Employer Attendance) and ((Employer)*see first point)
Now, the user only sees the Attendance icon in the tree but because we duplicate the attendance administrator group profile, the user have the configuration option on the tree. To remove this privilege I go to Administration > User > Groups and I select the new group created (attendance employer) group and in the tab [Access Menu] tab I remove the configuration icon Attendance/configuration.
Now everything seems ok I have a user who only sees the attendance icon when login, but, I have a problem. The user also sees other parties and if he clicks on the party, he accesses the information of the party, address, telephone, etc.
To limit access to third-party information I go to Administration > User > Groups and click to the new group employer attendance and on the [access permissions] tab and sub-tab [access models] I have added the party model with the read box unchecked. Save and now, the user sees the lines of the other users but, when clicking on the party it does not show information about it.
I don`t now if is correct because I rookie on Tryton but works for me.
Be caution not applied groups with less privileges to ADMIN USER
