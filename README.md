Cambios a la app 2.0


●	CredencialAccesoService.java:
○	Se agregó el constructor vacío para poder instanciar la clase en AppMenu y poder utilizar el método eliminar()
○	Se implementó correctamente el método eliminar(): ahora se comprueba que el registro exista antes de intentar “eliminarlo”, se inicia y finaliza la transacción manualmente (setAutoCommit(false)/(true)), se implementó rollback, y RegistroNoEncontradoException
●	CredencialAccesoDao.java:
○	Corregido el String sql en los métodos eliminar(), actualizar() y leer(). Estos hacían referencia a una columna ‘id’ inexistente en la tabla CredencialAcceso. Ahora hacen referencia correctamente a la columna “id_CredencialAcceso”
●	UsuarioService.java:
○	Ahora el método actualizar también valida el email ingresado con el método emailValido()

●	AppMenu.java: 
○	El método eliminarUsuario() ahora ejecuta también credencialService.eliminar(id) para que se actualice la baja lógica (eliminado=true) del registro en la tabla CredencialesAcceso.
○	Dentro del método crearUsuario() se ejecuta el método setRequiereReset(false), cuando por lógica, todo usuario nuevo con contraseña nueva por defecto debería requerir un reseteo de la misma por parte del usuario. Se cambia el argumento de setRequiereReset() a true.
○	Corregido el método actualizarUsuario() para que también actualice la fecha de modificación en la Credencial




Cambios a la app

●	UsuarioService.java:
○	Implementado try with resources en insertar().
○	Implementadas validaciones en :
■	username: -agregado método usernameValido()
-	devuelve true si el username es válido
-	no puede contener espacios
-	Sólo se permiten determinados símbolos. Se usa la RegEx (expresión regular): "^[a-zA-Z0-9_.-]{4,30}$"
-	Se comprueba el tamaño de la cadena (entre 4 y 30 caracteres)
-	Si no cumple, lanza IllegalArgumentException con mensaje de error personalizado.
■	email:       
●	devuelve true si el username es válido
●	agregado método emailValido()
●	comprueba caracteres y formato con RegEx. Expresión regular utilizada: "^(?=.{1,120}$)[\\w\\.-]+@([\\w\\-]+\\.)+[a-zA-Z]{2,4}$"
●	comprueba que no tenga espacios y no esté vacío
●	Si no cumple, lanza IllegalArgumentException con mensaje de error personalizado.
○	El método eliminar() ahora sólo intenta eliminar si encuentra el registro (antes intentaba eliminarlo sin comprobar la existencia del mismo. 
Ahora lanza una RegistroNoEncontradoException si no encuentra el registro a eliminar.
Ahora realiza un rollback si ocurre una SQLException.
Se agregó un bloque finally que ejecuta setAutoCommit(true) y cierra la conexión.

●	CredencialAccesoService.java:
○	Se agregó el constructor vacío para poder instanciar la clase en AppMenu y poder utilizar el método eliminar()
●	CredencialAccesoDao.java:
○	Corregido el String sql. Este hacía referencia a una columna ‘id’ inexistente en la tabla CredencialAcceso. Ahora hace referencia correctamente a la columna “id_CredencialAcceso”


●	AppMenu.java: 
○	Implementado try-catch dentro del método crear usuario para capturar las excepciones lanzadas por las validaciones de usernameValido() y emailValido().
○	Modificado leerLong() para que no acepte números negativos como id. De esta manera evitamos que se conecte a la base de datos a buscar un id negativo que obviamente no existe. Por otro lado, si recibe 0 como entrada, muestra por pantalla “Volviendo…”, ‘duerme’ 2 segundos (Thread.sleep(2000);) y retorna 0.
○	Modificados los métodos actualizarUsuario(), buscarUsuario() y eliminarUsuario() para que, al recibir un 0 desde la entrada por teclado, vuelvan al menú de opciones.
○	Al ingresar 0 como opción, la consola muestra el mensaje “Saliendo…”, y luego de 2 segundos (Thread.sleep(2000)) continúa la ejecución (el programa termina cuando el while evalúa opcion != 0)
○	Dentro del método crearUsuario() se implemente un try-catch especial para capturar un posible IllegalArgumentException lanzado por usuarioService.insertar().
○	El método eliminarUsuario() ahora ejecuta también credencialService.eliminar(id) para que se actualice la baja lógica (eliminado=true) del registro en la tabla CredencialesAcceso.
○	Dentro del método crearUsuario() se ejecuta el método setRequiereReset(false), cuando por lógica, todo usuario nuevo con contraseña nueva por defecto debería requerir un reseteo de la misma por parte del usuario. Se cambia el argumento de setRequiereReset() a true.

●	RegistroNoEncontradoException:
○	Se creó una excepción personalizada para ser lanzada cuando no se encuentra el id que ingresa el usuario. Antes de esto, al intentar eliminar un id inexistente, la app mostraba “Usuario eliminado (baja logica).”.

