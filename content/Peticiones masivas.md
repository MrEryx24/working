---
Sistema: OVICA
Fecha_de_inicio: 2025-01-02T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Producción: true
---
---
En ambos servidores de ovica dedicados al backend se observo que se realizaron peticiones masivas por distintas ips y agentes que vienen directamente de la pagina de ovica

![[Pasted image 20250321165244.png]]

**Resolución**

Se hizo el uso del *request* para cachar la información *ip* y *user-agent* pero se detecto que tanto los kioscos como pagina de ovica consumen el mismo servicio **getAdeudosVigentes**; debido a esto se separaron en 2 funciones 

Estructura

| Servicio                                                                                        | Controlador(es)                        | Método(s)                                           |
| ----------------------------------------------------------------------------------------------- | -------------------------------------- | --------------------------------------------------- |
| <br>https://ovica.finanzas.cdmx.gob.mx/ovica-backend/public/api/v1/adeudos/vigente/130200909990 | PagosController                        | getAdeudosVigentes                                  |
| https://ovica.finanzas.cdmx.gob.mx/ovica-backend/public/kioskos/solicitar_adeudo_vigente        | PagosController, KioskosControllerRest | solicitar_adeudo_vigente, getAdeudosVigentesKioscos |
|                                                                                                 |                                        |                                                     |

