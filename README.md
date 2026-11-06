# Diseño y desarrollo de servicios web · Caso AA5-EV01

Laura Tatiana Ciro Botero · SENA · Ficha 3235898 · 2026.

## Qué entrega este caso

API Java/Spring Boot para registrar un usuario y comprobar su usuario/contraseña.
Devuelve el mensaje de autenticación satisfactoria o error solicitado por la guía 7,
página 14. Incluye código comentado, validaciones, pruebas y una pantalla de práctica.
Este caso **comprueba credenciales**: no genera tokens, sesiones ni permisos para los
módulos de Recursos Humanos. Es independiente del proyecto completo AA4.

## Ejecutar

Requisitos: Java 17+ y Maven 3.9+. Desde esta carpeta:

```sh
mvn spring-boot:run
```

Abrir http://127.0.0.1:8080. No hay usuarios precargados: registrar primero una cuenta
ficticia. H2 guarda las cuentas en `data`; reiniciar no las elimina. El servidor se
limita a la interfaz local 127.0.0.1 para la práctica.

Para compilar: `mvn clean verify`. Después: `java -jar target/servicios-registro-login-1.0.0.jar`.
No es necesario Node para usar la aplicación. Node 22 se usa únicamente para las pruebas de navegador.

## Servicios

- POST `/api/registro`: recibe JSON `usuario` y `contrasena`; devuelve HTTP 201.
- POST `/api/login`: mismo JSON; HTTP 200 si coincide, HTTP 401 en caso contrario.
- GET `/api/salud`: comprobación auxiliar de disponibilidad.

Ejemplo ficticio:

```json
{"usuario":"tatiana.demo","contrasena":"Practica2026!"}
```

Las respuestas nunca incluyen contraseña ni hash. Se utiliza BCrypt con coste 12.
Usuario: 3-30 letras ASCII, números, puntos, guiones o guiones bajos. Se eliminan
espacios exteriores y se convierte a minúsculas. Contraseña: 8-64 caracteres,
máximo 72 bytes UTF-8; sus espacios y mayúsculas se conservan.

Errores: 400 datos/JSON inválidos; 401 credenciales incorrectas; 409 usuario duplicado.
El mensaje de credenciales incorrectas es igual si el usuario no existe.

## Verificación

`mvn test` ejecuta 15 pruebas de integración con base H2 en memoria, aislada.
Con una API de práctica nueva iniciada: `npm ci`, `npx playwright install chromium`
y `npm run test:e2e`. La prueba de navegador registra `prueba.navegador`.
Para repetirla, utilizar una base de pruebas nueva; no borrar una base con datos útiles.
El flujo GitHub Actions usa memoria y no modifica la base de trabajo.

## Archivos

- `src/main/java`: contratos, controlador, servicio y manejo de errores.
- `src/main/resources`: configuración, esquema y pantalla de práctica.
- `src/test`: pruebas de integración.
- `tests`: prueba del navegador real contra Spring Boot.
- `docs`: contrato y ejemplos HTTP.
- `.github/workflows`: compilación y verificaciones automáticas.

Git conserva las versiones; GitHub publica el código y los resultados de Actions.
Para usarlo con datos reales se necesitarían HTTPS, control de intentos, recuperación
de cuentas y gestión completa de sesiones/permisos; no son funcionalidades de este caso.

Repositorio: https://github.com/tatianabotero314-del/sena-ga7-aa5-ev01-servicios
