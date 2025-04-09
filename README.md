# DummyJSON API Tests

Este proyecto contiene pruebas automatizadas usando Postman para la API pública DummyJSON.

## 🔐 Autenticación

El primer paso realizado es la autenticación mediante `/auth/login`.  
Se capturan y guardan variables como el token de sesión y el ID del usuario para usarlas en futuras peticiones.

## 📁 Estructura

- `Collection/`: contiene la colección de Postman.
- `Environment/`: variables de entorno utilizadas en la colección.

## ✅ Autenticación

- Usuario: `emmaj`
- Contraseña: `emmajpass`

## 🚀 Cómo correr los tests

1. Cloná este repositorio o descargalo como ZIP.
2. Abrí Postman y hacé clic en `Import`.
3. Importá los archivos `.json` dentro de las carpetas `Collection` y `Environment`.
4. Seleccioná el environment correspondiente en la esquina superior derecha de Postman.
5. Ejecutá primero el request de login (dentro de la carpeta `Auth`) para autenticarte.
6. Luego podés correr el resto de los requests y tests incluidos.

## 🧪 Testeos realizados

### 👤 `/users`
- ✅ Valida status code 200 OK
- ⏱️ Valida que la respuesta sea menor a 1 segundo
- 📋 Verifica que la lista de usuarios no esté vacía
- 🔎 Valida que cada usuario tenga:
  - id (number)
  - username (string)
  - address.country (string)
  - company.title (string)
  - company.address.country (string)
  - role (string)

### 🔍 `/users/search`
- 🧠 Tests dinámicos por nombre usando variables de colección
- ✅ Valida coincidencia exacta con `firstName`
- 🧩 Valida estructura completa del response
- 🔐 Valida comportamiento cuando no hay resultados esperados
- 📊 Verifica que los usuarios estén ordenados por `id` ascendente (si hay más de uno)
- 🧪 Casos de prueba negativos con inputs inválidos (`123`, `!@#$`, espacios, etc.)

### 🔧 `/users` – POST / PUT / DELETE

Esta sección contiene pruebas de operaciones básicas sobre usuarios usando los métodos `POST`, `PUT` y `DELETE`.

#### ✅ `POST /users/add`
- Crea un nuevo usuario con `firstName`, `lastName`, `age` y `email`
- Se valida:
  - Código de estado `201 Created`
  - Que el objeto devuelto tenga las propiedades `id`, `firstName`,`lastName`, `age`, `email` con sus valores correspondientes
- El `id` del usuario creado se guarda en una variable de colección para los pasos siguientes

#### 🔄 `PUT /users/:id`
- Actualiza el nombre del usuario creado
- Se valida:
  - Código de estado `200 OK`
  - Que el objeto de respuesta no este vacío
  - Que el cambio reflejados coincidan con el dato enviado (`firstName`)
 
#### 🔄 `GET /users/:id`
- Busca por id al usuario actualizado
- Se valida:
  - Código de estado `200 OK`
  - Que el objeto de respuesta no este vacío
  - Que el id de respuesta coincidan con el id enviado


#### 🗑️ `DELETE /users/:id`
- Elimina el usuario previamente creado
- Se valida:
  - Código de estado `200 OK`
  - Que la respuesta incluya el `id` del usuario eliminado


### 🔽 `/users` con ordenamiento

- Endpoint probado: `/users?sortBy=age&order=desc`
- Se valida:
  - Código de estado `200 OK`
  - Que los usuarios estén ordenados por edad de forma descendente


### 🎯 `/users/filter` con parámetros personalizados

- Endpoint probado: `/users/filter?key=address.city&value=Chicago`
- Se valida:
  - Código de estado `200 OK`
  - Que todos los usuarios devueltos tengan como ciudad `"Chicago"` (`user.address.city`)


### 📄 `/users` con paginación (`limit` y `skip`)

- Endpoint probado: `/users?limit=5&skip=10`
- Se valida:
  - Código de estado `200 OK`
  - Que se devuelvan exactamente la cantidad de usuarios pasados en el parametro 'limit'.
  - Que los valores de `limit` y `skip` coincidan con los enviados
  - Que el total de usuarios sea mayor al `skip`

- 💡 Se accede dinámicamente a los valores de los query params
  
---

### 🧩 `/users` con campos seleccionados (`select`)

- Endpoint probado: `/users?select=username,email`
- Se valida:
  - Código de estado `200 OK`
  - Que cada usuario devuelto contenga **solo** las propiedades `username` y `email`
  - Que **no** se incluyan otras propiedades como `lastName`, `age`, `address`, `company`, etc.

---

⚠️ **Nota sobre persistencia de datos**

DummyJSON **no guarda los cambios realizados mediante `POST`, `PUT` o `DELETE`**.  
Las respuestas se simulan, pero **los datos no se modifican realmente en el servidor**.

🔧 Por eso, en esta colección:
- Se usa un ID real (`12`) para simular la edición y eliminación
- Se validan las respuestas como si fueran reales

## ⚠️ Casos especiales y hallazgos funcionales

### Búsqueda vacía devuelve todos los usuarios

- **Endpoint probado:** `GET /users/search?q=`
- **Caso:** valor `"   "` (espacios en blanco)
- **Resultado:** status `200 OK` y devuelve la lista completa de usuarios
- **Comportamiento esperado:** array vacío.
- **Impacto:** lógica ambigua en frontend.
- ✅ Test automatizado incluido que detecta este comportamiento
