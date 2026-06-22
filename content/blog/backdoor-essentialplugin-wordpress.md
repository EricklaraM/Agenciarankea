---
title: "Más de 30 plugins de WordPress fueron hackeados para distribuir malware a miles de sitios"
date: 2026-06-22
description: "El paquete EssentialPlugin fue comprometido con un backdoor que inyecta malware en wp-config.php, genera spam y redirecciones invisibles para el dueño del sitio. Te explicamos qué pasó y cómo protegerte."
category: "Noticia · Seguridad WordPress"
keywords: "essentialplugin backdoor, plugins wordpress hackeados, malware wordpress wp-config"
draft: false
---

Una de las cadenas de suministro más usadas del ecosistema WordPress acaba de demostrar por qué la seguridad no termina al instalar un plugin de fuente confiable. Más de 30 plugins del paquete **EssentialPlugin** fueron comprometidos con código malicioso que abre una puerta trasera (backdoor) y permite acceso no autorizado a los sitios que los tienen instalados.

El hallazgo es serio por su alcance: hablamos de complementos con **cientos de miles de instalaciones activas** en conjunto.

## Qué pasó exactamente

Según la investigación reportada por BleepingComputer, el código malicioso fue plantado en **agosto de 2025**, poco después de que el proyecto cambiara de dueño en una operación de seis cifras. EssentialPlugin existe desde 2015 (originalmente como WP Online Support) y ofrece sliders, galerías, herramientas de marketing, extensiones para WooCommerce, utilidades de SEO/analítica y temas.

El backdoor estuvo **inactivo durante meses**. Recién hace poco se activó y empezó a propagarse a través de actualizaciones automáticas de los plugins, lo que es especialmente peligroso: muchos administradores actualizan sin sospechar nada.

El descubrimiento lo hizo Austin Ginder, fundador del hosting administrado Anchor Hosting, tras recibir un aviso sobre un complemento que permitía acceso de terceros.

## Cómo funciona el ataque

El mecanismo es sofisticado y está diseñado para pasar inadvertido:

1. Una vez activado, el código contacta una infraestructura externa y descarga un archivo llamado `wp-comments-posts.php` (muy parecido al archivo legítimo `wp-comments-post.php`, para confundir).
2. Ese archivo **inyecta malware dentro de `wp-config.php`**, el archivo central que conecta el sitio con su base de datos.
3. El malware queda invisible para el dueño del sitio y usa resolución de direcciones de comando y control (C2) basada en Ethereum para evadir detección.
4. Según las instrucciones que recibe del servidor C2, genera enlaces spam, redirecciones y páginas falsas.

El detalle más astuto: el spam **solo se mostraba a Googlebot**, no a los visitantes normales ni al administrador. Así el sitio se contaminaba a ojos de Google mientras el dueño no veía nada raro en su pantalla.

La plataforma de seguridad PatchStack confirmó que el backdoor solo se ejecutaba cuando el endpoint `analytics.essentialplugin.com` respondía con contenido serializado malicioso.

## La respuesta de WordPress.org

El equipo de WordPress.org reaccionó rápido: **cerró los plugins afectados y empujó una actualización forzada** para neutralizar la comunicación del backdoor y deshabilitar su ejecución.

Sin embargo, hay una advertencia importante: esa actualización **no limpió el archivo `wp-config.php`**. Es decir, la puerta puede estar cerrada, pero el código que se inyectó en la configuración del sitio sigue ahí hasta que alguien lo remueva manualmente. Además, aunque una ubicación conocida es `wp-comments-posts.php`, el malware puede esconderse en otros archivos.

## Qué hacer si usas alguno de estos plugins

Si tu sitio (o el de un cliente) tiene instalado cualquier plugin de EssentialPlugin, conviene actuar de inmediato:

- **Revisa `wp-config.php`** en busca de código sospechoso o inyectado, idealmente comparándolo con una versión limpia.
- **Busca archivos extraños**, en particular `wp-comments-posts.php` y cualquier archivo modificado en fechas recientes que no reconozcas.
- **Audita las cuentas de administrador** y rota contraseñas y claves de seguridad (`salts`) del `wp-config.php`.
- **Inspecciona en Google Search Console** si aparecen páginas o enlaces que no creaste, ya que el spam estaba dirigido específicamente a Googlebot.
- **No confíes solo en la actualización forzada**: limpiar el `wp-config.php` requiere intervención manual.

## La lección de fondo

Este caso refuerza un principio clave de la seguridad web: un plugin confiable hoy puede dejar de serlo mañana si cambia de manos. La cadena de suministro de software es un vector de ataque real, y mantener visibilidad sobre qué se instala, quién lo mantiene y qué cambia en cada actualización ya no es opcional.

---

¿Necesitas auditar la seguridad de tu sitio WordPress o limpiar una infección? [Conversemos sobre tu caso →](/contacto)

*Fuente: BleepingComputer.*
