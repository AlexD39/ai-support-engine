# AI Support Engine

Propuesta de un sistema para analizar solicitudes de soporte mediante dos enfoques comparables: un motor tradicional basado en reglas y un motor de inteligencia artificial generativa.

> **Estado del repositorio:** fase de documentación y diseño. En esta etapa todavía no se incluye una implementación ejecutable.

## Descripción general

AI Support Engine busca automatizar la primera revisión de mensajes enviados a una mesa de ayuda. A partir del texto de una solicitud, el sistema deberá identificar su categoría, asignar una prioridad, generar una respuesta y determinar si se requiere revisión humana.

Las categorías contempladas son:

- `ACCESS`: problemas de inicio de sesión, credenciales o permisos de acceso.
- `BUG`: errores, fallas o comportamientos inesperados del sistema.
- `FEATURE`: solicitudes de nuevas funciones o mejoras.
- `SECURITY`: vulnerabilidades, accesos sospechosos o incidentes de seguridad.

Las prioridades posibles son `LOW`, `MEDIUM`, `HIGH` y `CRITICAL`.

## Alcance propuesto

El proyecto contempla:

- Un motor tradicional que clasifique mediante reglas y entregue respuestas predeterminadas.
- Un motor generativo integrado con un modelo real, encargado de interpretar y responder de forma contextual.
- Un endpoint `POST /api/analyze` con los modos `traditional` y `generative`.
- Validación de entradas y salidas, manejo de errores y protección de secretos.
- Revisión humana obligatoria para posibles incidentes de seguridad.
- Una comparación de ambos motores con al menos diez solicitudes de prueba.

## Contrato previsto de la API

Petición:

```http
POST /api/analyze
Content-Type: application/json
```

```json
{
  "message": "No puedo iniciar sesión",
  "mode": "generative"
}
```

Respuesta:

```json
{
  "category": "ACCESS",
  "priority": "MEDIUM",
  "response": "Lamentamos el inconveniente. Verifique sus credenciales y utilice la recuperación de contraseña si es necesario.",
  "requiresHumanReview": false,
  "mode": "generative"
}
```

Ambos motores deberán respetar este mismo formato.

## Documentación

- [Stack tecnológico](docs/01-stack-tecnologico.md)
- [Objetivo](docs/02-objetivo.md)
- [Problemática](docs/03-problematica.md)

## Estructura documental

```text
ai-support-engine/
|-- README.md
`-- docs/
    |-- 01-stack-tecnologico.md
    |-- 02-objetivo.md
    `-- 03-problematica.md
```

## Estado y siguientes pasos

Esta entrega define el contexto, el objetivo y una propuesta de tecnologías. La instalación, las variables de entorno, los comandos de ejecución, las pruebas y las evidencias deberán agregarse al README cuando exista una implementación funcional, evitando documentar comandos o resultados que todavía no puedan verificarse.

## Equipo

El proyecto es desarrollado por:

- Lilia Hernández Tun
- Alejandro Contreras Martínez
- Oscar Landeta
