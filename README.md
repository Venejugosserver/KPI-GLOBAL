# Tablero macro de cumplimiento por marca · Venejugos

Réplica a escala macro del tablero de cumplimiento de KPI Nestlé. Usa la misma arquitectura
(tres páginas HTML autocontenidas, un `dataset.json` y publicación en GitHub Pages), pero evalúa
**todo el portafolio** y toma como dimensión la **marca** (columna `DescrMarca` del Reporte General)
en lugar de la subcategoría Nestlé.

| Archivo | Para qué sirve |
|---|---|
| `admin.html` | Panel administrativo. Llave, carga del Excel, targets y cuotas por marca, estructura comercial. |
| `index.html` | Panel macro: cobertura, volumen, proyección, Pareto de marcas y mix por categoría. |
| `performance.html` | Performance vendedores: activación por marca, por supervisor y vendedor. |
| `dataset.json` | Datos del último reporte procesado, con los parámetros incluidos. |

Chart.js y SheetJS van incrustados: no requiere instalación ni conexión a internet.

---

## Qué cambia frente al modelo Nestlé

| Modelo Nestlé | Modelo macro |
|---|---|
| Solo filas Nestlé (las demás se descartan) | Todas las filas del archivo |
| Dimensión: `DescrSubCategoría` | Dimensión: `DescrMarca` (respaldo: `CodMarca`) |
| 5 categorías foco fijas en el código | Marcas foco elegidas en el panel; por defecto las 5 de mayor venta |
| Targets fijos por categoría | Target por marca editable; las marcas sin target usan el target por defecto (25%) |
| Cuotas de kilos fijas | Cuotas por marca y por asesor en US$ y en kilos, editables |
| Maestros general, Confites y base Nestlé | Un maestro de clientes de todo el portafolio |
| Volumen en kilos | Volumen en US$ por defecto, conmutable a kilos en cada página |
| Parámetros solo en el navegador del administrador | Los parámetros viajan dentro de `dataset.json` |

Visuales nuevas propias de la vista macro: marcas con venta, profundidad (marcas promedio por
cliente activo), venta por cliente activo, concentración (cuántas marcas hacen el 80% de la venta),
curva de Pareto por marca, mix por categoría (`DescrCategoría`) y rolling por marca.

## 1. Panel administrativo (`admin.html`)

1. Abra `admin.html` e introduzca la llave: **DIGIMARKET2026**
2. Cargue el `.xlsx` del Reporte General (botón o arrastrando el archivo).
3. **Parámetros generales**: maestro de clientes (por defecto 1.562, la suma de las carteras),
   target por defecto, medida de volumen con la que abren las páginas y fecha de corte.
4. **Marcas del portafolio**: una fila por marca encontrada, ordenada por venta.
   - Marque las marcas foco. Sin ninguna marcada se toman las cinco de mayor venta.
   - Escriba el target de activación y las cuotas en US$ y kilos.
   - **Precargar cuotas vacías con la proyección** propone como cuota la proyección de cierre
     al ritmo actual; úsela como punto de partida y ajuste.
5. **Estructura comercial**: supervisor, cartera y cuotas por asesor. Viene precargada con la
   estructura del modelo Nestlé; los asesores del archivo sin cartera aparecen marcados.
6. Pulse **Guardar parámetros** y luego **Publicar dataset.json** o **Descargar dataset.json**.

## 2. Panel macro (`index.html`)

Filtros de zona, vendedor, categoría y medida de volumen.

- **Paso 1**: activación de las marcas foco contra su target, y su volumen contra cuota.
- **Paso 2**: activación de todas las marcas y cuadro de cumplimiento con participación.
- **Paso 3**: clientes activados y no activados sobre el maestro.
- Pareto de participación, mix por categoría, zonas y vendedores con mayor aporte, resumen ejecutivo.

## 3. Performance vendedores (`performance.html`)

Selector de **marca de activación** (todas o una marca), vendedor, medida y orden.
Pestañas: equipos y marcas, clientes activados (con CSV), rolling de volumen por marca y por
asesor, y déficit semanal en volumen y en cobertura.

---

## Reglas de cálculo

- **Activación**: cliente con venta mayor a cero en la marca, sobre el maestro de clientes.
  Con filtro de vendedor la base es su cartera; con filtro de zona, los clientes atendidos en ella.
- **Meta por asesor**: cartera asignada × target de la marca (100% en "todas las marcas").
- **Proyección**: volumen del período × (días hábiles del mes ÷ días hábiles transcurridos).
- **Clientes faltantes**: target × universo − clientes ya activados.
- **Profundidad**: promedio de marcas distintas compradas por cada cliente activo.
- **Documentos**: igual que el modelo Nestlé. Las notas de crédito de tesorería (NCTESO, NCADM)
  se excluyen; las ligadas a una factura netean monto y kilos pero no activan.
- **Mes evaluado**: el de la fecha más reciente del archivo; las líneas de otros meses se dejan fuera.

### Columnas que lee del Excel

`DescrMarca`, `DescrCategoría`, `DescrSubCategoría`, `FechaFactura`, `CódigoCliente`, `RazónSocial`,
`RucCliente`, `DescZona`, `NombreVend`, `NombSuperv`, `Kilos`, `Cantidad`, `PrecioTotal`, `NúmeroFactura`.

---

## 4. Publicación en GitHub

Use un **repositorio o carpeta distinta** a la del tablero Nestlé: los dos esperan un
`dataset.json` en su raíz con formatos diferentes. Si el macro encuentra un `dataset.json` del
modelo Nestlé lo rechaza y lo indica en la cinta superior.

```bash
git init
git add .
git commit -m "Tablero macro de cumplimiento por marca - Venejugos"
git branch -M main
git remote add origin https://github.com/USUARIO/REPOSITORIO.git
git push -u origin main
```

En GitHub: **Settings → Pages → Source: Deploy from a branch → main / (root)**.

La publicación con token funciona igual que en el modelo Nestlé (tarjeta **Publicar en GitHub**,
token de alcance fino con *Contents: Read and write*). Las llaves de almacenamiento del navegador
llevan el prefijo `vjm_`, así que ambos tableros pueden convivir en el mismo dominio sin pisarse.

## 5. Actualizar cada mes

1. Cargue el nuevo `.xlsx` en el panel administrativo.
2. Revise marcas nuevas, foco, targets, cuotas y asesores sin cartera.
3. **Guardar parámetros** y **Publicar dataset.json**.

---

Venejugos C.A. · Ventas 0412 501-6394 · @Venejugos
