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
- Se cambian las abreviaciones para que aparesca el country completo.

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
**Columnas originales:** `OrderID, ProductoID, Cantidad, Precio_Unitario, "" `
📑 Duplicación de tabla para proceso de anexado.  
- ✂️ Delimitación de `ProductoID` por `,` en la tabla duplicada.  
- 🔄 Renombrado de columnas para mantener consistencia.  
- 🧹 Filtrado de filas innecesarias en ambas tablas.  
- 📎 Anexado de tablas restantes.  
- ❌ Eliminación de la columna **""**.  

### 6️⃣ Ventas_mensuales  
**Columnas originales:** ` ProductoID,  año: De enero hasta dicimiembre`
- ❌ Eliminación de las primeras 1 filas ya que no aportan nada.
- Colocar la nueva primera fila como  encabezados
- Crear una Columna personalizada para diciembre para que contenga todos los datos pertinentes:
if [Dic] = "-" or [Dic] = null then 
    if [Column15] <> null then [Column15] 
    else [Column16] 
else [Dic]
-Cambiar los meses de enero a diciembre por decimales
-Ajustar aquellos casillas que que contengan diferentes tipo de valor para que concuerde en la transformacion
📑 Duplicación de tabla para proceso de anexado.  
- ✂️ Delimitación de `ProductoID` por `,` en la tabla duplicada.  
- 🔄 Renombrado de columnas para mantener consistencia.  
- 🧹 Filtrado de filas innecesarias en ambas tablas.  
- 📎 Anexado de tablas restantes.  


### 7️⃣ Ventas_ordenes 
**Columnas originales:** ` OrderID, CustomerID, Total, Estado, Fecha`
📑 Duplicación de tabla para proceso de anexado.  
- ✂️ Delimitación de `OrderID` por `,` en la tabla duplicada.  
- 🔄 Renombrado de columnas para mantener consistencia.  
- 🧹 Filtrado de filas innecesarias en ambas tablas.  
- 📎 Anexado de tablas restantes.
- Configurar las fecha en columnas distintas mes, año, dia
----------------------

Unificar las tablas de empleados, Clientes y Proveedores en una tabla que se llama contactos
  - Se adecuan todas las columnas para que tengan las misma cantidad de columnas y tambien que contengan los mismos nombres

Se relacionan las tablas venta_detalles, ventas_mesuales y productos ERP
  <img width="906" height="502" alt="image" src="https://github.com/user-attachments/assets/269706ec-e84e-4515-b11f-3539623734bc" />
Primera Relacion
  <img width="906" height="502" alt="image" src="https://github.com/user-attachments/assets/937b30c5-e86b-4a5e-b6bc-80e8838bb04f" />
Segunda Relacion
  <img width="906" height="502" alt="image" src="https://github.com/user-attachments/assets/1b842594-92dd-4a8a-8f04-fb6cbfb0de76" />


