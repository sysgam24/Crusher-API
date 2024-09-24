# Crusher API Documentation

## Descripción General
La **Crusher API** permite obtener información sobre el stock de productos de Crusher. Está diseñada para facilitar el acceso a los datos sobre productos, disponibilidad y precios.

- **Versión:** 1.0.0
- **Servidor de Producción:** [https://api.farmapara.es](https://api.farmapara.es)

---

## Endpoints

### 1. Obtener Stock de Crusher

#### **GET /stock/crusher_stock**

Este endpoint devuelve la información del stock de Crusher según los filtros aplicados.

##### **Parámetros de consulta (Query Parameters):**
- `active` (boolean, opcional): Filtra productos activos.
  - Ejemplo: `?active=true`
- `last_updated_at` (string, opcional): Fecha de la última actualización del stock.
  - Formato: `date-time` (`YYYY-MM-DDTHH:MM:SS`).
  - Ejemplo: `?last_updated_at=2024-08-09T10:15:02`
- `page` (integer, opcional): Número de página para la paginación.
  - Ejemplo: `?page=2`
- `limit` (integer, opcional): Número de productos por página.
  - Ejemplo: `?limit=30`
- `all` (boolean, opcional): Incluye todos los productos, sin filtros.
  - Ejemplo: `?all=true`
- `product_id` (integer, opcional): ID de un producto específico.
  - Ejemplo: `?product_id=150526`

##### **Ejemplo de solicitud:**
GET https://api.farmapara.es/stock/crusher_stock?active=true&limit=30

##### **Respuestas:**

- **200 OK**: Devuelve los detalles del stock.
  - **Ejemplo de respuesta exitosa**:
    ```json
    {
      "data": [
        {
          "product_id": "150526",
          "ean": "1234567890123",
          "name": "Producto A",
          "brand_name": "Marca A",
          "stock": "50",
          "pvp": 12.99,
          "price_tax_excl": "10.74",
          "percentage_taxes": "21",
          "box_size": 10,
          "active": "true",
          "last_updated_at": "2024-08-09T10:15:02"
        }
      ],
      "pagination": {
        "current_page": 1,
        "total_pages": 5,
        "total_items": 150
      }
    }
    ```

- **400 Bad Request**: Parámetros inválidos en la solicitud.
  - **Ejemplo de respuesta**:
    ```json
    {
      "error": {
        "code": 400,
        "message": "Invalid parameters",
        "details": "The parameter 'limit' must be a positive integer."
      }
    }
    ```

- **404 Not Found**: No se encontraron productos que coincidan con los filtros.
  - **Ejemplo de respuesta**:
    ```json
    {
      "error": {
        "code": 404,
        "message": "No products found",
        "details": "No products were found for the specified filters."
      }
    }
    ```

- **500 Internal Server Error**: Error interno del servidor.
  - **Ejemplo de respuesta**:
    ```json
    {
      "error": {
        "code": 500,
        "message": "Internal server error",
        "details": "An unexpected error occurred."
      }
    }
    ```

---

### 2. Procesar una Orden de Productos

#### **POST /stock/process_order**

Este endpoint permite procesar una orden de productos de Crusher.

##### **Cuerpo de la Solicitud (Request Body):**
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
```
- **200 OK**:Orden procesada correctamente.
##### **Ejemplo de solicitud:**
POST https://api.farmapara.es/stock/process_order

 **Ejemplo de respuesta exitosa**:
 ```json
 {
  "message": "Order processed successfully."
}
```
- **422 Unprocessable Entity**: Entidad no procesable, error en los datos.
  - **Ejemplo de respuesta**:
```json
    {
  "error": {
    "code": 422,
    "message": "Unprocessable Entity",
    "details": "The quantity must be a positive integer."
  }
}
```
- **401 Unauthorized**: Error de autenticación.
  - **Ejemplo de respuesta**:
```json
    {
  "error": {
    "code": 401,
    "message": "Unauthorized",
    "details": "Invalid or missing authorization token."
  }
}
```
- **500 Internal Server Error**: Error interno del servidor.
  - **Ejemplo de respuesta**:
```json
    {
  "error": {
    "code": 500,
    "message": "Internal server error",
    "details": "An unexpected error occurred."
  }
}
```
### 3. Obtener Stockout

#### **GET /stock/stockout**

Este endpoint devuelve la información sobre los pedidos con stockout.

##### **Parámetros de consulta (Query Parameters):**
- `order_id` (string, opcional):  ID del pedido específico.
  - Ejemplo: `?order_id=3fa85f64-5717-4562-b3fc-2c963f66prueba`

##### **Ejemplo de solicitud:**
GET https://api.farmapara.es/stock/stockout

- **200 OK**: Devuelve la información de los pedidos con stockout.
 **Ejemplo de respuesta exitosa**:
 ```json
 {
  "ordersStockOut": [
    {
      "order_id": "3fa85f64-5717-4562-b3fc-2c963f66prueba",
      "external_order_id": "A17Z31JYF4",
      "client_id": "178e39f1-d74d-40ef-b714-289fbbd81aac",
      "order_date": "2024-08-09T10:15:02",
      "lines": [
        {
          "product_id": "0155d7e6-aa2b-4888-b09e-9c93d541f1ff",
          "quantity": 1,
          "stockout": 10
        }
      ]
    }
  ]
}
```
- **204 No Content**: No hay stockout disponible
  - **Ejemplo de respuesta**:
```json
    {
  "message": "No stockout available."
}
```
---

## Cómo visualizar la documentación completa
Para ver la especificación completa de la API en formato OpenAPI (YAML), sigue estos pasos:

1. Clona este repositorio.
2. Abre el archivo `openapi.yaml` con una herramienta como [Swagger Editor](https://editor.swagger.io/) para una vista más detallada e interactiva de los endpoints.

---

### Notas Adicionales:
- Recuerda que el servidor de producción solo debe ser utilizado con credenciales adecuadas.
- Los datos devueltos en la respuesta varían en función de los parámetros de consulta enviados en la solicitud.
- Asegúrate de incluir el token de autenticación en los encabezados de cada solicitud.
- La API está diseñada para manejar errores de manera clara, proporcionando detalles en las respuestas.
