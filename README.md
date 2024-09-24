# Documentación de la API de Crusher

Esta es la API de Crusher para obtener información sobre el stock de productos y procesar órdenes.

## Base URL

La URL base para acceder a la API es:
https://api.farmapara.es
## Autenticación

La API utiliza un token de autenticación Bearer. Asegúrate de incluir el token en el encabezado `Authorization` de cada solicitud.

Ejemplo de encabezado:
Authorization: Bearer tu_token_aqui
## Endpoints

### 1. Obtener stock de Crusher

**Método:** `GET`

**Endpoint:** `/stock/crusher_stock`

**Descripción:** Devuelve la información de stock para Crusher según los filtros aplicados.

#### Parámetros de Consulta

- **active** (boolean): Filtro para obtener solo productos activos.
- **last_updated_at** (string, formato: date-time): Fecha de última actualización.
- **page** (integer): Número de página para la paginación.
- **limit** (integer): Número de productos por página.
- **all** (boolean): Incluir todos los productos.
- **product_id** (integer): ID del producto específico.

#### Respuestas

- **200 OK**: Respuesta exitosa, devuelve el stock de Crusher.
- **400 Bad Request**: Solicitud incorrecta, parámetros inválidos.
- **404 Not Found**: No se encontraron datos que coincidan con los filtros especificados.
- **500 Internal Server Error**: Error interno del servidor.

### 2. Procesar una orden de productos

**Método:** `POST`

**Endpoint:** `/stock/process_order`

**Descripción:** Endpoint para procesar una orden de productos de Crusher.

#### Cuerpo de la Solicitud

```json
{
  "orders": [
    {
      "order_id": "3fa85f64-5717-4562-b3fc-2c963f66prueba",
      "external_order_id": "A17Z31JYF4",
      "client_id": "178e39f1-d74d-40ef-b714-289fbbd81aac",
      "lines": [
        {
          "product_id": "0155d7e6-aa2b-4888-b09e-9c93d541f1ff",
          "quantity": 1
        }
      ]
    }
  ]
}
---
Respuestas
200 OK: Orden procesada correctamente.
422 Unprocessable Entity: Entidad no procesable, error en los datos.
401 Unauthorized: Error de autenticación.
500 Internal Server Error: Error interno del servidor.
