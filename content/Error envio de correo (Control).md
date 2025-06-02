---
Sistema: Control vehicular
Fecha_de_inicio: 2024-11-21T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Producción: true
Fecha_termino: 2024-11-21T00:00:00.000-06:00
---
---

Se presentaron problemas en los servicios de correo, por ende marcaba error cuando se generaba un alta de vehiculo.

**Resolución**

Se coloco en el archivo .env una bandera para poder suspender el correo mas rapido, que en lugar de comentar las lineas desde el controlador.

.env
```
#Bandera que suspende el correo  
SUSPENDO_CORREO = false
```

> Regla. Cuando la bandera esta en *false* los correo salen, cuando la bandera esta en *true* se suspende el servicio de correo

VehiculosNuevosController::guardaTramite
```
if(!env('SUSPENDO_CORREO')){  
    $qr= url("/consulta_tramite/$idtramitehash");  
    $correo = Personas::where('id', Auth::user()->id_persona)->first()->email;  
    CorreoController::FinalizaTramiteFisica($correo, $request,$qr);  
}
```

