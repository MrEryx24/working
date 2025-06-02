---
Sistema: Pre-registro
Fecha_de_inicio: 2025-02-10T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Producción: true
Fecha_termino: 2025-02-10T00:00:00.000-06:00
Nota: "[[1.Sistemas]]"
Mes: Febrero
---
---

**Datos de prueba**

**Resolución:** Error en el guardado en la tabla de cuenta

El if no estaba validando correctamente ya que encontraba otros id_campo que no era cuenta predial o catastral y al buscar el value  daba error
```
if ($tipo_campo  === 6 ) {#Verificamos si el json de tramites contiene algun tipo de campo para cuenta catastral o predial  
  
    Log::info('Cuenta => '. $tramite['value'] . ' Nombre '. $tramite['nombre'] . ' id tipo campo '. $tramite['id_tipo_campo']);    $registro = Cuentas::setRegisterCuenta($tramite['nombre'],$tramite['value'],$id);  
}else if($tramite['id_tipo_campo'] == 2 || $tramite['id_tipo_campo'] == 3){  
  
    if ($tramite['nombre'] == 'CuentaCatastral' || $tramite['nombre'] == 'Cuentacatastral' || $tramite['nombre'] == 'cuentacatastral' || $tramite['nombre'] == 'cuenta_catastral' || $tramite['nombre'] == 'Cuenta_Catastral'        || $tramite['nombre'] == 'CuentaPredial' || $tramite['nombre'] == 'Cuentapredial' || $tramite['nombre'] == 'cuentapredial' || $tramite['nombre'] == 'cuenta_predial' || $tramite['nombre'] == 'Cuenta_Predial'    ){        Log::info('Cuenta => '. $tramite['value'] . ' Nombre '. $tramite['nombre'] . ' id tipo campo '. $tramite['id_tipo_campo']);        Log::info($tramite);        $registro = Cuentas::setRegisterCuenta($tramite['nombre'],$tramite['value'],$id);        Log::info('Linea 256 => '.$registro);    }}
```

Solución se cambia a switch
```
$tipo_campo = $tramite['id_tipo_campo'];  
  
switch ($tipo_campo) {  
    case 2:  
    case 3:  
    case 6:  
  
        $tipo_de_cuenta = $tramite['nombre'];  
  
        switch ($tipo_de_cuenta){  
            case 'CuentaCatastral':  
            case 'cuentacatastral':  
            case 'cuenta_catastral':  
            case 'Cuenta_Catastral':  
            case 'CuentaPredial':  
            case 'cuentapredial':  
            case 'cuenta_predial':  
            case 'Cuenta_Predial':  
                $cuenta = $tramite['value'];  
                Log::info('Cuenta => '. $tramite['value'] . ' Nombre '. $tramite['nombre'] . ' id tipo campo '. $tramite['id_tipo_campo']);  
                $registro = Cuentas::setRegisterCuenta($tramite['nombre'],$tramite['value'],$id);  
            break;        }  
    break;  
}
```

