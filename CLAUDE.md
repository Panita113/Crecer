# Crecer

App web para seguir el crecimiento de los hijos (talla, peso, perímetro cefálico, observaciones y estudios). Se usa desde el iPhone como app de pantalla de inicio. Todo en español rioplatense.

- El nombre visible es "Nenes" (pestaña e ícono). El usuario no quiere "Crecer" en la interfaz; el repo y la URL siguen llamándose Crecer.
- Son 3 hijos fijos: no hay botón para agregar hijos (solo aparece en modo prueba). Se editan con el ✏️ sobre la foto.
- Las fotos de los hijos están en `fotos/` y el campo `photo` de cada hijo apunta ahí con ruta relativa (ej: `fotos/rafa.jpg`).

## Archivos

- `crecer-app.html`: toda la app (HTML + CSS + JS en un `<script type="module">`).
- `oms.js`: tablas LMS oficiales de la OMS por sexo y mes, y `percentilOMS()`. Generado a partir de los .xlsx de who.int; no editar a mano.
- `manifest.json` e `iconos/`: ícono y datos para instalar la app en el celular.
- `fotos/`: fotos de perfil de los hijos (400x400, recortadas a la cara).
- `backups/` (no se sube): copias JSON de la base.

## Datos (Firebase Realtime Database, proyecto `crecer-57568`)

```
crecer/
  children: [ { id, name, emoji, photo, birth, sexo: 'M'|'F'|'' } ]
  records/{childId}: [ { id, fecha, edad, peso, talla, pc, ageDays, ageMonths } ]
  obs/{childId}: [ { id, fecha, text } ]
  estudios/{childId}: [ { id, tipo, fecha, hora, lugar, edad, nota } ]
```

- Guardar siempre con `saveToFirebase('ruta', ...)` pasando solo lo que cambió (ej: `records/${childId}`). Nunca reescribir todo `crecer` de una: pisa lo que guardó otro dispositivo.
- `ageDays`/`ageMonths` se recalculan al cargar a partir de `edad` (texto tipo "1A 3M 14D"), o de `fecha` + `birth` si no hay texto.
- Todo texto escrito por el usuario va con `esc()` antes de meterlo en `innerHTML`.

## Probar en local

```
python -m http.server 8080
```

Abrir `http://localhost:8080/crecer-app.html`. Con `?prueba` al final usa el nodo `crecer-prueba` (datos aparte, con una franja naranja arriba); al terminar, borrar lo que se haya creado ahí.

## Publicar

GitHub Pages desde la rama `main` de `panita113/Crecer`: `git push` y en 1-2 minutos se actualiza https://panita113.github.io/Crecer/crecer-app.html. El repo es público: no subir datos personales (el `.gitignore` ya excluye `backups/` y `GITHUB.txt`).
