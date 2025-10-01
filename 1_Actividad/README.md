# Arquitectura de Datos

Este repositorio contiene los trabajos realizados para la materia **Arquitectura de Datos**.

---

## 🛠️ Transformación de Datos con Power BI

Se trabajó con una base de datos proveniente de la **Evaluación de Tierras para el Cultivo Tecnificado de Maíz (Zea Mays L.)** en el departamento de **Guaviare**.

### 📊 Detalles de la Base de Datos

- **Filas:** 3,589  
- **Columnas:** 9

### 🧾 Columnas Originales

- `Objetic`  
- `Municipio`  
- `Departamento`  
- `Código departamento`  
- `Código municipio`  
- `Gridcode`  
- `Área (ha)`  
- `Aptitud`  
- `Consecutivo`

### 🔄 Transformaciones Realizadas

- ❌ Se eliminó la columna `Objetic`, ya que solo servía como enumerador temporal al extraer los datos.
- ✅ En la columna `Área (ha)`, se reemplazaron los **puntos (.)** por **comas (,)** para un formato numérico correcto.
- 🔢 Se transformaron las siguientes columnas para asegurar un formato consistente:
  - `Código departamento`: convertido a **entero**.
  - `Código municipio`: convertido a **entero**.
  - `Gridcode`: convertido a **entero**.
  - `Consecutivo`: convertido a **decimal**.

---

## 📈 Visualizaciones en Power BI

Se generaron las siguientes gráficas para el análisis de los datos:

1. **📊 Gráfico de Barras**  
   - Recuento de **Área (ha)** por **Municipio**.

2. **🥧 Gráfico Circular (Torta)**  
   - Distribución de **Aptitud** por **Municipio**.

---
