# CLAUDE.md — Cliente: Lila Wirz Eco Tours Mendoza

## Datos del cliente

**Nombre del negocio:** Lila Wirz Eco Tours Mendoza  
**Rubro:** Traslados turísticos privados en vehículos eléctricos — Mendoza

| Campo | Valor |
|-------|-------|
| WhatsApp | +54 9 261 5258157 → `5492615258157` para `wa.me/` |
| Instagram | [@LilaWirz](https://instagram.com/liwawirz) |
| Airbnb / Booking | **Vuela Alto Mendoza** |
| Experiencia | ~5 años en traslados y paseos turísticos |
| Vehículos | Nissan Leaf 100% eléctrico + sedán BAIC EU5 100% eléctrico |
| Capacidad | 1 a 20 personas (combinando autos y combis) |
| Credencial | Personal Autorizado Oficial — Aeropuerto Internacional El Plumerillo, Mendoza |
| Idiomas | Español (confirmar inglés/portugués con Lila) |
| Logo | Pendiente — Lila mencionó "logo de copas" |
| Reseñas | No tiene por ahora → usar placeholders marcados |
| Fotos | Drive "Mendoza entre montañas y vinos" — pendiente acceso |

---

## Servicios

### Servicios de traslado (punto de inicio: aeropuerto o hotel)

| Destino | Notas |
|---------|-------|
| Bodegas | Zonas: Maipú, Luján de Cuyo, Valle de Uco |
| Alta Montaña | Uspallata, Aconcagua, Puente del Inca, Villavicencio |
| Potrerillos | Embalse y entorno natural |
| Villavicencio | Ruta de montaña icónica |
| City Tour Mendoza | Recorrido por la ciudad |
| Alojamiento | Gestión o traslado hacia alojamientos en Ciudad, Tupungato, Chacras de Coria, Luján de Cuyo |

### Servicio estrella ⭐
**Visita a 3 bodegas en un día** — zonas: Maipú, Luján de Cuyo o Valle de Uco.  
Destacar como experiencia principal con copy y visual diferencial.

### Precios
- **No mostrar precios** — cada experiencia se cotiza a medida
- CTA: "Consultá tu cotización" / "Solicitá tu presupuesto"
- Presupuesto se envía por WhatsApp tras recibir el formulario

---

## Paleta de colores — DEFINITIVA

Lila confirmó: **fondo blanco/crema + color uva para acento**. Usar variante clara (no dark).

```css
:root {
  --accent:  #6B2D5E;   /* uva / grape — color principal */
  --accent2: #8B4A7E;   /* hover — uva más claro */
  --bg:      #FAF8F5;   /* blanco crema cálido */
  --bg2:     #F0EBE4;   /* cards y secciones alternas */
  --bg3:     #FFFFFF;   /* elementos elevados */
  --border:  #E2D9D0;   /* separadores sutiles */
  --text:    #1A1612;   /* negro cálido */
  --muted:   #756B62;   /* texto secundario, metadata */
  --success: #2D6A4F;   /* verde andino */
}
```

> **Nota:** El mapa de Google Maps NO lleva filtro `invert` — la paleta es clara.

---

## Estructura de páginas

SPA de **4 páginas** con routing por hash. Menú: Inicio / Servicios / Sobre mí / Contacto.

```
#          → Inicio
#servicios → Servicios
#sobre-mi  → Sobre mí
#contacto  → Contacto
```

---

## Página 1 — Inicio (`#`)

### Hero
- Imagen de fondo: `hero.jpg` (brindis con Nissan Leaf y Andes al fondo)
- **H1:** *"Llegás a Mendoza, yo me encargo del resto"*
- **Bajada:** Bodegas, alta montaña y Potrerillos — en vehículo eléctrico, a tu ritmo
- **CTA principal en el hero:** botón "Completá el formulario" → `#contacto` — visible above the fold en 375px
- Diferenciador eco (100% eléctrico) visible en el hero o inmediatamente debajo

### Sección Propuesta de Valor (3 columnas con ícono)
- **Privado y personalizado** — Ningún horario fijo, ningún grupo. Tu experiencia, tus paradas.
- **100% eléctrico** — Cero emisiones. Recorrés Mendoza cuidando el ambiente.
- **Autorizado en el aeropuerto** — Acceso oficial al Aeropuerto El Plumerillo. Sin esperas afuera.

### Servicio estrella — bloque destacado
- Tour 3 Bodegas en un día (Maipú / Luján de Cuyo / Valle de Uco)
- Foto grande (`bodega-vinedo.webp`) + descripción breve + CTA "Ver todos los servicios" → `#servicios`

### Sección "Cómo funciona" (3 pasos)
1. Completá el formulario con tus fechas e intereses
2. Te armo un itinerario personalizado
3. Te busco en el aeropuerto o tu alojamiento

**SIN sección de reseñas — no incluir en ninguna página.**

---

## Página 2 — Servicios (`#servicios`) ← LA MÁS IMPORTANTE

### Logo desde la tarjeta
- Intentar mostrar la imagen `logo-tarjeta.jpg` como referencia de marca en algún elemento visual de la página (header de sección, sobre fondo crema)
- La tarjeta tiene: texto "Lila Eco Tours – Experiences in Mendoza", copa, uvas y viñedo

### Componente: Expanding Cards (vanilla HTML/CSS/JS — sin React ni build step)

El componente es una lista `<ul>` con cards que se expanden al hover/click. Implementar en CSS puro + JS mínimo, adaptado al single-file HTML:

**Comportamiento:**
- Desktop: expansión horizontal — la card activa ocupa `5fr`, las demás `1fr`
- Mobile: expansión vertical — la card activa ocupa `5fr` en rows
- Transición: `grid-template-columns` / `grid-template-rows` 500ms ease-out
- Click o hover activa la card
- Card inactiva: imagen grayscale + levemente achicada (`scale(1.1)`)
- Card activa: imagen a color + `scale(1)` + muestra ícono SVG, título grande y descripción
- Título rotado 90° visible en cards inactivas (desktop) — desaparece al activar

**CSS base del componente:**
```css
.exp-cards {
  display: grid;
  gap: .5rem;
  width: 100%;
  transition: grid-template-columns 500ms ease-out, grid-template-rows 500ms ease-out;
  height: 520px; /* desktop */
}
@media (min-width: 768px) {
  .exp-cards { grid-template-rows: 1fr }
}
@media (max-width: 767px) {
  .exp-cards { grid-template-columns: 1fr; height: auto; min-height: 480px }
}
.exp-card {
  position: relative; overflow: hidden; border-radius: 12px;
  cursor: pointer; min-width: 0; min-height: 80px;
}
.exp-card img {
  position: absolute; inset: 0; width: 100%; height: 100%;
  object-fit: cover;
  filter: grayscale(1); transform: scale(1.08);
  transition: filter 400ms ease, transform 400ms ease;
}
.exp-card.active img { filter: grayscale(0); transform: scale(1) }
.exp-card-overlay { position: absolute; inset: 0; background: linear-gradient(to top, rgba(0,0,0,.8) 0%, rgba(0,0,0,.35) 50%, transparent 100%) }
.exp-card-content { position: absolute; inset: 0; display: flex; flex-direction: column; justify-content: flex-end; gap: .5rem; padding: 1.25rem }
.exp-card-title-sideways {
  display: none;
  font-size: .75rem; font-weight: 300; text-transform: uppercase; letter-spacing: .12em;
  color: rgba(255,255,255,.75); transform: rotate(90deg); transform-origin: left center;
  transition: opacity 300ms ease; white-space: nowrap;
}
@media (min-width: 768px) { .exp-card-title-sideways { display: block } }
.exp-card.active .exp-card-title-sideways { opacity: 0 }
.exp-card-icon { color: rgba(255,255,255,.9); opacity: 0; transition: opacity 300ms 75ms ease }
.exp-card-title { font-size: 1.25rem; font-weight: 700; color: #fff; opacity: 0; transition: opacity 300ms 150ms ease }
.exp-card-desc { font-size: .875rem; color: rgba(255,255,255,.8); max-width: 280px; opacity: 0; transition: opacity 300ms 225ms ease }
.exp-card.active .exp-card-icon,
.exp-card.active .exp-card-title,
.exp-card.active .exp-card-desc { opacity: 1 }
```

**JS del componente:**
```js
function initExpandingCards() {
  const cards = document.querySelectorAll('.exp-cards');
  cards.forEach(ul => {
    const items = ul.querySelectorAll('.exp-card');
    let isDesktop = window.innerWidth >= 768;
    
    function setActive(idx) {
      items.forEach((c, i) => c.classList.toggle('active', i === idx));
      const cols = Array.from(items).map((_, i) => i === idx ? '5fr' : '1fr');
      if (isDesktop) {
        ul.style.gridTemplateColumns = cols.join(' ');
        ul.style.gridTemplateRows = '';
      } else {
        ul.style.gridTemplateRows = cols.join(' ');
        ul.style.gridTemplateColumns = '';
      }
    }
    
    items.forEach((card, i) => {
      card.addEventListener('mouseenter', () => { if (isDesktop) setActive(i); });
      card.addEventListener('click', () => setActive(i));
    });
    
    window.addEventListener('resize', () => {
      isDesktop = window.innerWidth >= 768;
      const activeIdx = Array.from(items).findIndex(c => c.classList.contains('active'));
      setActive(activeIdx >= 0 ? activeIdx : 0);
    });
    
    setActive(0);
  });
}
```

### Cards de servicios — **6 cards**

| # | Título | Copy corto | Foto |
|---|--------|-----------|------|
| 1 | Bodegas ⭐ | Tres bodegas de categoría en Maipú, Luján de Cuyo o Valle de Uco. Degustaciones, viñedos y gastronomía de autor. | `bodega-vinedo.webp` |
| 2 | Alta Montaña | Uspallata, Aconcagua y Puente del Inca. La cordillera de los Andes desde adentro, en un solo día. | `montana-aconcagua.jpg` |
| 3 | Potrerillos | El embalse y el río Mendoza rodeados de montañas. Naturaleza pura a una hora de la ciudad. | Unsplash potrerillos/reservoir `<!-- REEMPLAZAR -->` |
| 4 | Villavicencio | La ruta de montaña más icónica de Mendoza. Hotel histórico, flora nativa y paisajes únicos. | `villavicencio.jpg` |
| 5 | City Tour | La ciudad, sus plazas, mercados y cultura local. El punto de partida perfecto antes de salir al campo. | `city-tour-mendoza.jpg` |
| 6 | Alojamiento | Casas y departamentos en Ciudad, Tupungato, Chacras de Coria y Luján de Cuyo. En Airbnb y Booking como "Vuela Alto Mendoza". | Unsplash cozy house mendoza `<!-- REEMPLAZAR -->` |

### CTA debajo de las cards — grande y prominente
- **Un solo botón** debajo de todas las cards, centrado, tamaño grande
- Texto: "Solicitar cotización"
- Acción: navega a `#contacto`
- **NO poner "Solicitar cotización" dentro de cada card**

---

## Página 3 — Sobre mí (`#sobre-mi`)

### Copy en primera persona

Hace más de 5 años llevo turistas de Argentina y de todo el mundo a descubrir Mendoza — uno de los principales destinos gastronómicos de categoría internacional.

Conozco cada bodega, cada curva de la montaña y cada rincón del embalse. Eso es lo que me diferencia: no soy un conductor, soy tu guía local.

Manejamos dos vehículos **100% eléctricos**:
- **Nissan Leaf** — silencioso, cómodo, sin emisiones
- **BAIC EU5** — sedán eléctrico con gran autonomía

Juntos podemos llevar de 1 a 20 personas combinando autos y combis.

Soy **Personal Autorizado Oficial del Aeropuerto Internacional El Plumerillo**. Te busco directamente en la terminal, sin esperas ni confusiones.

### Elementos visuales en la página
- Foto `nissan-leaf-bodega.jpg` del auto eléctrico (protagonista)
- Foto de Lila (placeholder con comentario `<!-- REEMPLAZAR: foto real de Lila -->`)
- Badges visuales (íconos + número):
  - +5 años de experiencia
  - 2 vehículos eléctricos
  - Hasta 20 personas
  - Credencial Aeropuerto El Plumerillo

---

## Página 4 — Contacto (`#contacto`)

### Formulario → WhatsApp
No usa backend. JS arma el mensaje y abre `wa.me/5492615258157`.  
**El mensaje llega SIEMPRE en español** sin importar el idioma del turista.

**Campos del formulario:**
1. Nombre completo
2. Cantidad de personas
3. Cantidad de maletas
4. Fecha estimada del viaje
5. Hora de llegada del vuelo (opcional)
6. Lugar de pick up — dropdown: Aeropuerto El Plumerillo / Hotel / Otro
7. Servicios de interés — checkboxes: Tour Bodegas / Alta Montaña / Potrerillos / Villavicencio / City Tour / Alojamiento / Otro
8. Tipo de viaje — radio: Traslado simple / Itinerario de varios días
9. Mensaje adicional (opcional)

**Mensaje WA generado (ejemplo):**
```
Hola Lila! Te escribo desde la web. Mis datos:

👤 Nombre: [nombre]
👥 Personas: [cantidad]
🧳 Maletas: [cantidad]
📅 Fecha: [fecha]
✈️ Hora llegada vuelo: [hora o "No especificada"]
📍 Pick up: [Aeropuerto / Hotel / Otro]
🗺️ Servicios: [lista de checkboxes marcados]
🔄 Tipo de viaje: [Traslado simple / Itinerario varios días]
💬 Mensaje: [texto o "Sin mensaje adicional"]
```

**WA number:** `5492615258157`

```js
const url = 'https://wa.me/5492615258157?text=' + encodeURIComponent(texto);
```

### Contenido de la página de contacto
- El formulario es el protagonista — **no agregar más texto de relleno**
- Debajo del formulario: links directos a redes y canales
  - WhatsApp: `https://wa.me/5492615258157`
  - Instagram: `https://instagram.com/liwawirz`
  - Airbnb: buscar perfil "Vuela Alto Mendoza" (URL pendiente — `<!-- REEMPLAZAR -->`)
  - Booking: buscar perfil "Vuela Alto Mendoza" (URL pendiente — `<!-- REEMPLAZAR -->`)

---

## CTAs — filosofía DEFINITIVA

- **Hero:** botón "Completá el formulario" → `#contacto` — siempre visible above the fold
- **Servicios:** UN botón grande "Solicitar cotización" DEBAJO de las 6 cards — no dentro de cada card
- **Botón flotante mobile:** ícono de formulario/clipboard abajo a la derecha en mobile — lleva a `#contacto`
  - Solo visible en mobile (display:none en desktop ≥ 769px)
  - Posicionado encima del botón de WhatsApp (bottom: 6rem aprox)
- **Botón WhatsApp flotante:** siempre visible en todas las páginas, abajo derecha
- **NO** poner "Solicitar cotización" ni "Reservar" en la nav
- **NO** hay sección de reseñas en ninguna página

---

## Idiomas

3 idiomas con selector de banderas en la nav:
- 🇦🇷 ES (default)
- 🇺🇸 EN
- 🇧🇷 PT

Sistema `data-es` / `data-en` / `data-pt` del CLAUDE.md del nicho.  
El formulario también se traduce (labels, placeholders, opciones).  
El mensaje de WhatsApp va **siempre en español**.

**IMPORTANTE:** No tocar la navbar ni el selector de banderas de idioma — están funcionando correctamente. Solo modificar el contenido de las páginas.

---

## Estética y diseño

- Tono: **boutique personal y cálido** — Lila habla en primera persona
- Paleta: clara (crema + uva) — ver sección "Paleta definitiva" arriba
- Animaciones: CSS puro con sistema `.fi` — nivel medio, sin GSAP
- Fotos grandes como protagonistas — el diseño es el marco
- Diferenciador eco (eléctrico) visible desde el primer scroll
- **Logo:** intentar extraer o mostrar `logo-tarjeta.jpg` como elemento de marca (en footer o sección "Sobre mí")

---

## Fotos disponibles — inventario completo

Todas las fotos están en esta misma carpeta. Descargadas del Drive "Mendoza entre montañas y vinos".

| Archivo | Qué muestra | Uso recomendado |
|---------|-------------|-----------------|
| `hero.jpg` | Dos manos brindando con copas de tinto. Al fondo: viñedo, **Nissan Leaf blanco**, Andes nevados | **Hero de inicio** — imagen principal confirmada por Lila |
| `bodega-vinedo.webp` | Bodega arquitectónica de lujo (Zuccardi o similar) con viñedos en primer plano y Andes nevados | Card "Tour Bodegas" en Servicios |
| `nissan-leaf-bodega.jpg` | **Nissan Leaf blanco** con stickers Andesmar/Eco, frente a una estancia/bodega. Cielo azul intenso | Sección "Sobre mí" o propuesta de valor "Vehículo eléctrico" |
| `montana-aconcagua.jpg` | Cartel oficial "Cerro Aconcagua 6.962 m.s.n.m." con el cerro nevado de fondo | Card "Alta Montaña" en Servicios |
| `villavicencio.jpg` | Hotel Villavicencio histórico rodeado de montañas nevadas (vista drone) | Card "Villavicencio" en Servicios o "Alta Montaña" |
| `city-tour-mendoza.jpg` | Letrero gigante "Mendoza" iluminado de noche con fuentes de agua | Card "City Tour" en Servicios |
| `logo-tarjeta.jpg` | **Tarjeta de marca** "Lila Eco Tours - Experiences in Mendoza" con copa, uvas y viñedo. Colores: burdó, verde botella, dorado | Extraer paleta de marca + usarla como referencia de identidad |

### Notas de diseño extraídas de las fotos
- **Paleta real de Lila** (del logo-tarjeta.jpg): burdó/vino `#7B2D3E`, verde botella `#2D5A3D`, dorado `#C9A84C` → considerar ajustar el acento de la web si Lila lo aprueba
- El **Nissan Leaf** es el mismo que aparece en el hero (auto eléctrico blanco) — consistencia visual garantizada
- Falta foto de **Potrerillos** — usar Unsplash como placeholder y marcar `<!-- REEMPLAZAR -->`

### Uso en el hero
- `background-image: url('hero.jpg')` con `object-fit: cover`
- Overlay: `rgba(26, 18, 14, 0.35)` para que el texto contraste sin tapar la foto
- En mobile: `background-position: center 45%`

---

## Pendiente antes de construir

- [x] Nombre del negocio / marca
- [x] WhatsApp — `5492615258157`
- [x] Instagram — `@LilaWirz`
- [x] Airbnb/Booking — "Vuela Alto Mendoza"
- [x] Lista de servicios con descripciones
- [x] Servicio estrella definido (Tour 3 Bodegas)
- [x] Preferencia de color confirmada (crema + uva)
- [x] Vehículos (Nissan Leaf + BAIC EU5, ambos 100% eléctricos)
- [x] Años de experiencia (~5 años)
- [x] Credencial aeropuerto confirmada
- [x] **Imagen hero confirmada** — `hero.jpg` guardada
- [x] **Fotos del Drive descargadas** — 7 fotos en carpeta, ver inventario completo arriba
- [ ] Fotos propias: Lila, autos, alojamiento
- [ ] URL real del perfil Airbnb "Vuela Alto Mendoza"
- [ ] Reseñas reales de clientes (en construcción)
- [ ] Logo definitivo ("logo de copas" — pendiente archivo)
- [ ] Idiomas que habla Lila (confirmar inglés/portugués)
