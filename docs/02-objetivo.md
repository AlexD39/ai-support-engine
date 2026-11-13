# Objetivo

## Objetivo general

Diseñar y desarrollar un prototipo funcional capaz de analizar solicitudes de soporte mediante dos enfoques de inteligencia artificial —uno tradicional basado en reglas y otro generativo— para comparar su capacidad de clasificar mensajes, asignar prioridades y producir respuestas útiles bajo un contrato de salida común.

## Objetivos específicos

1. Clasificar cada solicitud en una de cuatro categorías: `ACCESS`, `BUG`, `FEATURE` o `SECURITY`.
2. Asignar una prioridad `LOW`, `MEDIUM`, `HIGH` o `CRITICAL` según la urgencia, el impacto y el riesgo descritos.
3. Implementar un motor tradicional determinista que utilice reglas, condiciones o patrones y proporcione una respuesta predeterminada.
4. Integrar un modelo generativo real que interprete el contexto y produzca una respuesta profesional en formato JSON.
5. Exponer ambos motores mediante el endpoint `POST /api/analyze`, seleccionándolos con el atributo `mode`.
6. Validar las entradas y las salidas para asegurar que los dos enfoques respeten el mismo contrato.
7. Manejar fallos del proveedor generativo sin exponer secretos ni devolver respuestas no verificadas.
8. Marcar para revisión humana toda solicitud relacionada con un posible incidente de seguridad.
9. Comparar los motores con al menos diez casos previamente clasificados, incluyendo mensajes normales, ambiguos y adversariales.
10. Calcular y documentar el porcentaje de clasificaciones correctas de cada enfoque.

## Resultado esperado

El resultado final deberá ser un repositorio ejecutable y verificable que permita enviar una misma solicitud a cualquiera de los dos motores y recibir una respuesta con la siguiente información:

- Categoría detectada.
- Prioridad asignada.
- Respuesta para el usuario.
- Indicador de revisión humana.
- Modo utilizado.

La comparación deberá mostrar de manera cuantitativa cuál motor clasificó correctamente más casos con respecto a una referencia definida antes de ejecutar las pruebas.

## Criterios de éxito

Se considerará que el prototipo cumple su objetivo cuando:

- Los modos `traditional` y `generative` estén disponibles en el mismo endpoint.
- Las respuestas de ambos motores cumplan el esquema JSON acordado.
- El motor generativo utilice una integración real y no respuestas escritas manualmente.
- Las entradas inválidas y los fallos del modelo se manejen de forma controlada.
- Los casos de seguridad siempre requieran revisión humana.
- Exista evidencia reproducible de al menos diez pruebas comparativas.
- La documentación permita instalar, configurar, ejecutar y evaluar el proyecto.

## Fuera de alcance

Para el prototipo no se contempla:

- Reemplazar por completo al personal de soporte.
- Resolver automáticamente incidentes de seguridad.
- Entrenar un modelo de lenguaje desde cero.
- Desplegar una solución de producción con alta disponibilidad.
- Integrarse con sistemas empresariales de tickets, identidad o monitoreo.
- Almacenar historiales de usuarios o información sensible.

Estas capacidades podrían evaluarse en versiones posteriores, una vez validado el funcionamiento básico.
