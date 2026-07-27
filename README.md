# Vademécum APS Coquimbo

Consulta del **Arsenal Farmacológico de Atención Primaria del Servicio de Salud Coquimbo**, aprobado por la **Resolución N.º 2504243202** de la SEREMI de Salud Región de Coquimbo, del **11/09/2025**.

Aplicación web instalable que funciona **sin conexión**. Pensada para usarse en el teléfono dentro de la posta, con o sin señal.

- 232 medicamentos y productos
- 506 presentaciones autorizadas
- Tres arsenales: CESFAM / centros APS, dispositivos de urgencia (SAPU / SAR) y postas de salud rural

---

## Qué hace distinto

La resolución es un listado plano de catorce páginas. Esta aplicación la reordena para responder la pregunta que uno se hace en el mesón:

> **¿Puedo entregar esto acá, y bajo qué condición?**

Elegís tu establecimiento una vez y cada medicamento se muestra con una banda de color:

| Color | Significa |
|---|---|
| Verde | Está en el arsenal de tu establecimiento, sin condiciones adicionales en la resolución |
| Ámbar | Está, pero con condición: continuidad de tratamiento, programa o GES, registro sanitario, restricción a SAPU/SAR |
| Rojo | No figura en el arsenal de tu establecimiento, o la resolución lo restringe a hospitales |

Además:

- **Alternativas terapéuticas.** Ante quiebre de stock muestra el reemplazo que la propia resolución ya autoriza.
- **Notas al pie resueltas.** Las llamadas del PDF (¹ ² ³ …) aparecen expandidas junto a la presentación que las lleva, no al final del documento.
- **Carro de paro.** Las 45 presentaciones consignadas para carro de paro, como lista de verificación con lote, vencimiento y cantidad. Marca lo vencido y lo que vence en tres meses o menos, y genera un **acta de revisión imprimible** con firmas.
- **Alertas.** Productos sin registro sanitario vigente, restringidos a hospital, restringidos a SAPU/SAR, y los días que quedan de vigencia de la resolución.

---

## Publicarla en GitHub Pages

Queda con dirección propia, HTTPS e instalable en cualquier teléfono del equipo.

1. En GitHub, **New repository**. Nombre sugerido: `vademecum-aps-coquimbo`. Marcalo **Public**.
2. **Add file → Upload files**. Arrastrá todo el contenido de esta carpeta (`index.html`, `app.js`, `data.js`, `sw.js`, `manifest.webmanifest`, `.nojekyll` y la carpeta `icons/`). **Commit changes**.
3. **Settings → Pages**. En *Build and deployment*, fuente **Deploy from a branch**, rama `main`, carpeta `/ (root)`. **Save**.
4. Esperá uno o dos minutos. Queda publicada en:
   `https://TU-USUARIO.github.io/vademecum-aps-coquimbo/`
5. Abrí esa dirección en el teléfono → menú del navegador → **Instalar aplicación** o **Agregar a la pantalla de inicio**.

Después de la primera apertura queda guardada en el equipo y funciona sin señal.

Para publicar una versión nueva: subí los archivos cambiados, subí el número de `CACHE` en `sw.js` (por ejemplo `vademecum-aps-v3.0.1`) y hacé commit. Los equipos toman la actualización al volver a abrir la app.

## Sin GitHub

Existe también `Vademecum_APS_Coquimbo_v3_pagina_unica.html`: un solo archivo con todo adentro. Se abre con doble clic desde el escritorio, un pendrive o el propio teléfono, sin servidor y sin internet. Sirve para repartir por correo o WhatsApp. No es instalable ni se actualiza sola.

---

## Actualizar los datos cuando cambie la resolución

`data.js` se genera; no se edita a mano.

```bash
python3 herramientas/generar_datos.py
```

El script lee el JSON estructurado del arsenal y escribe `data.js`. Si cambia la resolución, hay que rehacer ese JSON desde el documento nuevo y volver a correr el script. Al terminar, actualizá también la fecha y el número de resolución en `index.html`, `README.md` y en el bloque `meta` del generador.

---

## Archivos

```
index.html                 estructura y estilos
app.js                     buscador, veredicto por establecimiento, carro de paro, alertas
data.js                    datos generados a partir de la resolución (no editar a mano)
sw.js                      service worker: uso sin conexión
manifest.webmanifest       instalación en el teléfono
icons/                     iconos de la aplicación
herramientas/generar_datos.py   generador de data.js
.nojekyll                  evita el procesamiento Jekyll de GitHub Pages
```

Sin dependencias, sin compilación, sin CDN. Nada sale del dispositivo: lo que se marca en el carro de paro se guarda sólo en ese equipo.

---

## Alcance y límites

Esta aplicación **ordena y consulta** el arsenal aprobado por la resolución. **No es una fuente de dosis** y no reemplaza la prescripción, la ficha técnica vigente del producto, los protocolos locales ni la evaluación clínica.

El bloque «Resumen clínico orientativo» de cada ficha **no proviene de la resolución**: está redactado a nivel de grupo farmacológico, no es específico del principio activo y no contiene esquemas de dosificación. Viene marcado como tal dentro de la aplicación y no debe usarse como referencia farmacológica. Para eso corresponde el folleto profesional del ISP, la ficha técnica vigente y las orientaciones técnicas del MINSAL.

Antes de administrar: prescripción vigente, alergias, indicación, dosis, vía, frecuencia, función renal y hepática, embarazo y lactancia, interacciones, vencimiento e integridad del producto.

## Fuente

Resolución N.º 2504243202, SEREMI de Salud Región de Coquimbo, 11/09/2025.
Validación del documento en `seremienlinea.minsal.cl`, opción *Ver Documento*, trámite **2504243202**.
Vigencia: dos años desde la fecha de la resolución.
