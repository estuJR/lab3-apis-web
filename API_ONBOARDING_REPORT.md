# API Onboarding Report - DummyJSON

## Resumen de la API

| Campo         | Detalle                                              |
|---------------|------------------------------------------------------|
| **Nombre**    | DummyJSON                                            |
| **Base URL**  | `https://dummyjson.com`                              |
| **Tipo de Auth** | JWT Bearer Token (vía `/auth/login`)              |
| **Rate Limit** | No documentado oficialmente (uso libre sin API key) |
| **Descripción** | REST API gratuita con datos ficticios para prototyping y testing de aplicaciones frontend |

---

## Tabla de Endpoints

| # | Método | URL | Query Params | Headers | Status Esperado | Status Obtenido |
|---|--------|-----|--------------|---------|-----------------|-----------------|
| 1 | GET | `{{baseURL}}/products` | — | `Content-Type: application/json` | 200 OK | 200 OK |
| 2 | GET | `{{baseURL}}/products/1` | — | `Content-Type: application/json` | 200 OK | 200 OK |
| 3 | GET | `{{baseURL}}/products/search` | `q=phone` | `Content-Type: application/json` | 200 OK | 200 OK |
| 4 | GET | `{{baseURL}}/products` | `sortBy=price&order=asc&select=title,price,rating` | `Content-Type: application/json` | 200 OK | 200 OK |
| 5 | GET | `{{baseURL}}/products` | `limit=5&skip=10` | `Content-Type: application/json` | 200 OK | 200 OK |
| 6 | GET | `{{baseURL}}/users` | — | `Content-Type: application/json` | 200 OK | 200 OK |
| 7 | POST | `{{baseURL}}/auth/login` | — | `Content-Type: application/json` | 200 OK | 200 OK |
| 8 | GET | `{{baseURL}}/auth/me` | — | `Authorization: Bearer {{token}}` | 200 OK | 200 OK |
| 9 | GET | `{{baseURL}}/products/99999` | — | `Content-Type: application/json` | 404 Not Found | 404 Not Found |
| 10 | POST | `{{baseURL}}/auth/login` | — | `Content-Type: application/json` (body vacío) | 400 Bad Request | 400 Bad Request |
| 11 | GET | `{{baseURL}}/auth/products` | — | Sin Authorization header | 401 Unauthorized | 401 Unauthorized |
| 12 | GET | `{{baseURL}}/auth/products` | — | `Authorization: Bearer token_invalido` | 401/403 Forbidden | 403 Forbidden |

---

## Evidencia de Respuestas

### ✅ Respuesta Exitosa #1 — Listar Productos

**Request:** `GET https://dummyjson.com/products?limit=2`

**Response (200 OK):**

```json
{
  "products": [
    {
      "id": 1,
      "title": "Essence Mascara Lash Princess",
      "category": "beauty",
      "price": 9.99,
      "rating": 4.94
    },
    {
      "id": 2,
      "title": "Eyeshadow Palette with Mirror",
      "category": "beauty",
      "price": 19.99,
      "rating": 3.28
    }
  ],
  "total": 194,
  "skip": 0,
  "limit": 2
}
```

---

### ✅ Respuesta Exitosa #2 — Búsqueda de Productos

**Request:** `GET https://dummyjson.com/products/search?q=laptop`

**Response (200 OK):**

```json
{
  "products": [
    {
      "id": 7,
      "title": "Apple MacBook Pro 14 Inch Space Grey",
      "category": "laptops",
      "price": 1999.99,
      "rating": 2.43
    }
  ],
  "total": 3,
  "skip": 0,
  "limit": 3
}
```

---

### ✅ Respuesta Exitosa #3 — Login exitoso

**Request:** `POST https://dummyjson.com/auth/login`

**Body:**
```json
{
  "username": "emilys",
  "password": "emilyspass",
  "expiresInMins": 30
}
```

**Response (200 OK):**

```json
{
  "id": 1,
  "username": "emilys",
  "email": "emily.johnson@x.dummyjson.com",
  "firstName": "Emily",
  "lastName": "Johnson",
  "gender": "female",
  "image": "https://dummyjson.com/icon/emilys/128",
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### ❌ Respuesta Fallida #1 — 404 Not Found

**Request:** `GET https://dummyjson.com/products/99999`

**Response (404 Not Found):**

```json
{
  "message": "Product with id '99999' not found"
}
```

---

### ❌ Respuesta Fallida #2 — 401 Unauthorized (Sin token)

**Request:** `GET https://dummyjson.com/auth/me`

**Headers:** Sin Authorization header

**Response (401 Unauthorized):**

```json
{
  "name": "JsonWebTokenError",
  "message": "Invalid/Expired Token!"
}
```

---

## Notas adicionales

- **Recursos disponibles:** products, carts, users, posts, comments, quotes, recipes, todos
- **Autenticación:** Se obtiene un JWT token vía `POST /auth/login` con credenciales de cualquier usuario de `/users`. El token se envía como `Bearer` en el header `Authorization`.
- **Paginación:** Se usa `limit` y `skip` como query params. Por defecto devuelve 30 items. Usar `limit=0` devuelve todos.
- **Búsqueda:** Endpoint `/search?q=texto` disponible en todos los recursos principales.
- **Filtros:** Soporta `sortBy`, `order` (asc/desc), y `select` para elegir campos específicos.
