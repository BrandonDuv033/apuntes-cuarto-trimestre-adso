---
fecha: 2026-08-14
materia: SQL
tema: Procedimientos almacenados - CRUD con CASE y variables con DECLARE
---

# 🗓️ 14 de agosto de 2026

## 📑 Contenido
- [Procedure CRUD](#-procedure-crud)
- [Parámetros IN](#-parámetros-in)
- [Ejecutar un Procedure](#-ejecutar-un-procedure)
- [Procedure con DECLARE](#-procedure-con-declare)

---

## 🔹 Procedure CRUD

Un procedimiento almacenado que realiza las cuatro operaciones básicas:

| Letra | Operación | Comando SQL |
|---|---|---|
| **C** | Crear | `INSERT` |
| **R** | Leer | `SELECT` |
| **U** | Actualizar | `UPDATE` |
| **D** | Eliminar | `DELETE` |

```sql
CREATE PROCEDURE CRUD(
    IN procedimiento INT,
    IN id VARCHAR(30),
    IN nombre VARCHAR(30),
    IN ruc VARCHAR(30),
    IN tipo VARCHAR(30),
    IN dir VARCHAR(30),
    IN tel VARCHAR(30)
)
BEGIN

CASE procedimiento

    WHEN 1 THEN
        INSERT INTO cliente
        VALUES (id, nombre, ruc, tipo, dir, tel);

    WHEN 2 THEN
        SELECT *
        FROM cliente
        WHERE id_cliente = id;

    WHEN 3 THEN
        UPDATE cliente
        SET nombre_cliente = nombre,
            tipo_cliente = tipo,
            direccion = dir
        WHERE id_cliente = id;

    WHEN 4 THEN
        DELETE FROM cliente
        WHERE id_cliente = id;

END CASE;

END;
```

## 🔹 Parámetros IN

`IN` indica que el procedimiento recibe un dato como entrada.

```sql
IN id VARCHAR(30)
-- El procedimiento recibe un id de tipo VARCHAR
```

## 🔹 Ejecutar un Procedure

```sql
CALL CRUD(...);
```

El número de procedimiento determina la operación:

| Número | Operación |
|---|---|
| 1 | `INSERT` |
| 2 | `SELECT` |
| 3 | `UPDATE` |
| 4 | `DELETE` |

## 🔹 Procedure con DECLARE

```sql
CREATE PROCEDURE contarVendedores()
BEGIN
    DECLARE numVent INT DEFAULT 0;

    SELECT COUNT(*)
    INTO numVent
    FROM vendedor;

    SELECT numVent;
END
```

| Palabra clave | Función |
|---|---|
| `DECLARE` | Declara una variable dentro del procedimiento |
| `DEFAULT 0` | Establece su valor inicial |
| `INTO` | Guarda el resultado de una consulta dentro de una variable |

En este ejemplo, `SELECT COUNT(*) INTO numVent FROM vendedor;` cuenta los vendedores y guarda el resultado en `numVent`.

---
⬅️ [Volver al índice de SQL](./README.md)
