---
Sistema: OVICA
Fecha_de_inicio: 2025-02-17T00:00:00.000-06:00
Tipo_de_solicitud: bug/fix
Estatus: Finalizado
Producción: true
Fecha_termino: 2025-02-17T00:00:00.000-06:00
Nota: "[[1.Sistemas]]"
Mes: Febrero
---
---

Error en referencia de pago en área de Lalo coordinador, las referencias de pago de cada proceso están saliendo duplicadas o muy parecidas

**Resolución**

al valor de *reference* se cambiara por fecha usando epoch para darle un valor numérico a la fecha 

```
const date = Math.floor(Date.now() / 1000);  
const new_reference = date.toString().padStart(20,'0');  
  
const payload = {  
  account_number: environment.santanderAccount,  
  applicant_name: '',  
  capture_line: this.pago.linea_captura,  
  //reference: this.pago.id_pago.padStart(20, '0'),  
  reference: new_reference,  
  channel: '06',  
  id_bank: environment.santanderIdBank,  
  concept: this.display,  
  amount: importe + '.00',  
  url_success: environment.success,  
  url_failure: environment.fail,  
  phone: this.telefono,  
  email: this.email,  
  additional_fields: '',  
};
```

