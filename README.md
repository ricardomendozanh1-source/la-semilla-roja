# La Semilla Roja — Landing Page

Landing page de una sola página para **La Semilla Roja**, restaurante de cochinita pibil en Ciudad de México (fundado en 2012, sucursales en Coyoacán e Iztapalapa/Sinatel). Este sitio está pensado como reemplazo de lasemillaroja.com.mx. Archivo único (`index.html`), sin frameworks ni build step — solo HTML, CSS y JS inline, más las fuentes de Google Fonts (Playfair Display + Poppins). El logo del header (`logo.webp`), la foto del hero (`semillas.jpg`, comprimida desde el `semillas.png` original de 2.2 MB a ~195 KB con calidad JPEG 82, sin pérdida visible), la galería (`lasemillaroja27.jpg.webp`–`lasemillaroja32.jpg.webp`) y las fotos de fachada en "Ubicación" (`sucursal-coyoacan.jpg.webp`, `sucursal-sinatel.jpg.webp`) son todas reales, tomadas de lasemillaroja.com.mx.

## En vivo

- **Producción:** https://la-semilla-roja.vercel.app
- **Repositorio:** https://github.com/ricardomendozanh1-source/la-semilla-roja
- Desplegado en Vercel, conectado al repo de GitHub — cada `git push` a `main` puede volver a desplegarse desde el dashboard de Vercel (o automáticamente si activas los despliegues por Git en el proyecto).

## Ver el sitio en local

```powershell
./serve.ps1
```

Luego abre `http://localhost:8877/`. También puedes abrir `index.html` directamente en el navegador.

## Origen del contenido

El menú (Cochinita, Sopas, Bebidas con precios en $ MXN), la historia (fundada en 2012, dos sucursales, servicio de eventos/taquizas), la sección "Eventos y Taquizas" (los 4 paquetes: Taquiza Roja, Plancha Especial, Plancha Premium, Guisados, con sus precios y condiciones), el teléfono, WhatsApp y las redes sociales están tomados de **lasemillaroja.com.mx** (menú, quiénes-somos, eventos-y-taquizas y contacto). Sigue pendiente de confirmar/reemplazar:

- **Horarios**: el sitio original no publica horario; los que aparecen en Reservas y Ubicación son un placeholder razonable, hay que confirmarlos con el negocio.
- **Segunda sucursal (Iztapalapa/Sinatel)**: por ahora solo se muestra la dirección de Coyoacán en el panel de "Ubicación"; falta diseñar un bloque para la segunda sucursal si quieren mostrar ambas.
- **Galería**: las 6 fotos (`lasemillaroja27`–`32`) muestran platos reales del negocio pero con leyendas genéricas ("Cocción Lenta", "Hecho a Mano", etc.) — puedes ponerles nombres más específicos si sabes qué es cada uno.
- **Testimonios**: siguen siendo citas de ejemplo (no clientes reales); reemplázalas cuando tengas reseñas reales.

## Efecto de scroll en el hero

El hero tiene un efecto de "ventana" que se revela con el scroll/gesto del mouse o del dedo: `semillas.jpg` aparece dentro de un recuadro central que se expande hasta cubrir toda la pantalla mientras la imagen hace zoom-out, con el texto siempre fijo y centrado encima. Mientras dura la animación, la página **no se desplaza realmente** (el scroll/wheel/touch se intercepta y solo alimenta la animación); al terminar, los botones "Reservar una mesa" / "Ver el menú" aparecen con un fade, y hay que presionar el botón "Descubre" para pasar a la sección Historia — recién ahí se libera el scroll normal de la página. Un enlace del menú de navegación también libera el bloqueo.

Está hecho en CSS (`clip-path` + `transform: scale`) y un script vanilla que interpola (lerp) el valor objetivo con `requestAnimationFrame` para que el movimiento se sienta suave — sin React, sin `framer-motion`, sin build step. Respeta `prefers-reduced-motion` (sin bloqueo de scroll, imagen y botones visibles de inmediato) y tiene un fallback `<noscript>`.

## Micro-interacciones

Para que las secciones bajo el hero no se sintieran genéricas (fade-up plano de "template"), cada grupo repetido tiene su propio detalle:

- **Reveal escalonado** (`.stagger`): platos, tarjetas de la galería, testimonios y estadísticas aparecen en cascada (cada elemento con su propio delay), no todos a la vez.
- **Contador animado**: las cifras de "Nuestra historia" (14 años, 2 sucursales, 100% cochinita) cuentan desde 0 cuando entran en pantalla.
- **Pestañas del menú**: al cambiar de categoría hay un crossfade (fade-out → fade-in con leve desplazamiento) en vez de un corte instantáneo.
- **Galería con tilt 3D**: en dispositivos con mouse, las tarjetas siguen el cursor con una leve inclinación 3D (`rotateX`/`rotateY`) además del levantamiento; se omite en touch y con `prefers-reduced-motion`.
- **Botón principal**: barrido de brillo diagonal al pasar el mouse.

Todo respeta `prefers-reduced-motion` (los conteos aparecen directo en su valor final, los estilos escalonados se muestran sin animar, el tilt y el sheen se desactivan).

## Popover de redes sociales

Abajo a la derecha, fijo en toda la página (`position:fixed`), hay un botón circular dorado que al hacer clic despliega Instagram, Facebook y WhatsApp (con los enlaces reales del negocio) en cascada. Se cierra al hacer clic fuera, con Escape, o al volver a presionar el botón. El botón aparece con un fundido 1.8s después de cargar la página (no está ahí desde el primer instante) y tiene un resplandor dorado que respira lentamente cuando está cerrado — sutil, no un badge ni un tooltip, para no sentirse como plantilla genérica.

## Notas de diseño

- Paleta: negro cálido, rojo vino profundo y dorado antiguo — evoca el nombre "Semilla Roja" y una estética de alta cocina.
- Tipografía: Playfair Display (titulares) + Poppins (cuerpo).
- Animaciones respetan `prefers-reduced-motion`.
- El formulario de reservas no envía datos a ningún servidor: arma un mensaje y abre WhatsApp Web/App con el texto prellenado.
- Ubicación: cada sucursal muestra su foto de fachada real y su dirección (ver sección "Origen del contenido").
