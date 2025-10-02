# 📊 Transformación de Datos con Power BI

Se revisaron **7 archivos CSV** y se realizaron procesos de limpieza, normalización y transformación en **Power Query**.

---

## 📂 Archivos procesados

### 1️⃣ Contactos_Clientes  
**Columnas originales:** `ID, Nombre completo, Correo, Teléfono, País`

**Transformaciones realizadas:**
- ✔️ Se colocó la primera fila como encabezados.  
- ✔️ Reorganización de datos mal distribuidos entre filas y columnas.  
- ✔️ Creación de **columnas personalizadas**:  
  - **Correo.1**  
    ```powerquery
    if Text.Contains([Correo], "@") then [Correo] 
    else [Correo] & [Telefono]
    ```
  - **Telefono.1**  
    ```powerquery
    if Text.Contains([Correo], "@") then [Telefono] 
    else [Pais]
    ```
  - **Pais.1**  
    ```powerquery
    let
      paisesValidos = {"Colombia", "México", "Perú", "Chile", "Argentina", "Ecuador", "Venezuela", "Guatemala"},
      paisCorregido = 
        if List.Contains(paisesValidos, [Pais]) then [Pais]
        else if List.Contains(paisesValidos, [Telefono]) then [Telefono]
        else if List.Contains(paisesValidos, [Correo]) then [Correo]
        else if List.Contains(paisesValidos, [Columna_extra]) then [Columna_extra]
        else null
    in
      paisCorregido
    ```
- ❌ Eliminación de columnas innecesarias.  
- ❌ Eliminación de la fila con ID `NO3` (más de un campo nulo).  
- ❌ Eliminación de la columna `ID`.  
- 🔄 Conversión de `Correo`, `Teléfono` y `País` a tipo **texto**.  

---

### 2️⃣ Contactos_Empleados  
**Columnas originales:** `Legajo, Nombre, e-mail, Celular, País`

**Transformaciones realizadas:**
- ✔️ Se colocó la primera fila como encabezados.  
- ✔️ Reorganización de datos mal distribuidos.  
- ✔️ Creación de **columnas personalizadas**:  
  - **Correo.1** (renombrando `e-mail` a `Correo`)  
    ```powerquery
    if Text.Contains([Correo], "@") then [Correo]
    else [Correo] & [Celular]
    ```
  - **Celular.1**  
    ```powerquery
    let
      limpio = Text.Select([Celular], {"0".."9"}),
      result = if Text.Length(limpio) = 0 then null
               else if Text.Length(limpio) > 10 then Text.End(limpio, 10)
               else limpio
    in
      result
    ```
  - **Pais.1** (similar validación de países que en `Contactos_Clientes`).  
- 🔄 Conversión de `Correo`, `Celular` y `País` a **texto**.  
- 🔄 Reemplazo de valores `na` por `n/a` en columna `Nombre`.  
- ❌ Eliminación de columnas innecesarias.  

---

### 3️⃣ Contactos_Proveedores  
**Columnas originales:** `id, RazonSocial, Email, Tel, Country`

**Transformaciones realizadas:**
- ✔️ Encabezados correctos.  
- ✔️ Creación de **columnas personalizadas**:  
  - **Email.1**  
    ```powerquery
    if Text.Contains([Email], "@") then [Email]
    else [Email] & [Tel]
    ```
  - **Tel.1**  
    ```powerquery
    let
      limpioTel = Text.Select([Tel], {"0".."9"}),
      limpioCountry = Text.Select([Country], {"0".."9"}),
      base = if Text.Length(limpioTel) > 0 then limpioTel else limpioCountry,
      result = if Text.Length(base) = 0 then null
               else if Text.Length(base) > 10 then Text.End(base, 10)
               else base
    in
      result
    ```
  - **Pais.1** (validación con abreviaciones: `CO, MX, PE, CL...`).  
- 🔄 Conversión de `Email`, `Tel` y `Country` a **texto**.  
- ❌ Eliminación de columnas innecesarias.  

---

### 4️⃣ Productos-erp  
**Columnas originales:** `ProductID, Nombre, Categoría, PrecioLista, Costo, Activo, SKU, Columna_irrelevante`

**Transformaciones realizadas:**
- ❌ Eliminación de la columna **Columna_irrelevante**.  
- 📑 Duplicación de tabla para proceso de anexado.  
- ✂️ Delimitación de `ProductID` por `,` en la tabla duplicada.  
- 🔄 Renombrado de columnas para mantener consistencia.  
- 🧹 Filtrado de filas innecesarias en ambas tablas.  
- 📎 Anexado de tablas restantes.  
- 🔄 Normalización de columna `Activo`:  
  - Reemplazo de `TRUE`, `Yes`, `yes` → `Sí`.  
  - Conversión a minúsculas para limpieza rápida.  
- 👁️ Tabla auxiliar no eliminada (pero marcada como **no visible en el modelo**).  

---

### 5️⃣ Ventas_detalle  
*(Procesada en conjunto con las tablas de ventas)*  

### 6️⃣ Ventas_mensuales  
*(Procesada en conjunto con las tablas de ventas)*  

### 7️⃣ Ventas_ordenes  
*(Procesada en conjunto con las tablas de ventas)*  

---

## ✅ Resultado Final
- Tablas **limpias y normalizadas**.  
- Corrección de inconsistencias en **correo, teléfono y país**.  
- Estandarización de variables a **tipo texto**.  
- Modelo de datos optimizado y listo para análisis en **Power BI**.  
