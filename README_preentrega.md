# Pre-entrega – Notificación horaria automática (New York)

## Nombre del caso
Notificación horaria automática según franja del día en New York.

## Importante: Las credenciales de Gmail se encuentran configuradas únicamente en el gestor de credenciales de n8n y no se incluyen en el archivo exportado. 

## Descripción del workflow y nodos
1. **Schedule Trigger**  
   El workflow se dispara automáticamente mediante un **Schedule Trigger**, configurado para ejecutarse cada 59 minutos.

2. **HTTP Request**  
   Consulta la API pública `worldtimeapi.org` para obtener la fecha y hora actual de la zona horaria America/New_York. (Tener en cuenta que la appi a veces se satura y hay que correrla varias veces para que no de error).

3. **Edit Fields**  
   Se normalizan los datos recibidos de la API, creando los campos `current_hour` y `time_period` (mañana, tarde o noche) para su reutilización en el flujo.

4. **Switch**  
   Evalúa el campo `time_period` y dirige el flujo por diferentes ramas según la franja horaria detectada.

5. **Edit Fields (Morning / Afternoon / Night)**  
   En cada rama se construye dinámicamente el contenido del mensaje de notificación (`email_body`) según el momento del día.
El nodo Switch evalúa el valor del campo `time_period`, permitiendo ejecutar distintas acciones según si la hora en New York corresponde a mañana, tarde o noche.

6. **Gmail – Send a message**  
   Envía una notificación por email utilizando el contenido generado en la rama correspondiente.
La notificación se envía por email mediante Gmail. El mensaje se genera dinámicamente en nodos previos y el nodo Gmail solo se encarga del envío.

## Buenas prácticas aplicadas
- No se exponen credenciales en el workflow exportado.
- Uso de API pública sin autenticación para simplificar la integración.
- Separación de lógica (condiciones) y presentación (mensaje).
- Uso del Execution Log de n8n para validación del funcionamiento.