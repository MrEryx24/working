---
Sistema: OVICA
Fecha_de_inicio: 2025-02-14T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Fecha_termino: 2025-02-14T00:00:00.000-06:00
Nota: "[[1.Sistemas]]"
Mes: Febrero
---
---
2025-02-13 14:59:43.0279 INFO Cajero PredialRepository Se va a consultar el predial anticipado 565216021982

2025-02-13 14:59:43.0279 INFO Cajero PredialRepository Obteniendo token

2025-02-13 14:59:43.0279 INFO Cajero ConsultarJsonGet TX: http://ovica.finanzas.cdmx.gob.mx/ovica-backend/public//solicitar_token/565216021982

2025-02-13 14:59:43.1047 INFO Cajero ConsultarJson RX: eyJpdiI6IkprZkxZeXBXV0VUOWZhcHNxVzNNMVE9PSIsInZhbHVlIjoieEQ5Nzk1TGZLTkQ2alJTTzM0SmYyd1VzaU00ZGQ4YjBHcHlpMzhpMTdPV29NSDRYTWFITXF5N3diSElzU1QwMSIsIm1hYyI6ImMxNWI1YWM0MmUwZmVmMGIxNjk1NDA0NjEwZjFkNjdkZDhlZGNkYWFiNjUwZmI5ZDlkZDBhN2YxY2QxZmJiYjAifQ==

2025-02-13 14:59:43.1047 INFO Cajero ConsultarJsonGet TX: http://ovica.finanzas.cdmx.gob.mx/ovica-backend/public//kioskos/solicitar_adeudo_anticipado

2025-02-13 14:59:43.2381 INFO Cajero ConsultarJson RX: {"mensaje":"El token no es v\u00e1lido"}

**Resolución** Se ejecuto una prueba en entorno y se pide al usuario validar bien su flujo 

![[Pasted image 20250217103442.png]]