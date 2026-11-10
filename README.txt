EGP Ui24R — Fullscreen (prototipo, no probado contra consola real)

REQUISITO: servir por HTTPS desde el mismo origen que el proxy de la Ui24R.
NO publicar solo en GitHub Pages: el navegador bloquea recursos HTTP internos desde HTTPS y no garantiza que el iframe funcione.

EJEMPLO DE CONFIGURACION CADDY (bloque para subdominio dedicado, NO pegar sobre tu Caddy existente sin inspeccionarlo):

mixer.elenagirjoaba.com {
  handle_path /app/* {
    root * /RUTA/ABSOLUTA/egp-ui24r-pwa
    file_server
  }
  handle {
    reverse_proxy http://10.10.10.10 {
      header_down -X-Frame-Options
      header_down -Content-Security-Policy
    }
  }
}

Abrir https://mixer.elenagirjoaba.com/app/ desde Safari conectado a la red EGP; Compartir -> Agregar a pantalla de inicio.
Se requiere DNS y TLS válido para el subdominio, incluyendo acceso local; NO se han configurado automáticamente.

NOTAS IMPORTANTES:
- La Ui24R tiene control MIX y GAIN. Un redimensionado no reemplaza ese cambio de pantalla.
- Zoom de dos dedos dentro del iframe solo se intenta si la consola se sirve en el MISMO ORIGEN y permite lectura del documento.
- Si el propio software de Soundcraft intercepta los gestos, usar los controles +/- y MOVER de la PWA.
- NO modificar Caddy o red de producción sin respaldo ni pruebas, especialmente durante un show.
- No hacer cambios de ganancia ni phantom power sin comprobar el estado real de la consola.
