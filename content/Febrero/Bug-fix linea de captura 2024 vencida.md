---
Sistema: Control vehicular
Fecha_de_inicio: 2025-02-04T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Sin respuesta
Note: "[[Bug-fix linea de captura 2024 vencida]]"
Nota: "[[1.Sistemas]]"
Fecha_termino: 2025-02-10T00:00:00.000-06:00
Mes: Febrero
---
**Bug-fix:** El día de ayer realice mi trámite de alta cuidada a imprimí Mis líneas 2024 y 2025  de mi vehículo Mazda  hoy fui a pagarlas y realizó el pago de la 2025 y cuando quiero pagar la 2024 me dice que está vencida, procedo a sacarla nuevamente en mi cuenta y me arroja esta leyenda.

EL problema radica cuando se da clic en el botón **Detalle de pago 2024**
![[Pasted image 20250205114306.png]]

Se agrega try catch para cachar el error y los datos que llegan 

```
try {  
    \Log::info('lista de pagos del niv '.$niv);  
    $año = Carbon::now()->format('Y')-1;  
    $datos = self::pendientes_($niv, $año);  
    \Log::info($datos);  
    return view('modals/pagos/detalle_pago_a')->with("datos",$datos);  
  
}catch (\Throwable $th) {  
    \Log::error($th->getMessage().' en el archivo '.$th->getFile().' linea '.$th->getLine());  
}
```

---

**Resolución:** El usuario no dio respuesta después de la petición de que volviera a intentar