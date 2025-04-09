# Documentación de Script de Pruebas para Postman y K6

## Descripción General

Este repositorio contiene un script de pruebas que puede ser utilizado tanto en Postman como en K6 para probar una API REST. El script está diseñado para validar múltiples operaciones CRUD (Create, Read, Update, Delete) contra la API de demostración JSONPlaceholder.

## Características

- **Pruebas para cuatro métodos HTTP**: GET, POST, PUT y DELETE
- **Validaciones automáticas** de códigos de respuesta, tiempos de respuesta y estructura de datos
- **Manejo robusto de errores** para diferentes escenarios como recursos no encontrados (404)
- **Capacidad de exportar** pruebas de Postman a K6 para pruebas de carga
- **Registro detallado** para facilitar la depuración de problemas

## Requisitos Previos

- [Postman](https://www.postman.com/downloads/) para pruebas interactivas
- [K6](https://k6.io/docs/getting-started/installation/) para pruebas de carga/rendimiento
- Conocimientos básicos de APIs REST y JavaScript

## Estructura del Script

El script incluye cuatro solicitudes principales:

1. **GET** a `/posts` - Verifica la obtención de la lista de posts
2. **POST** a `/posts` - Prueba la creación de nuevos posts
3. **PUT** a `/posts` - Valida la actualización de posts existentes
4. **DELETE** a `/posts` - Comprueba la eliminación de posts

## Validaciones Implementadas

### GET
- Código de estado 200
- Tiempo de respuesta inferior a 200ms
- Estructura correcta de los datos recibidos (userId, id, title, body)
- Validación de tipos de datos (números no negativos, cadenas no vacías)

### POST
- Código de estado 201
- Tiempo de respuesta inferior a 500ms
- Verificación de la asignación de ID
- Validación del encabezado Content-Type

### PUT
- Validación de códigos de estado (200, 201, 204 o 404)
- Manejo adecuado de recursos no encontrados
- Verificación de la estructura de datos en respuestas exitosas
- Control de tiempo de respuesta razonable

### DELETE
- Validación de códigos de estado (200, 202, 204 o 404)
- Verificación de respuestas vacías para 204
- Validación de mensajes de confirmación en respuestas con contenido
- Registro de URLs para verificación posterior

## Uso en Postman

1. Importa el script a tu colección de Postman
2. Asegúrate de actualizar las URLs si es necesario
3. Ejecuta las pruebas individualmente o como parte de una colección

## Uso en K6

El script ha sido generado usando el conversor postman-to-k6 y está listo para ser ejecutado con K6:

```bash
k6 run script.js
```

## Personalización

Para adaptar este script a tu propia API:

1. Modifica las URLs para que apunten a tu API
2. Ajusta las validaciones según la estructura de datos esperada
3. Actualiza los tiempos de respuesta aceptables según tu entorno

## Buenas Prácticas Implementadas

- Uso de `pm.test()` para pruebas bien definidas
- Manejo adecuado de diferentes códigos de respuesta
- Bloques try-catch para manejar respuestas inesperadas
- Verificación completa de tipos de datos y estructura
- Registro detallado para facilitar la depuración

## Limitaciones

- El script está configurado principalmente para la API JSONPlaceholder
- Las pruebas PUT y DELETE requieren IDs válidos para funcionar correctamente
- Se asume que la API sigue estándares REST convencionales

## Contribución

Siéntete libre de contribuir a este proyecto mediante pull requests o reportando problemas en el sistema de issues.

## Licencia

[MIT](LICENSE)
