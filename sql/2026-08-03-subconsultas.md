---
fecha: 2026-08-03
materia: SQL
tema: Subconsultas, funciones de agregado y consultas con múltiples condiciones
---

# 🗓️ 03 de agosto de 2026 (Lunes)

> **Tema:** Subconsultas, funciones de agregado y consultas con múltiples condiciones.

## 📑 Contenido
- [¿Qué es una subconsulta?](#-qué-es-una-subconsulta)
- [Sintaxis general](#-sintaxis-general)
- [Ejemplos con un solo valor](#-ejemplos-con-un-solo-valor)
- [Combinando condiciones con AND](#-dos-condiciones-and)
- [Subconsultas con fechas](#-comparar-fechas-con-el-promedio)
- [Más ejemplos prácticos](#-más-ejemplos-prácticos)
- [Operador ALL](#-operador-all)
- [Resumen y notas](#-resumen-de-la-clase)

---

## 🔹 ¿Qué es una subconsulta?

Una **subconsulta (subquery)** es una consulta SQL dentro de otra consulta. Su resultado es utilizado por la consulta principal para comparar o aplicar condiciones: primero se ejecuta la subconsulta y, con ese resultado, se ejecuta la consulta principal.

**¿Cuándo se utilizan?** Cuando el enunciado plantea más de una pregunta. Ejemplo: *"Mostrar los productos cuyo precio sea igual al precio más alto registrado"* → primero hay que saber cuál es el precio más alto, y luego buscar el producto que lo tiene.

## 🔹 Sintaxis general

```sql
SELECT columna1, columna2...
FROM tabla
WHERE columna OPERADOR
(
    SELECT columna
    FROM tabla
    WHERE condición
);
```

## 🔹 Ejemplos con un solo valor

**Precio más alto:**
```sql
SELECT nombreProducto
FROM productos
WHERE precio =
(
    SELECT MAX(precio)
    FROM productos
);
```

**Fecha de registro más antigua:**
```sql
SELECT MIN(fechaRegistro)
FROM clientes;
```

**Clientes registrados en la fecha más antigua:**
```sql
SELECT nombreCliente
FROM clientes
WHERE fechaRegistro =
(
    SELECT MIN(fechaRegistro)
    FROM clientes
);
```

**Producto con menor stock de la categoría "Muebles":**
```sql
SELECT nombreProducto, stock
FROM productos
WHERE stock =
(
    SELECT MIN(stock)
    FROM productos
)
AND categoria = "Muebles";
```

## 🔹 Dos condiciones (AND)

```sql
SELECT nombreProducto, categoria
FROM productos
WHERE categoria = "Tecnologia"
AND precio >
(
    SELECT AVG(precio)
    FROM productos
);
```
Se muestran solo los productos de categoría **Tecnología** con precio mayor al promedio general. Ambas condiciones deben cumplirse (`AND`).

**Con una tercera condición (stock):**
```sql
SELECT nombreProducto, categoria, stock
FROM productos
WHERE categoria = "Tecnologia"
AND precio >
(
    SELECT AVG(precio)
    FROM productos
)
AND stock > 20;
```

### Funciones de agregado usadas en subconsultas

| Función | Descripción |
|---|---|
| `MAX()` | Valor máximo |
| `MIN()` | Valor mínimo |
| `AVG()` | Promedio |
| `SUM()` | Suma |
| `COUNT()` | Cantidad de registros |

## 🔹 Comparar fechas con el promedio

Las fechas no pueden promediarse directamente: se convierten a *Unix Timestamp*.

```sql
SELECT c.nombreCliente, p.fechaPedido
FROM clientes c
INNER JOIN pedidos p
    ON c.idClientes = p.idClientes
WHERE p.fechaPedido >
(
    SELECT FROM_UNIXTIME(AVG(UNIX_TIMESTAMP(fechaPedido)))
    FROM pedidos
);
```

| Función | Explicación |
|---|---|
| `UNIX_TIMESTAMP()` | Convierte una fecha en un número entero |
| `AVG()` | Calcula el promedio de esos valores numéricos |
| `FROM_UNIXTIME()` | Convierte nuevamente ese número en una fecha |

**Clientes registrados después de la fecha promedio (versión simplificada):**
```sql
SELECT nombreCliente, ciudad, fechaRegistro
FROM clientes
WHERE fechaRegistro >
(
    SELECT AVG(fechaRegistro)
    FROM clientes
);
```
> Nota: en MySQL normalmente las fechas se convierten con `UNIX_TIMESTAMP()` y `FROM_UNIXTIME()` para calcular el promedio correctamente.

## 🔹 Más ejemplos prácticos

**Precio mayor que la suma de otra categoría:**
```sql
SELECT nombreProducto, precio
FROM productos
WHERE precio >
(
    SELECT SUM(precio)
    FROM productos
    WHERE categoria = "Accesorios"
);
```
> ⚠️ En el código visto en clase aparecía el operador `<`, pero según el enunciado ("mayor a la suma") corresponde `>`.

**Clientes cuyos pedidos fueron posteriores a la fecha promedio (con JOIN):**
```sql
SELECT c.nombreCliente, p.fechaPedido
FROM clientes c
INNER JOIN pedidos p
    ON c.idClientes = p.idClientes
WHERE p.fechaPedido >
(
    SELECT FROM_UNIXTIME(AVG(UNIX_TIMESTAMP(fechaPedido)))
    FROM pedidos
);
```
Combina `INNER JOIN` + subconsulta + función de promedio + conversión de fechas.

## 🔹 Operador ALL

Compara un valor con **todos** los valores devueltos por una subconsulta.

```sql
SELECT nombreProducto, stock, categoria
FROM productos
WHERE stock > ALL
(
    SELECT stock
    FROM productos
    WHERE categoria = "Tecnologia"
);
```
Muestra los productos cuyo stock es mayor que **todos** los stocks de la categoría Tecnología.

> ⚠️ En el código visto en clase, la condición `categoria = "Tecnologia"` quedó fuera de la subconsulta, lo que cambia el resultado; la versión anterior es la correcta según el enunciado.

## 📝 Resumen de la clase

- Una subconsulta es una consulta dentro de otra consulta y siempre va entre paréntesis.
- Se usa cuando primero es necesario obtener un dato para luego realizar otra consulta.
- Puede combinarse con `=`, `>`, `<`, `IN`, `ANY`, `ALL`, `EXISTS`.
- Las funciones de agregado (`MAX`, `MIN`, `AVG`, `SUM`, `COUNT`) son muy usadas dentro de subconsultas.
- Para promediar fechas: `UNIX_TIMESTAMP()` + `FROM_UNIXTIME()`.
- Las subconsultas pueden combinarse con `AND`, `OR`, `JOIN` y otras cláusulas.

### 📌 Notas importantes
- Primero se ejecuta la subconsulta y luego la consulta principal.
- Cuando el enunciado plantea más de una pregunta ("buscar el promedio y luego comparar"), probablemente se necesite una subconsulta.
- Revisa que el operador (`>`, `<`, `=`) coincida con lo que pide el enunciado.

---
⬅️ [Volver al índice de SQL](./README.md)
