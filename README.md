# DummyJSON API Tests

Este proyecto contiene pruebas automatizadas usando Postman para la API pública DummyJSON.  
Fue diseñado como práctica profesional para aplicar conceptos de QA técnico, buenas prácticas de documentación y uso básico-intermedio de Postman.

---

## 🔐 Autenticación

El primer paso realizado es la autenticación mediante `/auth/login`.  
Se capturan y guardan variables como el token de sesión y el ID del usuario para usarlas en futuras peticiones.

---

## 📁 Estructura

- `Collection/`: colección de Postman con los tests organizados por carpeta
- `Environment/`: variables de entorno necesarias para ejecutar la colección
- `Data/`: archivo `users.csv` para pruebas tipo Data Driven
- `README.md`: documentación general del proyecto

---


## 🚀 Cómo correr los tests

1. Cloná este repositorio o descargalo como ZIP.
2. Abrí Postman y hacé clic en `Import`.
3. Importá los archivos `.json` dentro de las carpetas `Collection` y `Environment`.
4. Seleccioná el environment correspondiente en la esquina superior derecha de Postman.
5. Ejecutá primero el request de login (dentro de la carpeta `Auth`) para autenticarte.
6. Luego podés correr el resto de los requests y tests incluidos.

---

## 🧪 Tests realizados

### 👤 `/users`
- Lectura de todos los usuarios (`GET`)
- Validación de propiedades clave
- Pruebas con búsqueda por nombre (válidas e inválidas)
- Comprobación de estructura, orden y casos vacíos

### 🔧 CRUD (Simulado)
- `POST`, `PUT`, `GET` y `DELETE` usando un ID preexistente
- Validaciones de status, cambios y consistencia

### 📊 Parámetros de búsqueda
- `sortBy` y `order`
- `filter` por propiedades anidadas
- `limit` y `skip` con extracción dinámica de query params
- `select` validando campos devueltos

### 🧪 Data Driven Testing
- Uso de archivo CSV con múltiples usuarios
- Validación de datos y tipos por iteración

---

