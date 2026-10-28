[README.md](https://github.com/user-attachments/files/34105668/README.md)
# PERIMETER GPS — Distancias de seguridad IED / UXO

Aplicación web de una sola página (`index.html`, sin servidor ni dependencias propias) para calcular **perímetros de seguridad orientativos** ante artefactos explosivos improvisados (IED) y municiones sin explotar (UXO), usando **distancias escaladas Z (Hopkinson–Cranz)** con K = 15,4 m/kg^⅓ y umbral de daño auditivo mínimo con K = 18. Incluye mapa en tiempo real, catálogo de municiones, control de puntos (PCI / PCEOD), registro de incidentes e informe PDF.

> ⚠️ **Aviso**: herramienta de apoyo para personal formado en EOD/desminado humanitario. Los radios son **estimaciones orientativas** basadas en fórmulas abiertas; nunca sustituyen los SOP, la doctrina oficial ni la evaluación de un especialista cualificado.

---

## 🚀 Publicación en GitHub Pages

1. Crea un repositorio nuevo en GitHub (p. ej. `perimeter-gps`).
2. Sube el archivo **`index.html`** (y este `README.md`).
3. Ve a **Settings → Pages** → en *Source* elige la rama `main` y la carpeta `/ (root)`.
4. En unos segundos la app quedará en: `https://TU-USUARIO.github.io/perimeter-gps/`

No requiere build, base de datos ni conexión para el cálculo (solo se cargan Leaflet, html2canvas, jsPDF y las fuentes desde CDN, y el mapa de teselas de OpenStreetMap, por lo que se necesita internet para el mapa y el PDF).

---

## 📋 Uso de la aplicación

### 01 · Amenaza

Selecciona el tipo de amenaza con los botones **IED** / **UXO / Munición**.

**Modo IED**
- **Contenedor / ocultación**: carta postal, paquete, mochila, maletín, maleta grande, bidón, bombona de gas, coche (VBIED), furgoneta o camión. Fija la carga base y la eficiencia de explosivo.
- **Tipo de explosivo supuesto**: TNT, RDX, PETN, HMX, C-4, Comp. B, dinamita, ANFO, emulsión, TATP, HMTD, pólvora negra o clorato + azúcar. El factor de equivalencia multiplica el peso automáticamente.
- Ajusta el **peso de carga (kg TNT eq.)** si lo necesitas.

**Modo UXO / Munición**
- **Catálogo de municiones sin explotar**, agrupado por familias: granadas de mano (Rusia/URSS, EE. UU., otros), morteros, proyectiles de artillería, cohetes, bombas de aviación, submuniciones, minas terrestres, MANPADS, misiles y **drones / UAS kamikaze** (Lancet-3, Shahed-131, Shahed-136/Geran-2 — al final de la lista), más munición para drones/FPV.
- Al seleccionar munición se **fijan automáticamente la NEQ (TNT equivalente)** y la metralla.
- **Metralla / fragmentación**: con metralla o solo onda expansiva.
- El botón **🔗 Ver ficha e imagen** abre una referencia visual de la munición seleccionada (Wikipedia / cat-uxo.com).

### 02 · Distancias calculadas

Tabla con los radios según la carga equivalente:
- **Onda de choque — letal** (≈ 35 kPa), rojo.
- **Onda de choque — daño mínimo** (K = 18), naranja.
- **Daño estructural** — madera (≈ 1 psi), ladrillo (≈ 2 psi) y hormigón (≈ 4–5 psi), círculos discontinuos.
- **Fragmentación — personal al descubierto** (fórmula de alcance de fragmentos), amarillo.
- **Zona de exclusión / desalojo — rotura de cristales**, verde.

Con el selector **kg · m / lb · ft** se cambia todo el panel (incluido el peso) a unidades imperativas o métricas.

### 03 · Mapa real · perímetros

1. Pulsa **◎ Activar mi posicionamiento GPS** (acepta el permiso del navegador; tu posición se muestra en azul en tiempo real, actualizada al moverte).
2. Pulsa **📍 Posicionar en mapa amenaza** y toca el mapa donde esté la amenaza. Mientras no la fijes, puedes **arrastrar** el punto rojo. Confirma con **✅ Fijar amenaza actual** o cancela con **🗑 Borrar amenaza**.
3. Igual de simple para los puntos de control:
   - **🚩 PCI** — punto de control de incidente (azul).
   - **⛑ PCEOD** — punto de control EOD (verde).
   - Botones de borrado específicos para cada uno.

Los perímetros se dibujan sobre el mapa alrededor de la amenaza; la leyenda explica cada color. Con varias amenazas, cada punto mantiene su cálculo y tu exposición se recalcula respecto a la más cercana.

### 04 · Coordenadas y exposición

Panel con las coordenadas de todas las amenazas, PCI y PCEOD, la distancia de tu posición a cada amenaza y tu **estado de exposición** (dentro/fuera de cada radio). Útil para copiar coordenadas a otros sistemas o comunicaciones.

### 05 · Registro de incidentes

Cada amenaza fijada (y cada modificación) se añade al registro de la sesión con hora, coordenadas, tipo de munición y NEQ. El registro es **editable y borrable** línea a línea (botón ✕).

### 06 · Generar informe

- Pulsa **📄 Generar informe PDF**: captura automáticamente el mapa actual y genera un PDF con:
  - amenazas con coordenadas, tipo y NEQ,
  - tabla de radios de seguridad,
  - PCI y PCEOD,
  - tu exposición,
  - registro de incidentes,
  - la **captura del mapa insertada como imagen**.
- Guarda el PDF o compártelo directamente (según el navegador/dispositivo).

---

## 📐 Base de cálculo

| Radio | Fórmula |
|---|---|
| Onda de choque letal / daño mínimo | Distancia escalada Z: `R = K · W^(1/3)`, K = 15,4 (letal ≈ 35 kPa) y K = 18 (daño mínimo) |
| Daño estructural (madera/ladrillo/hormigón) | `R = K · W^(1/3)` con K ≈ 5,5 / 4 / 2,9 (≈ 1, 2 y 4–5 psi) |
| Fragmentación (personal al descubierto) | Alcance de fragmentos: `R = 140 · W^(1/3)` (con metralla) o `max(R letal, 6·W^(1/3))` |
| Zona de exclusión (cristales) | `R = 45 · W^(1/3)` |

W = carga en kg de TNT equivalente (NEQ del catálogo × factor de explosivo, en modo IED; o NEQ de la munición, en modo UXO).

---

## 🗂 Estructura del repositorio

```
/
├── index.html   ← la aplicación completa (HTML + CSS + JS, autocontenida)
└── README.md    ← esta guía
```

No hay otros archivos: todo el código está incrustado en `index.html`. Las librerías (Leaflet, html2canvas, jsPDF) se cargan desde CDN público, y el mapa usa teselas de OpenStreetMap.

---

## 🔒 Privacidad

- La posición GPS se procesa **solo en el dispositivo**; no se envía a ningún servidor.
- No hay cuentas, cookies ni telemetría.
- El registro de incidentes vive en memoria (se borra al recargar).

---

## 🛠 Personalización rápida

- **Añadir municiones al catálogo UXO**: en `index.html`, busca el `<select id="uo2">` y añade una opción con el formato `NEQ|Metralla(1/0)|Nombre|URL de referencia` dentro del `optgroup` que corresponda.
- **Cambiar K o umbrales**: edita el array `RANGES` y las constantes al inicio del script.
- **Añadir contenedores IED**: añade opciones al `<select id="cont">` con el formato `Peso kg|Eficiencia|Nombre`.

---

## 📚 Fuentes de referencia

- Catálogo de municiones basado en bases de datos de acceso libre: [cat-uxo.com](https://cat-uxo.com) y [ordata.info](https://ordata.info) (CORD).
- *Explosive Ordnance Guide for Ukraine* — GICHD (3.ª ed., 2025): https://www.gichd.org/publications-resources/publications-and-reports/explosive-ordnance-guide-for-ukraine-third-edition
- Fórmulas de distancias escaladas y umbrales de sobrepresión: literatura EOD/amt munitions safety de acceso público.

---

*PERIMETER GPS · v0.6 · Uso humanitario y formativo*
