---
Sistema: Control vehicular
Fecha_de_inicio: 2025-02-19T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Fecha_termino: 2025-02-19T00:00:00.000-06:00
Mes: Febrero
Nota: "[[1.Sistemas]]"
---
---
Envio un caso de Control Vehicular
Estoy tratando de hacer mi tramite en linea para darme de alta y sacar las placas de mi vehiculo nuevo pero el sistema arroja un error, dice que el archivo .xml de mi factura no cuenta con el numero de identificacion NIV, que es el numero de serie de 17 digitos pero mi factura si cuenta con el, incluso en el archivo xml se puede leer.

![[Imagen de WhatsApp 2025-02-19 a las 10.56.28_6209c146.jpg]]

**Respuesta:** Para un mejor entendimiento, recomendamos revisar la documentación oficial del SAT sobre el complemento para facturas electrónicas de venta de vehículos nuevos. Es importante notar que los parámetros claveVehicular y NIV (Número de Identificación Vehicular) deben incluirse en la factura, según lo establece la normativa.
Estos parámetros no se encuentran en el XML de la manera en que el sistema los requiere y como lo especifica la documentación. Para obtener información detallada sobre la estructura correcta y su implementación, en la sección 'Complementos concepto' en el punto 2 "Venta de vehiculos" sobre el enlace "Estándar" la página oficial del SAT:
https://www.sat.gob.mx/portal/public/tramites/complementos-de-factura 
