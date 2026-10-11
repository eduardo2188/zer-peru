# Tablero de zonificación de energías renovables · carpeta para publicar

Dos páginas, una por idioma:

| Archivo | Idioma | Dirección una vez publicado |
|---|---|---|
| `index.html` | Español | la raíz del sitio |
| `en/index.html` | Inglés | `/en/` |

Cada una se basta sola. El mapa base satelital, las ocho capas ráster, el logo
y todos los datos van incrustados dentro del propio archivo. No llaman a ningún
servidor externo salvo la hoja de tipografías de Google Fonts, y si esa no
carga la página se ve igual con la tipografía del sistema. Pesan 1,4 MB cada
una. El conmutador de idioma del riel enlaza una con otra, así que hay que
subir las dos juntas conservando la carpeta `en`.

Elaborado por Eduardo Rojas Baez y Christian Contreras Otiniano.

---

## Opción A · Servidor del MINEM o de TNC (la recomendada)

Es la que da una dirección institucional, del tipo
`https://mapas.minem.gob.pe/zer/tablero/`.

Entregar la carpeta al área que administra el portal y pedir que la coloquen en
el servidor web. Para ellos es copiar dos archivos: no hay base de datos, ni
backend, ni dependencias que instalar.

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

## Cómo se usa el mapa

Los botones de la esquina acercan, alejan y vuelven a la vista del país
completo. También se puede hacer **Ctrl + rueda** sobre el mapa, doble clic
para acercar, y arrastrar para desplazarse. La rueda a secas sigue
desplazando la página, para no dejar al lector atrapado a media pantalla.
La leyenda va dentro del mapa y cambia con la capa elegida.

## El mapa base

Es la imagen satelital **Blue Marble** de la NASA, con sombreado de relieve y
batimetría, pedida al servicio GIBS ya recortada a la caja del estudio. Es
imagen de dominio público: se puede incrustar y redistribuir sin pedir permiso
ni pagar licencia, que es justo lo que hace falta en una página que se va a
copiar a varios sitios.

Si en algún momento se quiere más detalle en la cordillera, hay una alternativa
con la imagen de Esri:

```
python generar_satelital.py esri
```

Se ve más nítida, pero obliga a citar a «Esri, Maxar, Earthstar Geographics y
la comunidad de usuarios SIG» y su redistribución queda sujeta a los términos
de Esri. Con la de la NASA no hay ninguna de las dos ataduras.

## Volver a generar las páginas

Desde `E:\TNC_2026\PowerBI`, en este orden:

```
python rasterizar_capas.py      # las ocho capas ráster a PNG
python generar_satelital.py     # el mapa base satelital
python preparar_insumos.py      # TopoJSON y tablas del modelo
python estadisticas_zonales.py  # cruce de rásteres con límites regionales
python construir_tablero.py     # arma las tres salidas
```

El orden importa: `rasterizar_capas.py` reescribe el manifiesto, por eso va
antes que `generar_satelital.py`, y `estadisticas_zonales.py` lee de ese
manifiesto qué valor de píxel es cada clase. El último paso escribe
`tablero_zer.html` y las dos páginas de esta carpeta.

Esos scripts necesitan el Python de QGIS y sus variables de entorno
(`PYTHONHOME`, `PROJ_DATA`, `GDAL_DATA`); lo más cómodo es lanzarlos desde la
consola de OSGeo4W que instala QGIS.

## Comprobación de consistencia

El cruce zonal sirve además de control: la clase «Excelente» de la
zonificación reproduce exactamente la superficie del escenario 1 del documento
(772 850 ha solar y 550 975 ha eólico), y las tres clases juntas dan el
escenario 2 con una diferencia de dos a siete píxeles de borde. Si al
regenerar los insumos esas cifras dejan de cuadrar, algo cambió en los
rásteres de partida.
