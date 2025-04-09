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

## ⚠️ Casos especiales y hallazgos funcionales

### Búsqueda vacía devuelve todos los usuarios

- **Endpoint probado:** `GET /users/search?q=`
- **Caso:** valor `"   "` (espacios en blanco)
- **Resultado:** status `200 OK` y devuelve la lista completa de usuarios
- **Comportamiento esperado:** array vacío.
- **Impacto:** lógica ambigua en frontend.
- ✅ Test automatizado incluido que detecta este comportamiento