getAdeudosVigentes
```
try {

            Log::info('Iniciamos obtencion de ip, revisamos primero headers');
            Log::info($request->headers->all());

            $clientIp = $request->header('X-Forwarded-For');#Guardamos la cabecera

            if ($clientIp) {#Si la cabecera tiene informacion

                $ipsArray = explode(',', $clientIp);
                Log::info('IPS QUE VIENENE DE LA VARIABLE CLIENTIP  || '. json_encode($ipsArray));
                $originalClientIp = trim($ipsArray[0]);# La primera IP en la lista es la IP del cliente original

            } else {#Si la cabera esta vacia

                $originalClientIp = $request->ip();
                Log::info('IP TOMADA DEL REQUEST->IP  || '. json_encode($originalClientIp));
            }

            $ip = $originalClientIp;

            $userAgent = $request->header('User-Agent');
            Log::info('User-agent '. $userAgent);

            

            //------

            if($ip == '201.103.64.92' || $ip == '158.23.138.117' || $ip == '158.23.81.138' || substr($ip, 0,6) ==='158.23' || $userAgent == 'python-requests/2.32.3'){

                Log::info('peticion de: '.$ip);
                return false;
            }else{

                $this->modelAdeudos = new Adeudos();
                $adeudos = [];
                if (!preg_match('/[a-zA-Z0-9]{12}/', $ctapredial)) {
                    return response()->json(['mensaje' => 'Formato de cuenta invalido'], 400);
                }
                $inmueble = $this->modelAdeudos->getIdInmueble($ctapredial); Log::info("LA INFO DE LOS INMUEBLES EN VIGENTE "); Log::info($inmueble);
                //error_log(json_encode($inmueble));

                if($inmueble){
                    if(empty($inmueble['idinmueble'])){
                        return response()->json(['mensaje' => 'La cuenta no existe.'], 404);
                    }

                        switch ($inmueble['codestadocuenta']) {
                            case 998:
                                break;
                            case 1:
                            case 2:
                            case 4:
                            case 5:
                            case 999:
                                if(env('APP_ENV') === 'prod'){
                                    return response()->json(['mensaje' => $inmueble['descestadocuenta']], 404);
                                }
                            break;
                            case 666:
                                if(env('APP_ENV') === 'prod'){
                                    return response()->json(['mensaje' => "Cuenta activa, próxima a emitir"], 404);
                                }
                            break;

                            default:
                                break;
                        }


                if(env("PROGRAMA")){
                    //comentado para el programa del 2021
                    //$adeudos = $this->modelAdeudos->getPagosVigentesDescuento($inmueble['idinmueble'], $ctapredial);
                    //comentado para el programa del 2021 de Julio
                    //$adeudos = $this->modelAdeudos->getPagosVigentes($inmueble['idinmueble'], $ctapredial);
                    //AGREGADO PARA EL PROGRAMA DEL 2021 DE JULIO
                    /*$rangos = array('A','B','C','D','E','F','G');
                    $periodo = 1;
                    $infoRango = $this->modelLiquidacion->getCodrangotarifa(array('PERIODO'=>$periodo,'ANO'=>'2021'),$inmueble);
                    error_log("LA INFO EN VIGENTE ".json_encode($infoRango));

                    $noHabitacional = false;
                    if(count($infoRango) > 0){
                        foreach($infoRango as $infoR){
                            if(trim($infoR['coduso']) != 'H'){
                                $noHabitacional = true;
                            }
                        }
                    }else{
                        if($infoRango[0]['coduso'] != 'H'){
                            $noHabitacional = true;
                        }
                    }

                    if(in_array(trim($infoRango[0]['codrangotarifa']),$rangos) && $noHabitacional == false){
                        error_log("ENTREEEE AQUI VIG 2 ");*/

                        //COMENTADO EL 13/05/2022 para programa 2022
                        /*$emision = $this->modelAdeudos->getProcedureEmisionVigente($inmueble['idinmueble']);

                        if((trim($emision['IMPORTELCA1ANT']) != '' && $emision['IMPORTELCA1ANT'] > 0) || (trim($emision['IMPORTELCA2ANT']) != '' && $emision['IMPORTELCA2ANT'] > 0)){
                            $adeudos = $this->modelAdeudos->getPagosVigentesDescuento_21_2($inmueble['idinmueble'], $ctapredial);
                        }else{
                            $adeudos = $this->modelAdeudos->getPagosVigentes($inmueble['idinmueble'], $ctapredial);
                        }*/
                        //FIN COMENTADO EL 13/05/2022
                    /*}else{
                        error_log("ENTREEEE AQUI VIG 3 ");
                        $adeudos = $this->modelAdeudos->getPagosVigentes($inmueble['idinmueble'], $ctapredial);
                    }*/

                    //AGREGADO EL 13/05/2022 para programa 2022
                    $adeudos = $this->modelAdeudos->getPagosVigentes($inmueble['idinmueble'], $ctapredial);

                    $rangos = array('A','B','C','D','E','F','G');

                    if(count($adeudos) > 0){
                        //$infoRangoPRUEBA = $this->modelLiquidacion->getCodrangotarifa(array('PERIODO'=>$adeudos[0]['periodo'],'ANO'=>'2022'),$inmueble); //Log::info("LA INFO DE LOS RANGOS ".date('Y')." ".$adeudos[0]['periodo']); Log::info($infoRangoPRUEBA);
                        $infoRango = $this->modelLiquidacion->getCodrangotarifa(array('PERIODO'=>$adeudos[0]['periodo'],'ANO'=>'2022'),$inmueble); //Log::info("LA INFO DE LOS RANGOS "); Log::info($infoRango);

                        $noHabitacional = true;
                        if(count($infoRango) > 0){
                            $controlHabitacional = 0;
                            foreach($infoRango as $rango){
                                if(trim($rango['coduso']) == 'H'){
                                    $controlHabitacional = $controlHabitacional+1;
                                }
                            }
                            //Log::info("LA COMPARATIVA "); Log::info($controlHabitacional." == ".count($infoRango));
                            if($controlHabitacional == count($infoRango)){
                                $noHabitacional = false;
                            }
                        }

                        $adeudos[0]['RANGO'] = '';
                        if(in_array(trim($infoRango[0]['codrangotarifa']),$rangos) && $noHabitacional == false){
                            $adeudos[0]['RANGO'] = trim($infoRango[0]['codrangotarifa']);
                        }

                        /*$adeudos[0]['IDCOLONIA'] = '';
                        $infoColonia = $this->modelAdeudos->getIdColonia($ctapredial); //Log::info($infoColonia);
                        foreach($infoColonia as $inf){
                            if(isset($inf['IDCOLONIA']) && trim($inf['IDCOLONIA']) != ''){
                                $adeudos[0]['IDCOLONIA'] = $inf['IDCOLONIA'];
                            }
                        }*/


                    }

                    //Log::info("INFO DE VIGENTES"); Log::info($adeudos);
                    //FIN AGREGADO EL 13/05/2022
                }else{
                    //error_log("ENTREEEE AQUI VIG 4 ");
                    $adeudos = $this->modelAdeudos->getPagosVigentes($inmueble['idinmueble'], $ctapredial);
                }

                if (count($adeudos) > 0) {
                    return response()->json($adeudos, 200);
                } else {
                    return response()->json(['mensaje' => 'No se dispone de adeudos vigentes para esta cuenta predial.'], 404);
                }
            }else{
                return response()->json(['mensaje' => 'La cuenta no existe.'], 404);
            }

            }



            
        } catch (\Throwable $th) {
            error_log($th);
            Log::info($th);
            return response()->json(['mensaje' => 'Ocurrió algo inesperado, intente nuevamente más tarde.'], 500);
        }
```

Se agregaron las validaciones para cachar las **ips** de origen y el **user-agent**,  así como las reglas para bloquear ciertos segmentos o el agente **python-requests**

```
Log::info('Iniciamos obtencion de ip, revisamos primero headers');
            Log::info($request->headers->all());

            $clientIp = $request->header('X-Forwarded-For');#Guardamos la cabecera

            if ($clientIp) {#Si la cabecera tiene informacion

                $ipsArray = explode(',', $clientIp);
                Log::info('IPS QUE VIENENE DE LA VARIABLE CLIENTIP  || '. json_encode($ipsArray));
                $originalClientIp = trim($ipsArray[0]);# La primera IP en la lista es la IP del cliente original

            } else {#Si la cabera esta vacia

                $originalClientIp = $request->ip();
                Log::info('IP TOMADA DEL REQUEST->IP  || '. json_encode($originalClientIp));
            }

            $ip = $originalClientIp;

            $userAgent = $request->header('User-Agent');
            Log::info('User-agent '. $userAgent);

            

            //------

            if($ip == '201.103.64.92' || $ip == '158.23.138.117' || $ip == '158.23.81.138' || substr($ip, 0,6) ==='158.23' || $userAgent == 'python-requests/2.32.3')
```