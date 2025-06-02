---
Sistema: Alta vehicular
Fecha_de_inicio: 2025-03-10T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Fecha_termino: 2025-03-10T00:00:00.000-06:00
Nota: "[[1.Sistemas]]"
Mes: Marzo
---
---
**Reportan** Podrían apoyarme con el siguiente tema, el día 04 de marzo realice unos pagos para altas de placas, el cual aun no se ven reflejados en el sistema de altas vehiculares para poder solicitar cita y recogerlas.


| LC                   | NIV               | Resolucion                                                                                                                                                                  |
| -------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 84CX83886436U4K2FBPW | LK6SMAE39TB000084 | (FINANZAS) Linea Captura: 84CX83886436U4K2FBPW --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732362"} |
| 84CX8388651WK4K2FBY0 | LK6SMAE32TB000069 | (FINANZAS) Linea Captura: 84CX8388651WK4K2FBY0 --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732364"} |
| 84CX8388663SV4K2FB3F | LK6SMAE38TB000108 | (FINANZAS) Linea Captura: 84CX8388663SV4K2FB3F --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732367"} |
| 84CX8388687I74KNQARE | 3GKAL8EG5TL100056 | (FINANZAS) Linea Captura: 84CX8388687I74KNQARE --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732368"} |
| 84CX8388711G64KNQAF5 | 3GKAL8EG8TL100052 | (FINANZAS) Linea Captura: 84CX8388711G64KNQAF5 --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732370"} |
| 84CX8388715NS4KQ8HXH | 3GKAL8EG3TL100007 | (FINANZAS) Linea Captura: 84CX8388715NS4KQ8HXH --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732373"} |
| 84CX8388723LB4KNHHNX | 3GKAL8EGXTL100022 | (FINANZAS) Linea Captura: 84CX8388723LB4KNHHNX --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59732376"} |
- Se envía al área de semovi en el grupo *SAF-SEMOVI 2022* 
- Una vez que responde semovi se deja pasar una hora para que ejecute el cron
-  Se ejecuta la siguiente consulta 

```
select * from log where log.linea_captura = NIV ;
```

Deberemos tener la siguiente respuesta:
```
 (FINANZAS) Linea Captura: 84CX78854223E9PFH4V3 --  Se envió correctamente ! --- {"error":"exito","code":200,"data":"El tramite se guardo correctamente con folio 59223339"}
```

