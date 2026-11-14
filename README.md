# Edificios GEM-15

Visor 3D de alturas estimadas de edificios. Cada edificio se extruye según la diferencia entre el modelo de superficie (DSM) y el de terreno (DTM) de [GEM-15](https://developers.google.com/earth-engine/datasets/catalog/projects_climate-and-sustainability_assets_gem-15_v1), calculada sobre las huellas de Google Open Buildings v3.

Es una página estática (HTML, CSS y JavaScript con MapLibre GL JS). No necesita servidor ni claves.

## Qué incluye

- **Mapas base:** claro y oscuro (CARTO), calles (OpenStreetMap), satélite (Esri), relieve (OpenTopoMap) y sin base.
- **Altura mostrada:** percentil 95 de la celda o media de la celda.
- **Histograma, mediana, P90 y máxima** de las alturas visibles.
- **Exageración vertical, opacidad de edificios y área mínima de huella.**
- **Clic en un edificio:** alturas, variación vertical, área y confianza de la huella.
- **La URL guarda la vista** (zoom, posición, giro e inclinación), así que puedes compartir un encuadre exacto.

## Estructura

```
index.html                  página del visor
css/style.css               estilos
js/app.js                   lógica del visor
js/data.js                  edificios con alturas (generado)
vendor/                     MapLibre GL JS 4.7.1 (copia local)
data/edificios_altura_gem15.geojson   datos completos para descargar
gee/gem15_alturas_edificios.js        script de Earth Engine que calcula las alturas
tools/build_data.py         convierte un GeoJSON nuevo en js/data.js
```

## Probarlo en tu computadora

Abre `index.html` con el navegador. Los datos van en `js/data.js`, por eso funciona sin servidor. Los mapas base necesitan internet.

## Publicarlo con GitHub Pages (enlace para compartir)

GitHub Pages es gratis para repositorios **públicos**.

**Desde la web de GitHub**

1. Entra en <https://github.com/new>, ponle un nombre (por ejemplo `edificios-gem15`), márcalo como **Public** y crea el repositorio.
2. Pulsa **uploading an existing file**, arrastra **el contenido** de esta carpeta (no la carpeta en sí, incluye `.nojekyll`) y confirma con **Commit changes**.
3. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama **main**, carpeta **/ (root)** y pulsa **Save**.
4. En uno o dos minutos el visor estará en:

   `https://TU-USUARIO.github.io/edificios-gem15/`

**Desde la terminal**

```bash
cd edificios-gem15-visor
git init
git add .
git commit -m "Visor de alturas GEM-15"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/edificios-gem15.git
git push -u origin main
```

Después activa Pages en **Settings → Pages** como en el paso 3.

## Actualizar los datos

1. Ejecuta `gee/gem15_alturas_edificios.js` en el [Code Editor de Earth Engine](https://code.earthengine.google.com/) (cambia el área y el CRS al inicio) y exporta el GeoJSON.
2. Guárdalo en `data/edificios_altura_gem15.geojson`.
3. Regenera el archivo del visor:

   ```bash
   python3 tools/build_data.py data/edificios_altura_gem15.geojson js/data.js
   ```

4. Sube los cambios a GitHub.

El visor calcula el encuadre inicial a partir de los datos, así que sirve para cualquier zona.

## Limitaciones de las alturas

- Cada celda de GEM-15 mide 15 m (225 m²) y muchas viviendas son más pequeñas. Varios edificios de una misma celda comparten casi el mismo valor.
- El modelo no distingue edificios de árboles, y el percentil 95 tiende a captar árboles altos.
- El DTM es una predicción de aprendizaje profundo con error de varios metros, que pesa mucho al restar alturas bajas.
- Los edificios de más de unos 50 m se capturan mal.
- Las imágenes de GEM-15 son de 2022 a 2024 y las huellas son anteriores, así que pueden faltar construcciones nuevas.
- GEM-15 es un conjunto experimental, en revisión. No lo uses para ingeniería, permisos ni decisiones legales.

Valida siempre con edificios de altura conocida antes de apoyarte en los números.

## Créditos y licencias

- **Alturas:** GEM-15, este conjunto de datos lo produce Google ([CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)). Batchu et al. (2026), [doi:10.31223/X55N75](https://doi.org/10.31223/X55N75).
- **Huellas:** [Google Open Buildings v3](https://sites.research.google/open-buildings/).
- **Mapas base:** © OpenStreetMap contributors, © CARTO, Esri/Maxar/Earthstar Geographics y OpenTopoMap (CC-BY-SA). Respeta sus condiciones de uso. Los mapas de OpenStreetMap y OpenTopoMap son para uso moderado, y si el visor recibe mucho tráfico conviene cambiar a un proveedor con clave propia.
- **Visor:** [MapLibre GL JS](https://maplibre.org/) (BSD-3-Clause), incluido en `vendor/`.
- **Código del visor:** añade la licencia que prefieras (por ejemplo, un archivo `LICENSE`).
