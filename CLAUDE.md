# Plantilla «Reformas y oficios» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **contundente**
(amarillo obra + negro, titulares pesados en mayúsculas, bordes de sección en diagonal, esquinas rectas).
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario, acento, fondo, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`…
  - `primario` = amarillo de botones, etiquetas y banda de cifras. La letra encima se calcula sola (`--sobre-primario`: negra si el primario es claro, blanca si es oscuro).
  - `texto` = color de la letra **y** de las bandas oscuras (barra superior, proceso, presupuesto, pie). En ellas la letra usa `fondo`. Si el cliente pide fondo oscuro, el diseño se invierte solo.
- **Tipografías y aspecto general**: `src/styles/global.css` (`--fuente-titulos` = "Arial Black"…, botones `.btn`/`.btn-sec`, `.tarjeta` con borde grueso izquierdo, bandas `.banda-oscura`/`.banda-gris`/`.banda-primaria`, diagonales `.diagonal-abajo`/`.diagonal-arriba`/`.diagonal-ambas`, `.franja-obra`, barra superior, cabecera, pie y barra inferior fija de móvil). Fuentes del sistema: no añadas fuentes externas.
- **Imágenes**: en `public/`. Las fotos van en `public/fotos/` (lista y descripción en `fotos.json`); las de relleno, en `public/placeholders/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/hero.jpg`).
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = inicial en cuadro amarillo + nombre.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie` (true en plan gratis).

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`.
- `hero` → `hero.astro`: foto con velo oscuro y diagonal; titular, `texto`, `destacado: {titulo, texto}` («Urgencias 24 h»), `cta` (llamar: `url: "tel:+34…"`) y `cta_secundario` (presupuesto). A la derecha, formulario corto (nombre, teléfono, qué necesitas). Opcionales: `formulario_titulo` (si falta, se usa el texto de `cta_secundario`) y `boton_formulario`.
- `servicios` → `servicios.astro`: `items: [{titulo, texto, icono}]` en rejilla densa (6-8). Sus títulos son las opciones de «Tipo de trabajo» del formulario de presupuesto.
- `proceso` → `proceso.astro`: `items: [{titulo, texto, icono}]` numerados en horizontal (4 pasos), banda oscura en diagonal.
- `trabajos` → `trabajos.astro`: `trabajos: [{titulo, texto, lugar, imagen, imagen_antes}]`. Con `imagen_antes` se pinta el par antes/después; sin ella, una sola foto. Etiquetas: `etiqueta_antes` / `etiqueta_despues` (por defecto «Antes» / «Después»).
- `cifras` → `cifras.astro`: `items: [{titulo: "+300", texto}]` en banda amarilla. **No se pinta si `items` está vacío.** No inventes cifras: solo las que dé el cliente.
- `zonas` → `zonas.astro`: `texto` (radio de actuación), `items: [{titulo: localidad}]` como etiquetas, `imagen` opcional.
- `opiniones` → `opiniones.astro`: `testimonios: [{texto, autor, nota}]` (`nota` opcional: trabajo y pueblo). No inventes opiniones.
- `presupuesto` → `presupuesto.astro`: `texto`, `items` (garantías con icono), `nota` («Sin compromiso»), `boton_formulario`; formulario completo (nombre, teléfono, localidad, tipo de trabajo, mensaje).
- `contacto` → `contacto.astro`: bloques con teléfono, WhatsApp, email, dirección y horario de `contacto`; `imagen` opcional. Si `presupuesto` no está activa, aquí sale el formulario.
- `texto` → `texto.astro`: apartado genérico (párrafos, `imagen`, `items`, `cta`). Para apartados nuevos: `tipo: texto`.

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`. Evita poner seguidas dos secciones con diagonal (`proceso`, `cifras`, `presupuesto`).
3. **Cambiar textos, teléfonos, horarios, fotos**: solo en `site.yaml`. Si un texto contiene comas o dos puntos dentro de `{ … }`, ponlo entre comillas.
4. **Listas planas**: nunca una lista dentro de un elemento de lista. Precios y cifras siempre como texto entre comillas (`"+300"`, `"24 h"`). Máximo 24 elementos por lista.
5. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu` (`{ texto: …, url: "/<slug>" }`). Ejemplo: `src/pages/aviso-legal.astro`.
6. **Sección nueva**: mejor un apartado con `tipo:` (`tipo: texto`, `tipo: servicios`, `tipo: trabajos`…) y su propio id. Si hace falta un diseño nuevo, crea `src/components/secciones/<id>.astro` con `seccion(Astro.props.id)`, `Seccion`, `Titular`. `src/pages/index.astro` la recoge sola: no lo toques.
7. **No tocar** `src/layouts/Base.astro`, `src/pages/index.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño. Los ids `hero` y `contacto` son obligatorios.
8. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real, 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil, barra inferior fija de móvil («Llamar» / «WhatsApp»: sale sola si hay teléfono o WhatsApp).
9. Los formularios no tienen servidor: abren el correo (`mailto`) con el email de `site.yaml`. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro`.
10. Iconos disponibles: ver `src/components/ui/Icono.astro` (de oficio: `casa, cocina, ducha, grifo, gota, enchufe, rayo, rodillo, ventana, ladrillo, martillo, herramienta, plano, termometro`…).

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.
