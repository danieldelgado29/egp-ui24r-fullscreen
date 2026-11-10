# EGP Ui24R — Fullscreen PWA V0.1

Prototipo para iPhone 15 Pro Max y Soundcraft Ui24R (`10.10.10.10`). **Control de consola no comprobado.**

## Publicación

https://danieldelgado29.github.io/egp-ui24r-fullscreen/

La versión de Pages es una carcasa instalable y una demostración de encuadre, sin conexión a la mezcla. No cumple todavía el control real de la consola.

## Instalación en iPhone

1. Abre el enlace anterior en Safari.
2. Compartir → Añadir a pantalla de inicio. Activa «Abrir como app web» si aparece.
3. Abre EGP Ui24R desde su icono y gira el iPhone horizontalmente.
4. AJUSTAR encuadra la vista; + amplía; MOVER activa desplazamiento y pellizco; LISTO devuelve el modo de control; OCULTAR esconde la barra y ⚙ EGP la recupera.

El botón − del original tiene un error: también amplía. Se conserva para no alterar funcionalidades del prototipo; usa AJUSTAR o pellizco en MOVER para reducir.

La apertura standalone está declarada en el manifest y las etiquetas Apple, pero no se ha verificado en un iPhone físico. No garantiza ocultar todos los elementos del sistema iOS.

## Por qué Pages no controla directamente la Ui24R

El original carga `/` dentro de un iframe y presupone que ahí existe un proxy de la consola. En github.io esa ruta no es la Ui24R. Pages solo sirve archivos estáticos: no puede ejecutar ese proxy ni alcanzar la IP privada de tu red.

Cambiar el iframe a `http://10.10.10.10` desde HTTPS introduce contenido mixto. Las excepciones y permisos de acceso a red local varían según navegador y no se deben asumir en Safari. Además, la política de mismo origen impide que la PWA inyecte los manejadores de pellizco en un iframe de otro origen. X-Frame-Options o CSP frame-ancestors de la consola pueden impedir incrustarlo; sus cabeceras reales no se han inspeccionado. CORS no elimina esas restricciones.

Añadir a inicio o usar un Service Worker no elimina estas restricciones. El caché guarda la carcasa, nunca convierte la consola en accesible offline. El tamaño 1180 × 720 pretende una vista tipo tablet; la selección efectiva de interfaz MIX/GAIN y los faders no están verificados.

## Solución sin modificar el MacBook

Usar un equipo independiente en la LAN existente (por ejemplo un pequeño servidor dedicado) que sirva la PWA y un proxy HTTP/WebSocket de `10.10.10.10` bajo el mismo origen HTTPS, con certificado válido y acceso desde el iPhone. No necesita cambiar la IP de la consola, el MacBook, Caddy, dnsmasq ni EGP Bridge. Requiere configurar ese equipo y validar TLS, rutas, WebSocket y política de incrustación. No se ha instalado ni configurado ningún equipo.

Conservar el aislamiento local y no exponer el control de mezcla a Internet. No se propone retirar indiscriminadamente cabeceras de seguridad. Otra vía para desarrollar en el futuro es una app iOS con WKWebView y permiso de red local; ya no sería una PWA alojada únicamente en Pages.

## Archivos y conservación

- `index.html`: original con iframe de aviso para Pages y aviso de estado; sin conexión automática.
- `connection.html`: aviso local de demostración.
- `manifest.webmanifest`, `sw.js`: rutas relativas compatibles con este subdirectorio.
- `icon.png`, `icon-512.png`: iconos originales.
- `README.txt`: documentación original sin cambios (su ejemplo de Caddy no es una instrucción para esta publicación).
- `egp-ui24r-pwa-original-v0.1.zip`: ZIP recibido, intacto.

No se ha modificado ningún MacBook, servicio, red ni mezcla real. Validación física pendiente: instalación Safari, pantalla completa, tablet, pellizco dentro de la consola y respuesta táctil de faders.

## Referencias

- https://developer.mozilla.org/en-US/docs/Web/Security/Defenses/Mixed_content
- https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy
- https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/X-Frame-Options
- https://developer.chrome.com/docs/modern-web-guidance/guides/security/local-network-access
