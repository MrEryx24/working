---
Sistema: Alta vehicular
Fecha_de_inicio: 2024-11-16T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Producción: true
Fecha_termino: 2024-11-16T00:00:00.000-06:00
---
---
Se presentaron problemas en los servicios de correo, por ende marcaba error cuando se generaba un alta de vehiculo.

**Resolución**

Se comentaron las líneas encargadas del envió de correo 

mainController::storagePDF
```
#Envío correo electrónico  
try {  
    $sendmail = new mailController();  
    $sendmail->enviaCorreoAltaRegistro($request->Niv);  
} catch (\Throwable $th) {  
    DB::rollback();  
    \Log::error($th->getMessage().' en el archivo '.$th->getFile().' linea '.$th->getLine());  
    return response('Falló servicio de envío de correo', 400);  
}
```


mainController::storagePDFMoto
```
try {  
    $sendmail = new mailController();  
    $sendmail->enviaCorreoMoto($request);  
    $sendmail->enviaCorreoAltaRegistro($request->Niv);  
  
} catch (\Throwable $th) {  
  
    DB::rollback();  
    \Log::error($th->getMessage().' en el archivo '.$th->getFile().' linea '.$th->getLine());  
    return response('Falló servicio de envío de correo', 400);  
}
```
