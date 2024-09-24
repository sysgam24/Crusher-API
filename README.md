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

## Cómo visualizar la documentación completa
Para ver la especificación completa de la API en formato OpenAPI (YAML), sigue estos pasos:

1. Clona este repositorio.
2. Abre el archivo `openapi.yaml` con una herramienta como [Swagger Editor](https://editor.swagger.io/) para una vista más detallada e interactiva de los endpoints.

---

### Notas Adicionales:
- Recuerda que el servidor de producción solo debe ser utilizado con credenciales adecuadas.
- Los datos devueltos en la respuesta varían en función de los parámetros de consulta enviados en la solicitud.
