# Control Yaxché: actividades, asistencia y bitácora de producción

Aplicación web interna para el equipo de **Yaxché México**, laboratorio que elabora suplementos alimenticios. Reúne en un solo lugar la organización del trabajo, la asistencia del personal con el checador biométrico de la empresa y la bitácora de los cuartos de producción. Se instala en el celular como una app.

> **Nota:** el código fuente es privado porque pertenece al cliente. Este repositorio documenta el proyecto: qué hace, cómo está construido y las decisiones técnicas que tomé.

---

## Contexto

Corresponde a la **optimización de procesos administrativos** de mi Residencia Profesional de Ingeniería en Sistemas Computacionales, TecNM Campus Acapulco (agosto–diciembre 2026). Desarrollé la aplicación completa de forma individual.

---

## Funcionalidades

### Actividades y calendario
- Actividades divididas en pasos y asignadas a una o varias personas.
- El estado (pendiente, en proceso, terminada) se actualiza solo según el avance de los pasos.
- Conversación dentro de cada actividad, con archivos adjuntos y mensajes automáticos cuando algo cambia.
- Calendario del equipo y búsqueda por actividad o paso.
- Avisos para todo el equipo.

### Notificaciones
- Notificaciones dentro de la app y **notificaciones push al celular**, aunque la app esté cerrada.
- Web Push con claves VAPID y el cifrado de los mensajes implementado en PHP con OpenSSL, sin librerías externas.

### Asistencia con checador biométrico
- **Integración con el checador Hikvision de la empresa:** el equipo envía cada checada al sistema en tiempo real (HTTP Listening), protegido con una clave.
- Cada persona se liga con su ID del checador.
- Horario general y horarios por persona.
- Presencia del día y reporte de asistencia con retardos y faltas.
- Bitácora de lo que envía el checador, con botón de prueba de conexión.

### Áreas y cuartos de producción
- Registro de entrada y salida de cada persona en cada cuarto, con la actividad, el equipo usado y la producción (producto, lote y cantidad).
- Vista de quién está adentro en este momento.
- Archivos del día por cuarto.
- **Bitácora descargable en Excel**, con hoja de resumen, generada con un creador de archivos `.xlsx` propio, sin librerías.

### Usuarios y perfil
- Roles con permisos distintos: administrador general, administrador y empleado.
- Cada persona puede ver su propia asistencia, cambiar su contraseña y registrar un correo de recuperación.

### App instalable (PWA)
- Se instala en Android y en iPhone desde el navegador, sin pasar por una tienda de apps.
- Service Worker con página sin conexión y accesos directos a "Mis actividades" y "Avisos".

---

## Seguridad

- Contraseñas con `password_hash` y bloqueo tras varios intentos fallidos de inicio de sesión.
- Sesiones "recordarme" con tokens propios y recuperación de contraseña por correo.
- Tokens CSRF y consultas con PDO y sentencias preparadas.
- Los archivos subidos no se pueden abrir directamente: solo se entregan a través de un script que valida la sesión.
- Carpetas internas bloqueadas desde la web y HTTPS forzado.
- El receptor del checador valida su clave con comparación segura (`hash_equals`) y registra los intentos rechazados.

---

## Retos y decisiones técnicas

- **Integrar un checador comercial sin SDK.** El checador manda sus eventos por HTTP en un formato propio. El receptor reconoce los eventos, ignora los latidos de conexión para no llenar la bitácora y responde rápido. Si algo falla al guardar, contesta con error para que el equipo reintente y no se pierda ninguna checada.
- **Notificaciones push en hosting compartido.** Implementé el protocolo Web Push (VAPID, ECDH y AES-128-GCM) directamente con las funciones de OpenSSL de PHP, sin instalar dependencias.
- **Excel sin librerías.** La bitácora se genera como un `.xlsx` real, con estilos, encabezados fijos y hoja de resumen.
- **Una app sin tienda de apps.** Como PWA, el equipo la instala desde el navegador en su iPhone o Android y recibe notificaciones como cualquier app.

---

## Tecnologías

| Área | Tecnología |
|---|---|
| Backend | PHP 8, PDO |
| Base de datos | MySQL |
| Frontend | HTML, CSS y JavaScript sin frameworks |
| App | PWA: Service Worker, Web App Manifest y Web Push |
| Integraciones | Checador biométrico Hikvision (HTTP Listening) |
| Reportes | Generador de Excel (`.xlsx`) propio |
| Hosting | Hostinger |

---

## Mi rol

Desarrollador full-stack del proyecto completo:

- Diseño de la base de datos y de los permisos por rol.
- Módulo de actividades, calendario, avisos y conversaciones.
- Integración con el checador Hikvision y reportes de asistencia.
- Bitácora de cuartos de producción y exportación a Excel.
- Notificaciones push y conversión a app instalable (PWA).
- Puesta en producción en Hostinger.

---

## Contacto

- GitHub: [@ZzLeoG](https://github.com/ZzLeoG)
- Correo: [LeonardoGXD@gmail.com](mailto:LeonardoGXD@gmail.com)
