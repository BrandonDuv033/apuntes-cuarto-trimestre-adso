---
fecha: 2026-08-04
materia: SQL
tema: MySQL Workbench, funciones de texto, funciones de fecha y funciones matemáticas
---

# 🗓️ 04 de agosto de 2026 (Martes)

> **Tema:** Introducción a MySQL Workbench, funciones de texto, funciones de fecha y funciones matemáticas.

## 📑 Contenido
- [MySQL Workbench](#-qué-es-mysql-workbench)
- [Funciones de texto](#-funciones-de-texto)
- [Funciones de fecha](#-funciones-de-fecha)
- [Funciones matemáticas](#-funciones-matemáticas)
- [Consultas realizadas en clase](#-consultas-realizadas-en-clase)
- [Resumen y notas](#-resumen-de-la-clase)

---

## 🔹 ¿Qué es MySQL Workbench?

Herramienta CASE (*Computer Aided Software Engineering*) de Oracle para diseñar, administrar y consultar bases de datos MySQL de manera gráfica. Permite:

- Diseñar modelos de bases de datos (MER)
- Crear y modificar tablas
- Ejecutar consultas SQL
- Administrar usuarios y permisos
- Exportar e importar bases de datos
- Visualizar relaciones entre tablas

> 💡 El puerto por defecto de **Apache** es `80` (estándar para HTTP).

## 🔹 Funciones de texto

| Función | Descripción | Ejemplo |
|---|---|---|
| `UPPER()` | Convierte a mayúsculas | `Juan Pérez` → `JUAN PÉREZ` |
| `LOWER()` | Convierte a minúsculas | `Juan Pérez` → `juan pérez` |
| `LENGTH()` | Cuenta caracteres | `Carlos` → `6` |
| `TRIM()` | Elimina espacios al inicio/final | `"  Brandon  "` → `Brandon` |
| `LEFT(texto, n)` | Extrae `n` caracteres desde la izquierda | `Brandon` → `Bran` |
| `RIGHT(texto, n)` | Extrae `n` caracteres desde la derecha | `Brandon` → `on` |
| `CONCAT()` | Une varios textos | `Brandon` + `Soacha` → `Brandon es de Soacha` |

```sql
SELECT UPPER(nombreCliente) FROM clientes;
SELECT LOWER(nombreCliente) FROM clientes;
SELECT LOWER(UPPER(nombreCliente)) FROM clientes;   -- se pueden combinar
SELECT LENGTH(nombreCliente) FROM clientes;
SELECT TRIM("     Brandon     ");
SELECT TRIM(CONCAT("      ", nombreCliente, " es de ", ciudad));
SELECT LEFT(nombreCliente,4);
SELECT RIGHT(nombreCliente,2);
SELECT CONCAT(nombreCliente," es de ",ciudad);
```

**Ejemplo práctico — generar un correo institucional:**
```sql
SELECT CONCAT(
    LEFT(nombreCliente,4),
    RIGHT(fechaRegistro,2),
    "@soysena.edu.co"
) AS Correos
FROM clientes;
-- Nombre: Brandon | Fecha: 2025-06-15 → Bran15@soysena.edu.co
```

## 🔹 Funciones de fecha

| Función | Descripción | Ejemplo |
|---|---|---|
| `CURDATE()` | Fecha actual | `2026-08-04` |
| `NOW()` | Fecha y hora actual | `2026-08-04 10:35:28` |
| `DATEDIFF(f1, f2)` | Diferencia en días entre dos fechas | `120` |
| `DAYNAME()` | Nombre del día | `Monday` |
| `DAYOFMONTH()` | Número del día del mes | `15` |
| `DAYOFWEEK()` | Número del día de la semana (1=Domingo … 7=Sábado) | ver tabla abajo |
| `DAYOFYEAR()` | Día del año | `216` |
| `MONTH()` | Número del mes | `8` |
| `MONTHNAME()` | Nombre del mes | `August` |
| `YEAR()` | Año | `2026` |
| `DAY()` | Número del día | — |
| `TIME()` | Extrae la hora | `10:40:58` |
| `LAST_DAY()` | Último día del mes de la fecha dada | `2026-08-31` |
| `DATE_ADD()` | Suma tiempo a una fecha | — |
| `DATE_SUB()` | Resta tiempo a una fecha | — |

```sql
SELECT CURDATE();
SELECT NOW();
SELECT DATEDIFF(CURDATE(), fechaRegistro);
SELECT DAYNAME(fechaRegistro);
SELECT DAYOFMONTH(fechaRegistro);
SELECT DAYOFWEEK(fechaRegistro);
SELECT DAYOFYEAR(fechaRegistro);
SELECT MONTH(fechaRegistro);
SELECT MONTHNAME(fechaRegistro);
SELECT YEAR(fechaRegistro);
SELECT DAY(fechaRegistro);
SELECT TIME(NOW());
SELECT LAST_DAY("2026-08-04");   -- Resultado: 2026-08-31
```

> 📌 Aunque en clase se mencionó "último día del mes anterior", `LAST_DAY()` realmente devuelve el último día del **mes correspondiente** a la fecha dada.

**Valores de `DAYOFWEEK()`:**

| Número | Día |
|---|---|
| 1 | Domingo |
| 2 | Lunes |
| 3 | Martes |
| 4 | Miércoles |
| 5 | Jueves |
| 6 | Viernes |
| 7 | Sábado |

**`DATE_ADD()` / `DATE_SUB()`:**
```sql
SELECT DATE_ADD(NOW(), INTERVAL 20 DAY);
SELECT DATE_ADD(NOW(), INTERVAL 30 MINUTE);
SELECT DATE_ADD(NOW(), INTERVAL '10-05' YEAR_MONTH);  -- suma 10 años y 5 meses
SELECT DATE_SUB(NOW(), INTERVAL 8 YEAR);
```

## 🔹 Funciones matemáticas

| Función | Descripción | Ejemplo |
|---|---|---|
| `ABS()` | Valor absoluto | `ABS(-9)` → `9` |
| `CEIL()` | Redondea hacia arriba | `CEIL(7.35)` → `8` |
| `FLOOR()` | Redondea hacia abajo | `FLOOR(7.35)` → `7` |
| `PI()` | Valor de π | `3.14159265...` |
| `RAND()` | Número aleatorio | `SELECT ROUND(RAND(),2)` → `0.57` |
| `SQRT()` | Raíz cuadrada | `SQRT(16)` → `4` |
| `TRUNCATE(n, d)` | Elimina decimales sin redondear | `TRUNCATE(7.91,1)` → `7.9` |

## 🔹 Consultas realizadas en clase

**1. Nombre en mayúsculas, ciudad en minúsculas y año del pedido:**
```sql
SELECT
    UPPER(c.nombreCliente) AS Nombre,
    LOWER(UPPER(c.ciudad)) AS Ciudad,
    YEAR(p.fechaPedido) AS Año
FROM clientes c
INNER JOIN pedidos p
    ON c.idClientes = p.idClientes;
```

**2. Pedidos realizados en enero:**
```sql
SELECT
    c.nombreCliente, p.fechaPedido,
    MONTH(p.fechaPedido) AS Mes
FROM clientes c
INNER JOIN pedidos p
    ON c.idClientes = p.idClientes
WHERE MONTH(p.fechaPedido) = 1;
```

**3. Cliente, producto comprado y cantidad vendida (4 tablas):**
```sql
SELECT
    c.nombre_cliente, p.desc_producto, dp.cantidad_vendida
FROM cliente c
INNER JOIN comprobante co
    ON c.id_cliente = co.cod_cli
INNER JOIN detalle_comprobante dp
    ON co.cod_comprobante = dp.cod_comprobante
INNER JOIN productos p
    ON p.id_producto = dp.id_producto;
```

**4. Nombre del cliente y número de mes del comprobante:**
```sql
SELECT
    c.nombre_cliente,
    MONTH(co.fecha) AS "Número de Mes"
FROM cliente c
INNER JOIN comprobante co
    ON c.id_cliente = co.cod_cli;
```

**5. Productos con precio mayor al promedio:**
```sql
SELECT
    desc_producto AS Nombre, precio
FROM productos
WHERE precio >
(
    SELECT AVG(precio)
    FROM productos
);
```

## 📝 Resumen de la clase

- MySQL Workbench es la herramienta gráfica de Oracle para administrar bases de datos MySQL.
- Las funciones de texto modifican, unen y extraen caracteres.
- Las funciones de fecha obtienen día, mes, año, hora o diferencias entre fechas.
- Las funciones matemáticas hacen redondeos, raíces, valores absolutos y números aleatorios.
- Todas se pueden combinar con `SELECT`, `WHERE`, `JOIN` y subconsultas.

### 📌 Notas importantes
- `UPPER()` / `LOWER()` solo cambian **cómo se muestran** los datos, no lo que está almacenado.
- `DATE_ADD()` / `DATE_SUB()` admiten `DAY`, `MONTH`, `YEAR`, `MINUTE` o combinaciones como `YEAR_MONTH`.
- `RAND()` genera un número distinto en cada ejecución; combínalo con `ROUND()` para limitar decimales.
- `TRUNCATE()` **no** redondea (solo elimina decimales); `ROUND()` sí redondea.

---
⬅️ [Volver al índice de SQL](./README.md)
