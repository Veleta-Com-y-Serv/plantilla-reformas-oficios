# Plantilla NoExpertise · Reformas y oficios

Web contundente para reformas y oficios, con urgencias, trabajos antes y después y presupuesto.

- Sectores: reformas, fontanería, electricidad, construcción, instalaciones, pintura, carpintería. Estilo: contundente. Plan mínimo: gratis.
- Secciones: hero, servicios, proceso, trabajos, cifras, zonas, opiniones, presupuesto, contacto (+ `texto` para apartados nuevos).
- Campos propios: `hero.destacado`, `trabajos[]` (con `imagen_antes`), `presupuesto.nota`, `cifras.items` (no se pinta vacía).
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`; fotos a generar en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`). Imágenes de relleno (`public/placeholders/`) generadas para esta plantilla, sin licencias de terceros.
