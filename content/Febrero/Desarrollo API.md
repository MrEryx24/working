---
Sistema: OVICA
Fecha_de_inicio: 2025-02-13T00:00:00.000-06:00
Tipo_de_solicitud: feature
Estatus: Finalizado
Producción: true
Fecha_termino: 2025-02-13T00:00:00.000-06:00
Nota: "[[1.Sistemas]]"
Mes: Febrero
---
---

**Descripción:** Generación de API  para consultar información del inmueble.

Diagrama de flujo:
![[Ovica_page-0001.jpg]]

Servidor productivo 

| Servidor    | Rama                     | Version    |
| ----------- | ------------------------ | ---------- |
| 10.1.128.24 | feature/api-consulta-fis | QA         |
| 10.1.128.44 | prod_serv_44             | PRODUCCION |

Se usa la siguiente consulta con query builder
```
$response = DB::connection('ovica-precat')  
    ->select("SELECT FIS.OBTENER_INFO_INMUEBLE_P(:codigo) FROM DUAL", ['codigo' => $ctaPredial]);
```

