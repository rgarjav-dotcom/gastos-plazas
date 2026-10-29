# Gastos Plazas Península

Control de gastos por proveedor para Cancún, Playa del Carmen, Mérida y Campeche, con comparativa mes a mes, presupuesto de TI y proyección con inflación. Los datos viven en el proyecto **Gastos-Plazas** de Supabase.

## Acceso por número de empleado

Cada persona entra con su **número de empleado** y una contraseña. La primera vez usa una contraseña temporal y la página le pide crear una propia.

Todo se administra en supabase.com → proyecto Gastos-Plazas → **SQL Editor**:

| Para | Ejecuta |
|---|---|
| Dar de alta | `select admin.alta_empleado('12345', 'Nombre Apellido');` |
| Ver contraseñas temporales | `select * from admin.credenciales_iniciales;` |
| Restablecer una contraseña | `select admin.restablecer_contrasena('12345');` |
| Quitar el acceso | `select admin.baja_empleado('12345');` |
| Ver quién tiene acceso | `select empleado, nombre, agregado from public.usuarios_autorizados order by empleado;` |

Cuando entregues las contraseñas temporales, bórralas: `delete from admin.credenciales_iniciales;`

## Seguridad

La llave de `index.html` es la llave publicable de Supabase y es pública por diseño. Los datos los protegen las reglas de acceso de la base: sin sesión de un empleado dado de alta no se puede leer ni escribir nada. Las funciones de administración y las contraseñas temporales están en el esquema privado `admin`, que la página no puede usar.
