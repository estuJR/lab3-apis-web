# HTTPie Commands - DummyJSON API

## Variables de entorno

```bash
export BASE_URL="https://dummyjson.com"
export RESOURCE="products"
export SECOND_RESOURCE="users"
export ID="1"
export QUERY="phone"
```

---

## 1. Listar recurso principal (Products)

```bash
http GET $BASE_URL/$RESOURCE \
  Content-Type:application/json \
  Accept:application/json
```

## 2. Detalle por ID

```bash
http GET $BASE_URL/$RESOURCE/$ID \
  Content-Type:application/json \
  Accept:application/json
```

## 3. Búsqueda por query

```bash
http GET $BASE_URL/$RESOURCE/search \
  q==$QUERY \
  Content-Type:application/json \
  Accept:application/json
```

## 4. Filtro avanzado (ordenar por precio + seleccionar campos)

```bash
http GET $BASE_URL/$RESOURCE \
  sortBy==price \
  order==asc \
  select==title,price,rating \
  Content-Type:application/json \
  Accept:application/json
```

## 5. Paginación (limit + skip)

```bash
http GET $BASE_URL/$RESOURCE \
  limit==5 \
  skip==10 \
  Content-Type:application/json \
  Accept:application/json
```

## 6. Segundo recurso (Users)

```bash
http GET $BASE_URL/$SECOND_RESOURCE \
  limit==5 \
  Content-Type:application/json \
  Accept:application/json
```

## 7. Login - Autenticación correcta ✅

```bash
http POST $BASE_URL/auth/login \
  Content-Type:application/json \
  username=emilys \
  password=emilyspass \
  expiresInMins:=30
```

> **Nota:** Guardar el token devuelto para los siguientes comandos:
> ```bash
> export TOKEN="<accessToken_devuelto>"
> ```

## 8. Acceso autenticado - Obtener perfil

```bash
http GET $BASE_URL/auth/me \
  Authorization:"Bearer $TOKEN" \
  Content-Type:application/json \
  Accept:application/json
```

## 9. Autenticación fallida - Token inválido ❌

```bash
http GET $BASE_URL/auth/me \
  Authorization:"Bearer token_invalido_falso_123" \
  Content-Type:application/json \
  Accept:application/json
```

> **Resultado esperado:** `401 Unauthorized` o `403 Forbidden`

## 10. Error 404 - Recurso inexistente

```bash
http GET $BASE_URL/$RESOURCE/99999 \
  Content-Type:application/json \
  Accept:application/json
```

## 11. Error 400 - Login sin credenciales

```bash
http POST $BASE_URL/auth/login \
  Content-Type:application/json
```

## 12. Búsqueda en segundo recurso (Users)

```bash
http GET $BASE_URL/$SECOND_RESOURCE/search \
  q==John \
  Content-Type:application/json \
  Accept:application/json
```

---

## Resumen de comandos

| #  | Descripción                          | Método | Auth  | Status Esperado |
|----|--------------------------------------|--------|-------|-----------------|
| 1  | Listar productos                     | GET    | No    | 200             |
| 2  | Detalle por ID                       | GET    | No    | 200             |
| 3  | Búsqueda por query                   | GET    | No    | 200             |
| 4  | Filtro avanzado                      | GET    | No    | 200             |
| 5  | Paginación                           | GET    | No    | 200             |
| 6  | Segundo recurso (Users)              | GET    | No    | 200             |
| 7  | Login correcto                       | POST   | Creds | 200             |
| 8  | Acceso autenticado                   | GET    | JWT   | 200             |
| 9  | Auth fallida (token inválido)        | GET    | JWT   | 401/403         |
| 10 | Recurso inexistente                  | GET    | No    | 404             |
| 11 | Login sin credenciales               | POST   | No    | 400             |
| 12 | Búsqueda en segundo recurso          | GET    | No    | 200             |
