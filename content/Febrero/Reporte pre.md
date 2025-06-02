---
Sistema: Pre-registro
Fecha_de_inicio: 2025-02-04T00:00:00.000-06:00
Tipo_de_solicitud: Reporte excel
Estatus: Finalizado
Nota: "[[1.Sistemas]]"
Fecha_termino: 2025-02-10T00:00:00.000-06:00
Mes: Febrero
---
![[Pasted image 20250204184444.png]]

Datos solicitados
![[Pasted image 20250204184506.png]]

---

**DBA** Debe generar el reporte a través de la base de datos de postgreSQL 

1. query (general)
```
select

public.contribuyentes_tramites.created_at as "Fecha de ingreso",

CASE WHEN public.ca_subtramites.id = 39 then

public.contribuyentes_tramites.tramite->4->>'value'

else

public.contribuyentes_tramites.tramite->3->>'value'

end

as "Número de Cuenta",

public.contribuyentes_tramites.tramite->1->>'value' as "Número de Notario",

public.ca_estatus.descripcion as "Estatus",

public.contribuyentes_tramites.folio as "Folio",

public.ca_tramites.descripcion as "Trámite",

public.ca_subtramites.descripcion as "Subtrámite",

CASE WHEN public.contribuyentes_tramites.segundo_apellido IS NULL then

public.contribuyentes_tramites.nombre || ' ' || public.contribuyentes_tramites.primer_apellido

ELSE

public.contribuyentes_tramites.nombre || ' ' || public.contribuyentes_tramites.primer_apellido || ' ' ||public.contribuyentes_tramites.segundo_apellido

END

as "Nombre Completo",

public.contribuyentes_tramites.rfc as "RFC",

public.contribuyentes_tramites.curp AS "CURP",

public.contribuyentes_tramites.celular as "Teléfono celular",

public.contribuyentes_tramites.telefono as "Teléfono fijo",

public.contribuyentes_tramites.email as "Correo electrónico",

public.contribuyentes_tramites.observaciones as "Observaciones"

FROM public.contribuyentes_tramites

JOIN public.ca_estatus ON public.ca_estatus.id = public.contribuyentes_tramites.id_estatus

JOIN public.ca_subtramites ON public.ca_subtramites.id = public.contribuyentes_tramites.id_subtramite

JOIN public.ca_tramites ON public.ca_tramites.id = public.ca_subtramites.id_tramite

WHERE public.ca_tramites.id IN(8,9,10,14)

and public.contribuyentes_tramites.created_at between '2024-01-01 00:00:00' AND '2024-01-31 00:00:00'

ORDER BY public.ca_tramites.descripcion asc, public.ca_subtramites.descripcion;
```

> Nota. Este query es para un reporte general en caso de que se requiera tramites específicos se debe ajustar el query después de los JOINs

```
WHERE public.ca_tramites.id IN(8,9,10,14)

and public.contribuyentes_tramites.created_at between '2024-01-01 00:00:00' AND '2024-01-31 00:00:00'
```


 2. Solicitar el repositorio para poder cargar el reporte en formato excel o csv
 3. Subir el reporte 
 4. Regresar el link del repositorio para generar la liga de envió que tendrá una vigencia de días naturales y acceso a través de contraseña.
 5. Generar codigo qr (https://www.codigos-qr.com/generador-de-codigos-qr/)
 6. Oficio de respuesta con código qr


- [x] Reporte anual (2024)
![[qrcode_reporte_anual_2024.png]]
link: https://tics.finanzas.cdmx.gob.mx/repositorio/index.php/s/TxJ2ZDp8aDritZy


- [x] Reporte de enero | Fecha de entrega: 05/02/2025
![[qrcode_reporte_enero_2025.png]]
link: https://tics.finanzas.cdmx.gob.mx/repositorio/index.php/s/L3RanH5fTimH9HX


---
7. Una vez que el oficio salga mandar la contraseña por correo electrónico

![[Pasted image 20250326124310.png]]

