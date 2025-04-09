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

### 🔧 `/users – CRUD (Crear, Editar y Eliminar usuario)`
Esta sección contiene pruebas de operaciones básicas sobre usuarios usando los métodos POST, PUT y DELETE.

✅ POST /users/add
Crea un nuevo usuario con nombre, email y edad

Se valida:
Código de estado 200 OK
Que se devuelva un objeto con las propiedades id, firstName y email
Que los tipos de datos sean los correctos
El ID del nuevo usuario se guarda en una variable de colección para los pasos siguientes

🔄 PUT /users/:id
Actualiza el nombre y edad del usuario creado

Se valida:
Código de estado 200 OK
Que se reflejen correctamente los cambios en la respuesta
Que los campos actualizados sean correctos (firstName, age)

🗑️ DELETE /users/:id
Elimina el usuario previamente creado/modificado

Se valida:
Código de estado 200 OK
Que se devuelva una respuesta confirmando el id del usuario eliminado

## ⚠️ Nota sobre persistencia de datos
⚠️ DummyJSON no persiste los datos creados, modificados ni eliminados mediante POST, PUT o DELETE.
Es decir, las operaciones CRUD se simulan, pero no afectan realmente la base de datos.

### 🧪 En esta colección:

###Se usa un ID existente (12) para simular actualizaciones y eliminaciones


## ⚠️ Casos especiales y hallazgos funcionales

### Búsqueda vacía devuelve todos los usuarios

- **Endpoint probado:** `GET /users/search?q=`
- **Caso:** valor `"   "` (espacios en blanco)
- **Resultado:** status `200 OK` y devuelve la lista completa de usuarios
- **Comportamiento esperado:** array vacío.
- **Impacto:** lógica ambigua en frontend.
- ✅ Test automatizado incluido que detecta este comportamiento
