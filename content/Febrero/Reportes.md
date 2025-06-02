---
Sistema: SSO
Fecha_de_inicio: 2025-02-04T00:00:00.000-06:00
Tipo_de_solicitud: Reporte excel
Estatus: Finalizado
Fecha_termino: 2025-02-04T00:00:00.000-06:00
Producción: false
Nota: "[[1.Sistemas]]"
Mes: Febrero
---
---

## Consultas por base de datos 

Para generar consultas de los usuarios de *SIGAPRED* por base de datos y obtener los siguientes datos:

- Datos personales
- Datos laborales
- Datos de sesión
- Estatus del usuario 
- Trayectoria y cambios que ha sufrido el usuario 
- Datos de acceso 

1. Realizar la consulta en **oracle** para obtener *Datos personales, Datos laborales, Datos de sesión*
```
SELECT cu.IDUSUARIO, cu.CURP, cu.NOMBRE, cu.APELLIDOPATERNO, cu.APELLIDOMATERNO, cs.SUBAREA, ca.AREA,
cu2.LOGIN, CU2.ERRORES, cu2.HASHCLAVE, cu2.CADUCIDAD
FROM CAC_USUARIO cu
INNER JOIN CAC_SUBAREA cs ON cu.IDSUBAREA = CS.IDSUBAREA
INNER JOIN CAC_AREA ca ON cs.IDAREA = ca.IDAREA
INNER JOIN CAC_USUMEC cu2 ON cu.IDUSUARIO = cu2.IDUSUARIO
WHERE rfc LIKE '%MOBR%';
```

*Salida*
![[Pasted image 20250326130902.png]]

2. Para obtener los datos *Estatus del usuario ,Trayectoria y cambios que ha sufrido el usuario, Datos de acceso* es necesario tener el **IDUSUARIO** de la consulta **Oracle**

*Estatus del usuario*
```
select * from usuario_complemento uc where id_usuario = 9033;
```

*Plataformas y roles asignados al usuario*
```
select rupr.id, rupr.id_usuario as usuario, cp.nombre as plataforma, cr.nombre as role from rel_usuarios_plataforma_roles rupr
inner join ca_plataformas cp on cp.id = rupr.id_plataforma
inner join ca_roles cr on cr.id = rupr.id_rol
where rupr.id_usuario = 9329
```

***Salida***
![[Pasted image 20250326131235.png]]

*Trayectoria del usuario*
```
SELECT id_usuario as usuario_movimiento, cp.nombre as plataforma, ch.movimiento, ch.datos, ch.created_at, ch.ip as ip_origen FROM ca_historico ch
inner join ca_plataformas cp on cp.id = ch.id_plataforma
WHERE datos->'usuario'->>'idusuario' = '8066' and movimiento = 'Asignación de rol a usuario' order by created_at ASC;
```

***Salida***
![[Pasted image 20250326131634.png]]

```
--Query para consultar la tabla historico por el id_usuario que se modifico
SELECT id_usuario as usuario_movimiento, cp.nombre as plataforma, ch.movimiento, ch.datos, ch.created_at, ch.ip as ip_origen FROM ca_historico ch
inner join ca_plataformas cp on cp.id = ch.id_plataforma
WHERE datos->'usuario'->>'idusuario' = '8066' --Se refiere a un campo JSON en la columna datos. Utiliza la notación de JSON para acceder a un valor específico, en este caso 'usuario'->>'idusuario'
and
movimiento = 'Asignación de rol a usuario' --El tipo de movimiento que se busca
order by created_at ASC;
```

***Salida***
![[Pasted image 20250326131720.png]]

Esta consulta SQL selecciona información de la tabla `ca_historico` (con alias `ch`) y de la tabla `ca_plataformas` (con alias `cp`) a través de un **JOIN**. El propósito es obtener detalles específicos de un movimiento registrado en la base de datos. A continuación te explico lo que hace cada parte de la consulta:

### Explicación por partes:

1. **Selección de columnas:**
    
    - `id_usuario as usuario_movimiento`: Obtiene el ID del usuario desde la tabla `ca_historico` y lo renombra como `usuario_movimiento`.
        
    - `cp.nombre as plataforma`: Obtiene el nombre de la plataforma desde la tabla `ca_plataformas` y lo renombra como `plataforma`.
        
    - `ch.movimiento`: Obtiene el tipo de movimiento desde la tabla `ca_historico`.
        
    - `ch.datos`: Obtiene los datos asociados al movimiento.
        
    - `ch.created_at`: Obtiene la fecha y hora en que ocurrió el movimiento.
        
    - `ch.ip as ip_origen`: Obtiene la dirección IP desde la cual se registró el movimiento, renombrándola como `ip_origen`.
        
2. **FROM y JOIN:**
    
    - `FROM ca_historico ch`: La consulta se hace sobre la tabla `ca_historico`, que está aliasada como `ch`.
        
    - `INNER JOIN ca_plataformas cp ON cp.id = ch.id_plataforma`: Realiza un **INNER JOIN** entre la tabla `ca_historico` y la tabla `ca_plataformas` usando la columna `id_plataforma` de `ca_historico` y la columna `id` de `ca_plataformas`. Esto permite obtener el nombre de la plataforma asociada al registro.
        
3. **Condición en el WHERE:**
    
    - `datos->'usuario'->>'idusuario' = '8066'`: Filtra los registros en los que el campo `datos` (que parece ser un tipo JSON o JSONB) contiene un valor en el objeto `usuario` donde el valor de `idusuario` sea igual a `'8066'`.
        
    - `movimiento = 'Asignación de rol a usuario'`: Filtra los registros donde el tipo de movimiento sea `'Asignación de rol a usuario'`.
        
4. **Ordenamiento:**
    
    - `ORDER BY created_at ASC`: Ordena los resultados por la fecha de creación (`created_at`) en orden ascendente (de más antiguo a más reciente).
        

### En resumen:

La consulta devuelve un listado de registros donde el movimiento es una "Asignación de rol a usuario", específicamente para el usuario con `idusuario` igual a `8066`. Además, obtiene información sobre la plataforma, la fecha y hora del movimiento, la IP de origen, y los datos asociados al movimiento, ordenados por la fecha en la que ocurrieron (de más antiguo a más reciente).
