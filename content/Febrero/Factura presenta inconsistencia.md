---
Sistema: Control vehicular
Fecha_de_inicio: 2024-11-21T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Nota: "[[1.Sistemas]]"
Producción: true
Fecha_termino: 2025-02-18T00:00:00.000-06:00
Mes: Febrero
---
---

![[a34d2b83-84d3-4d0f-8114-1950fcf07bbf.jpg]]

**Respuesta del log:**  
```
local.ERROR: Client error: `GET 128.222.200.20/sicove/v1/consume/repuve/TSMYAA2S2RMB51968` resulted in a `409 Conflict` response:
{"error":"Registro de REPUVE no existe","code":409}
```

![[Pasted image 20250217111153.png]]

**Resolución** Se verifica respuesta de servicio de repuve, se cacha y se limpia mensaje

WebServicesController::WebServicesRepuve
```
try {  
  
         $client = new Client();  
         $response = $client->get(env('URL_SEMOVI_REPUVE').$niv, ['auth' => [env('AUTH_SEMOVI_USER'), env('AUTH_SEMOVI_PASS')]]);  
$response = json_decode( $response->getBody()->getContents(), true );  
  
return $response;  
  
     } catch (\Throwable $th) {  
         $error = $th->getMessage();  
         preg_match('/{.*}/', $error, $matches);  
  
         if (!empty($matches)) {  
             $jsonPart = $matches[0];  
             $responseJson = json_decode($jsonPart, true);  
             return $responseJson;  
         }else{  
             \Log::error("Error en servicio de repuve: ".__METHOD__." -> ".$th->getLine()." -> ". $th->getMessage()." -> ".$th->getCode());  
             return false;  
         }  
     }
```

