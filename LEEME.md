# Tablero de zonificación de energías renovables — carpeta para publicar

`index.html` es la página completa. **No necesita nada más**: el mapa, las ocho
capas ráster, el relieve, el logo y todos los datos van incrustados dentro del
propio archivo. No llama a ningún servidor externo salvo la hoja de tipografías
de Google Fonts, y si esa no carga la página se ve igual con la tipografía del
sistema.

Pesa 1,1 MB. Funciona abriéndola con doble clic, desde una memoria USB o desde
cualquier hosting estático.

---

## Opción A · Servidor del MINEM o de TNC (la recomendada)

Es la que da una dirección institucional, del tipo
`https://mapas.minem.gob.pe/zer/tablero/`.

Entregar `index.html` al área que administra el portal y pedir que lo coloquen
en una carpeta del servidor web. Para ellos es copiar un archivo: no hay base de
datos, ni backend, ni dependencias que instalar.

## Opción B · GitHub Pages (gratuito y permanente)

Dirección del tipo `https://USUARIO.github.io/zer-peru/`.

1. Crear un repositorio nuevo y **público** en <https://github.com/new>,
   por ejemplo `zer-peru`. No marcar «Add a README».
2. En esta carpeta, con el repositorio ya creado:

   ```
   git remote add origin https://github.com/USUARIO/zer-peru.git
   git branch -M main
   git push -u origin main
   ```

3. En el repositorio: **Settings → Pages**. En *Source* elegir
   «Deploy from a branch», rama `main`, carpeta `/ (root)`. Guardar.
4. A los dos o tres minutos la dirección queda activa. Cada `git push`
   posterior la actualiza.

El repositorio local ya está inicializado y con el primer *commit* hecho, así
que solo faltan los tres comandos del punto 2.

## Opción C · Netlify Drop (lo más rápido)

Dirección del tipo `https://zer-peru.netlify.app`.

1. Entrar en <https://app.netlify.com/drop>.
2. Arrastrar **esta carpeta** completa a la ventana del navegador.
3. Crear la cuenta gratuita que pide para conservar el sitio, y en
   *Site configuration → Change site name* poner el nombre que se quiera.

---

## Volver a generar la página

Desde `E:\TNC_2026\PowerBI`:

```
python generar_relieve.py     # mapa base de relieve, solo si cambia el DEM
python rasterizar_capas.py    # las ocho capas ráster a PNG
python preparar_insumos.py    # TopoJSON y tablas del modelo
python construir_tablero.py   # arma la página
```

El último paso escribe dos archivos: `tablero_zer.html` (fragmento para
incrustar) y `publicar/index.html` (esta página completa).
