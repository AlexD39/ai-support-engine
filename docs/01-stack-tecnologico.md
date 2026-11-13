# Stack tecnológico

## Propósito

Este documento propone las tecnologías para implementar AI Support Engine. La selección prioriza una API sencilla, validación estricta, integración real con un modelo generativo y facilidad para ejecutar pruebas comparativas.

> Las herramientas descritas representan el stack recomendado; aún no implican que la aplicación esté implementada.

## Stack recomendado

| Área | Tecnología | Uso previsto |
| --- | --- | --- |
| Lenguaje | Python 3.11 o superior | Desarrollo de la API, los motores y las pruebas. |
| Framework de API | FastAPI | Exposición del endpoint `POST /api/analyze` y generación de documentación OpenAPI. |
| Servidor | Uvicorn | Ejecución local de la aplicación ASGI. |
| Validación | Pydantic | Validación de la petición y del contrato de respuesta. |
| Motor tradicional | Python: reglas, expresiones regulares y diccionarios | Clasificación determinista sin modelos generativos. |
| Motor generativo | Ollama | Ejecución local de un modelo generativo sin exponer credenciales externas. |
| Modelo sugerido | Llama 3.1 8B Instruct o equivalente compatible con Ollama | Clasificación y generación de respuestas estructuradas. |
| Cliente HTTP | HTTPX | Comunicación de la API con Ollama y control de tiempos de espera. |
| Pruebas | Pytest y TestClient de FastAPI | Pruebas unitarias, validación del endpoint y comparación de motores. |
| Configuración | Variables de entorno y archivo `.env` local | Configuración del proveedor sin incluir secretos en Git. |
| Control de versiones | Git | Seguimiento de cambios y colaboración del equipo. |

## Justificación de la selección

### Python y FastAPI

Python permite implementar reglas de clasificación y conectarse con proveedores de modelos utilizando un solo lenguaje. FastAPI facilita el desarrollo del endpoint requerido, mientras que Pydantic permite restringir los valores aceptados para categoría, prioridad y modo.

### Motor tradicional

El enfoque tradicional utilizará vocabularios, patrones y reglas de precedencia. Por ejemplo, términos relacionados con credenciales pueden orientar a `ACCESS`, mientras que referencias a filtraciones, vulnerabilidades o accesos no autorizados deben prevalecer como `SECURITY`. La prioridad se obtendrá a partir del impacto, la urgencia y los indicadores de riesgo.

Este motor deberá ser completamente determinista y no depender de un modelo generativo.

### Motor generativo con Ollama

Ollama permite integrar un modelo real de manera local. Esto reduce la dependencia de servicios externos y evita almacenar una clave de API. El modelo recibirá instrucciones para tratar el mensaje del usuario como datos no confiables y devolver únicamente una estructura JSON válida.

Si el equipo decide utilizar un proveedor en la nube, la arquitectura puede conservarse sustituyendo el adaptador de Ollama. En ese caso, la clave deberá suministrarse mediante una variable de entorno y nunca almacenarse en el repositorio.

## Arquitectura propuesta

```text
Cliente
  |
  v
POST /api/analyze
  |
  +-- Validación de entrada
  |
  +-- mode = traditional --> Motor de reglas
  |
  `-- mode = generative  --> Adaptador de Ollama --> Modelo local
                              |
                              `--> Validación del JSON generado
  |
  v
Respuesta con contrato común
```

## Componentes previstos

- **Capa de API:** recibe la solicitud, valida `message` y `mode`, y traduce los errores a respuestas HTTP apropiadas.
- **Motor tradicional:** aplica reglas de categoría, prioridad y respuesta predeterminada.
- **Motor generativo:** construye instrucciones seguras, consulta al modelo y procesa su respuesta.
- **Esquemas compartidos:** garantizan que ambos motores devuelvan los mismos campos y valores permitidos.
- **Capa de seguridad:** marca los casos `SECURITY` para revisión humana y limita el tamaño de los mensajes.
- **Suite comparativa:** ejecuta un conjunto previamente etiquetado y calcula la exactitud de cada motor.

## Validaciones y seguridad previstas

- Rechazar cuerpos vacíos, mensajes en blanco y modos diferentes de `traditional` o `generative`.
- Establecer una longitud máxima para `message`.
- Validar la salida del modelo contra el esquema común antes de responder.
- Manejar respuestas inválidas, indisponibilidad del modelo y tiempos de espera.
- No registrar claves, encabezados de autorización ni información sensible.
- Separar las instrucciones del sistema del contenido enviado por el usuario.
- Ignorar instrucciones incluidas en la solicitud que intenten alterar la clasificación.
- Forzar `requiresHumanReview: true` cuando la categoría final sea `SECURITY`.
- Utilizar una plantilla `.env.example` sin valores secretos si se integra un proveedor externo.

## Alternativa posible

Node.js con TypeScript, Express o Fastify, Zod, Vitest y el SDK del proveedor también cubriría los requisitos. Sin embargo, se recomienda mantener un solo stack durante el reto para reducir complejidad y facilitar que todos los integrantes puedan explicar la solución.
