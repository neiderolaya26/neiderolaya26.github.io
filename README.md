# Neider Andrés Olaya F. | portfolio

Portafolio bilingüe (inglés/español) de Neider Andrés Olaya F., Data Scientist en riesgo de crédito, machine learning y analítica financiera.

**Página en línea:** https://neiderolaya26.github.io (español: https://neiderolaya26.github.io/es/)

- CV en PDF: https://neiderolaya26.github.io/cv/neider-olaya-cv.pdf
- LinkedIn: https://www.linkedin.com/in/neider-olaya/

Sitio Hugo ligero y sin dependencias de tema, basado en la estructura del portafolio de [Sergio A. Mora Pardo](https://github.com/sergiomorapardo/sergiomorapardo.github.io).

## Desarrollo local

Requiere Hugo `0.165.0` o posterior.

```bash
hugo server
```

## Validación

```bash
hugo --gc --minify --cleanDestinationDir --destination /tmp/neider-portfolio-build
python3 scripts/check_site.py /tmp/neider-portfolio-build
```

## Estructura

- `content/`: páginas y casos en ambos idiomas.
- `data/portfolio.yaml`: textos de inicio, experiencia, métricas y especialidad.
- `layouts/`: plantillas Hugo.
- `assets/`: CSS y JavaScript.
- `static/`: monograma, imagen para redes y CV público (`/cv/neider-olaya-cv.pdf`).
- `.github/workflows/hugo.yaml`: compilación y despliegue en GitHub Pages.

Para usar una foto en lugar del monograma, súbela a `static/images/profile/` y cambia la ruta en `layouts/index.html` y `layouts/_default/baseof.html`.

## Despliegue

Los cambios enviados a `main` activan el workflow de GitHub Pages. En la configuración del repositorio, la fuente de Pages debe ser **GitHub Actions**.
