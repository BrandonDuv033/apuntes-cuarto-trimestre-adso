---
Fecha: 2026-07-24
Tema: Relaciones entre tablas, funciones de agregado, vistas y modificación de tablas existentes
---

# 🗓️ 24 de julio de 2026 (Viernes)

> **Tema:** Relaciones entre tablas, funciones de agregado, vistas y modificación de tablas existentes.

## 📑 Contenido
- [Relaciones atípicas](#-relaciones-atípicas)
- [CONCAT()](#-concat)
- [Funciones de agregado](#-funciones-de-agregado)
- [HAVING](#-having)
- [Ver el código de una vista](#-mostrar-el-código-de-una-vista)
- [ROUND()](#-round)
- [Agregar una llave foránea a una tabla existente](#-agregar-una-llave-foránea-a-una-tabla-existente)
- [Resumen y notas](#-resumen-de-la-clase)

---

## 🔹 Relaciones atípicas

Relaciones que no siguen la estructura tradicional entre entidades.

> ❌ **No** realizar relaciones circulares: cuando varias tablas dependen unas de otras en forma de ciclo, dificultando insertar, modificar y eliminar datos.

## 🔹 CONCAT()

Une varios textos en un solo resultado.

```sql
CONCAT(nombre_proveedor, " ", apellidos_proveedor)
```

Si `nombre = Juan` y `apellido = Pérez` → resultado: `Juan Pérez`.

## 🔹 Funciones de agregado

| Función | Descripción |
|---|---|
| `SUM()` | Suma valores numéricos |
| `COUNT()` | Cuenta registros |
| `MIN()` | Obtiene el valor mínimo |
| `MAX()` | Obtiene el valor máximo |
| `AVG()` | Calcula el promedio |

```sql
SELECT SUM(precio) FROM productos;
SELECT COUNT(*) FROM clientes;
SELECT AVG(precio) FROM productos;
```

## 🔹 HAVING

Filtra resultados obtenidos mediante funciones de agregado (a diferencia de `WHERE`, que filtra registros individuales).

**Orden correcto de una consulta:**
```sql
SELECT ...
FROM ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
```

| Cláusula | Filtra... |
|---|---|
| `WHERE` | Registros, **antes** del agrupamiento |
| `HAVING` | Grupos, **después** del `GROUP BY` |

## 🔹 Mostrar el código de una vista

```sql
SHOW CREATE VIEW nombreVista;
-- Ejemplo
SHOW CREATE VIEW consulta1;
```

> Una vista es una tabla virtual basada en una consulta SQL, que permite reutilizar consultas complejas sin reescribirlas.

## 🔹 ROUND()

```sql
SELECT ROUND(15.678,2);
-- Resultado: 15.68
```

### ON
La cláusula `ON` establece la condición con la que se relacionan dos tablas en un `JOIN` (generalmente compara una llave primaria con una foránea):
```sql
ON proveedores.codCiudad = ciudades.codCiudad
```

## 🔹 Agregar una llave foránea a una tabla existente

**Paso 1 — Agregar el campo:**
```sql
ALTER TABLE proveedores
ADD COLUMN codCiudad INT;
```

**Paso 2 — Crear la llave foránea:**
```sql
ALTER TABLE proveedores
ADD FOREIGN KEY (codCiudad)
REFERENCES ciudades(codCiudad);
```

**Paso 3 — Actualizar los registros existentes** (quedan en `NULL` tras crear la relación):
```sql
UPDATE proveedores SET codCiudad = 1 WHERE cod_proveedor = 1;
```

## 📝 Resumen de la clase

- Evitar relaciones circulares entre tablas.
- `CONCAT()` une varios textos en uno solo.
- Las funciones de agregado operan sobre conjuntos de datos.
- `HAVING` filtra resultados después de agruparlos.
- `SHOW CREATE VIEW` muestra el código SQL de una vista.
- `ROUND()` redondea números decimales.
- `ON` define la condición de relación entre tablas.
- Una llave foránea puede agregarse posteriormente con `ALTER TABLE`.

### 📌 Notas importantes
- `WHERE` filtra antes del `GROUP BY`; `HAVING` filtra después.
- Las funciones de agregado suelen usarse junto con `GROUP BY`.
- Siempre crea primero la columna antes de agregarle una `FOREIGN KEY`.
- Al agregar una FK a una tabla con registros existentes, normalmente hay que actualizarlos con `UPDATE` para reemplazar los `NULL`.

---
⬅️ [Volver al índice de SQL](./README.md)
