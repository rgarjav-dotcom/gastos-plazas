# Gastos Plazas Península

Control de gastos por proveedor para Cancún, Playa del Carmen, Mérida y Campeche, con comparativa mes a mes, presupuesto de TI y proyección con inflación. Los datos viven en el proyecto **Gastos-Plazas** de Supabase.

## Acceso por número de empleado

Cada persona entra con su **número de empleado** y una contraseña. La primera vez usa una contraseña temporal y la página le pide crear una propia.

Hay dos roles:

- **Captura:** ve el tablero y registra gastos.
- **Admin:** además ve el botón **Admin**, donde da de alta usuarios, restablece contraseñas, quita accesos y descarga reportes en Excel.

### Acceso por plaza

Cada usuario de **Captura** tiene una plaza: Cancún, Playa del Carmen, Mérida, Campeche o **Las 4 plazas**.

- Con una plaza asignada solo ve y captura los gastos de esa plaza. No ve el Presupuesto de TI y no puede cambiar la inflación guardada.
- La base de datos aplica esta regla, no solo la página.
- Los **Admin** siempre ven las 4 plazas.
- La plaza se elige al dar de alta y se cambia en la tabla de usuarios del módulo Admin.

### Primer administrador

El primer admin se crea una sola vez en supabase.com → proyecto Gastos-Plazas → **SQL Editor**:

```sql
select admin.alta_empleado('TU-NUMERO', 'Tu nombre', 'admin');
select * from admin.credenciales_iniciales;
```

Después, todo lo demás se hace desde el módulo **Admin** de la página.

### Desde el SQL Editor (opcional)

| Para | Ejecuta |
|---|---|
| Dar de alta | `select admin.alta_empleado('12345', 'Nombre Apellido', 'captura', 'Mérida');` (sin plaza = las 4) |
| Cambiar la plaza | `update public.usuarios_autorizados set plaza = 'Cancún' where empleado = '12345';` |
| Ver contraseñas temporales | `select * from admin.credenciales_iniciales;` |
| Restablecer una contraseña | `select admin.restablecer_contrasena('12345');` |
| Quitar el acceso | `select admin.baja_empleado('12345');` |
| Ver quién tiene acceso | `select empleado, nombre, agregado from public.usuarios_autorizados order by empleado;` |

Cuando entregues las contraseñas temporales, bórralas: `delete from admin.credenciales_iniciales;`

## Seguridad

La llave de `index.html` es la llave publicable de Supabase y es pública por diseño. Los datos los protegen las reglas de acceso de la base: sin sesión de un empleado dado de alta no se puede leer ni escribir nada. Las funciones de administración y las contraseñas temporales están en el esquema privado `admin`, que la página no puede usar.
