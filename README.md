# Dashboard Consolidado Apagados — Azimut Energía / UniRed

Aplicación web estática (un solo archivo `index.html`) que permite **arrastrar y soltar** el Excel maestro *Consolidado Apagados.xlsx* y genera automáticamente el dashboard con KPIs, tablas y gráficas.

- No requiere servidor ni base de datos.
- Todo el procesamiento del Excel ocurre **en el navegador** del usuario (los datos nunca salen de su equipo).
- Compatible con Azimut Energía y UniRed: cualquiera con acceso al Excel del SharePoint puede usar el link.

---

## 📁 Archivos del proyecto

| Archivo | Descripción |
|---|---|
| `index.html` | Dashboard completo (auto-contenido). Único archivo necesario. |
| `README.md` | Este documento. |
| `render.yaml` | Configuración opcional para despliegue en Render como *Static Site*. |
| `.gitignore` | Evita subir el Excel a GitHub. |

> ⚠️ **Importante:** los archivos `.xlsx` y el HTML original están excluidos por `.gitignore`. El Excel debe mantenerse en el SharePoint compartido.

---

## 🚀 Cómo usar el dashboard (usuario final)

1. Abre el link público (el que Render te dé, algo como `https://dashboard-apagados.onrender.com`).
2. **Arrastra el archivo `Consolidado Apagados.xlsx`** al recuadro central. O haz clic en *Seleccionar archivo*.
3. En 5–15 segundos el dashboard se renderiza con toda la información del Excel.
4. Usa los filtros del header (Responsable, Operador, Región, UniRed, búsqueda) para explorar.
5. Haz clic en **⬇ PDF** para descargar una copia del dashboard actual.
6. Para cargar otro Excel: **↺ Cambiar Excel**.

### ¿Qué pasa si el Excel no es el correcto?

- Si le falta la hoja `Consolidado Apagados`, o si faltan columnas obligatorias, se muestra un mensaje de error listando **qué falta**. Nada se renderiza.

### Columnas obligatorias que debe tener la hoja `Consolidado Apagados`

- `Operador`, `Pair Site (Offgrid) Emplazamiento`, `NIC`, `Nombre del Sitio`, `Comercializador`
- `Valor promedio mensual`, `UniRed`, `Estado Azimut`, `Gestión`
- `Meses con ahorro`, `Valor Ahorro Acumulado`, `Región`, `Responsable de la cuenta`

Estructura esperada de las filas:
- **Fila 1:** totales/resumen (se ignora).
- **Fila 2:** encabezados.
- **Fila 3 en adelante:** datos de sitios.

---

## ⚙️ Cómo subirlo a GitHub y desplegarlo en Render

### Paso 1 — Crear repositorio en GitHub

1. Entra a https://github.com/new
2. Nombre: `dashboard-apagados-azimut` (o el que prefieras).
3. Visibilidad: **Público** (o privado si tu cuenta de Render lo permite).
4. **NO** marques "Add README" (ya tienes uno).
5. Click en *Create repository*.

### Paso 2 — Subir los archivos

Desde una terminal en la carpeta del proyecto:

```bash
git init
git add index.html README.md render.yaml .gitignore
git commit -m "Dashboard inicial"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/dashboard-apagados-azimut.git
git push -u origin main
```

O más fácil: usa **GitHub Desktop** (https://desktop.github.com/), arrastra la carpeta y publica.

### Paso 3 — Desplegar en Render

1. Entra a https://dashboard.render.com y haz login.
2. Clic en **New +** → **Static Site**.
3. Conecta tu cuenta de GitHub y selecciona el repositorio recién creado.
4. Configura:
   - **Name:** `dashboard-apagados` (o el que quieras)
   - **Branch:** `main`
   - **Build Command:** *(dejar vacío)*
   - **Publish Directory:** `.` (un punto — significa la raíz)
5. Clic en **Create Static Site**.
6. Espera ~1 minuto. Render te dará una URL pública como `https://dashboard-apagados.onrender.com`.
7. Comparte ese link con tu equipo (Azimut + UniRed).

---

## 🔄 ¿Cómo actualizar el dashboard cuando cambian los datos?

**No necesitas actualizar nada.** El Excel es la fuente única de verdad:

- Los usuarios (Azimut y UniRed) descargan el Excel más reciente del SharePoint.
- Lo arrastran al dashboard y ven la información actualizada al instante.

El código en Render solo se actualiza si cambia la **estructura del Excel** (nuevas columnas, secciones, KPIs). En ese caso:

1. Edita `index.html` localmente.
2. `git commit -am "actualizacion X"` + `git push`
3. Render detecta el cambio y despliega automáticamente en ~1 minuto.

---

## 🛠 Tecnologías usadas (sin instalación)

- **SheetJS (`xlsx.js`)** para parsear el Excel en el navegador.
- **html2pdf.js** para la exportación a PDF.
- CSS puro y JavaScript vanilla — cero frameworks, cero build step.

Ambas librerías se cargan desde CDN público (jsDelivr / cdn.sheetjs.com), así que basta con abrir el HTML en cualquier navegador moderno (Chrome, Edge, Firefox, Safari).

---

## ❓ Preguntas frecuentes

**¿El Excel se sube a algún servidor?**
No. Todo se procesa localmente en el navegador con JavaScript. Render solo sirve el HTML.

**¿Funciona en móvil?**
Sí, pero está optimizado para pantallas grandes (dashboard). En móvil se ve, pero la experiencia ideal es en escritorio.

**¿Puedo trabajar sin internet?**
Necesitas conexión al menos una vez para cargar las librerías del CDN. Después funciona hasta que refresques.

**¿Y si el Excel tiene más de 10 mil filas?**
Debe funcionar (SheetJS aguanta bien). La carga inicial puede tardar 20-30 segundos con archivos muy grandes.

**¿Cómo cambio los colores o el logo?**
El logo está embebido en base64 en `index.html` (buscá `LOGO_B64`). Los colores están en las variables CSS al inicio del archivo (`--bg`, `--cyan`, etc.).
