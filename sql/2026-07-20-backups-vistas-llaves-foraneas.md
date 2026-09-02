---
Fecha: 2026-07-20
Tema: Copias de seguridad, restauración de bases de datos, vistas y llaves foráneas
---

# 🗓️ 20 de julio de 2026 (Lunes)

> **Tema:** Copias de seguridad, restauración de bases de datos, vistas (Views) y llaves foráneas.

## 📑 Contenido
- [Exportar una base de datos (Backup)](#-exportar-una-base-de-datos-backup)
- [Restaurar una base de datos](#-restaurar-una-base-de-datos)
- [Vistas (Views)](#-vistas-views)
- [Llave foránea (Foreign Key)](#-llave-foránea-foreign-key)
- [Resumen y notas](#-resumen-de-la-clase)

---

## 🔹 Exportar una base de datos (Backup)

```bash
mysqldump -u root -p claseII > copiaClase.sql
```

| Elemento | Explicación |
|---|---|
| `mysqldump` | Herramienta de MySQL para exportar una base de datos |
| `-u root` | Usuario que realiza la exportación |
| `-p` | Solicita la contraseña |
| `claseII` | Nombre de la base de datos a respaldar |
| `>` | Redirecciona la salida hacia un archivo |
| `copiaClase.sql` | Archivo donde queda la estructura y los datos |

**Resultado:** un archivo `.sql` con todas las instrucciones necesarias para reconstruir la base de datos.

## 🔹 Restaurar una base de datos

```sql
source C:\xampp\mysql\bin\copiaClase.sql;
```

`source` ejecuta un archivo `.sql` y recrea tablas, relaciones y registros contenidos en él.

## 🔹 Vistas (Views)

Una **vista** es una tabla virtual creada a partir de una consulta SQL. No almacena datos propios, solo el código SQL que la genera.

```sql
CREATE VIEW consulta1 AS
SELECT c.nombre,
       SUM(cantidad * precio) AS total
FROM clientes c
INNER JOIN pedidos p
    ON c.idCliente = p.idCliente
GROUP BY c.nombre
ORDER BY total DESC
LIMIT 1;
```

| Cláusula | Explicación |
|---|---|
| `CREATE VIEW consulta1 AS` | Crea la vista `consulta1` a partir de la consulta indicada |
| `SUM(cantidad * precio)` | Calcula el valor total de compras por cliente |
| `INNER JOIN` | Une `clientes` y `pedidos` |
| `ON` | Compara la llave primaria con la llave foránea (`c.idCliente = p.idCliente`) |
| `GROUP BY` | Agrupa los pedidos por cliente |
| `ORDER BY total DESC` | Ordena de mayor a menor (`ASC` = menor a mayor) |
| `LIMIT 1` | Muestra solo el cliente con la compra más alta |

## 🔹 Llave foránea (Foreign Key)

Conecta dos tablas mediante un campo en común, garantizando la integridad de los datos.

```sql
FOREIGN KEY (columna) REFERENCES tabla(campo);

-- Ejemplo
FOREIGN KEY (idCliente) REFERENCES clientes(idCliente);
```

El valor almacenado en `idCliente` debe existir previamente en la tabla `clientes`.

## 📝 Resumen de la clase

- `mysqldump` crea respaldos de bases de datos.
- `source` restaura una base de datos desde un archivo `.sql`.
- Las **Views** guardan consultas complejas para reutilizarlas.
- `INNER JOIN` une información de dos tablas relacionadas y `ON` indica la condición de unión.
- `GROUP BY` agrupa registros y `ORDER BY` los organiza; `LIMIT` restringe la cantidad mostrada.
- Las **Foreign Keys** relacionan tablas y mantienen la integridad de los datos.

### 📌 Notas importantes
- Una vista no almacena datos, solo código SQL.
- `ORDER BY DESC` → mayor a menor; `ASC` → menor a mayor.
- Antes de crear una llave foránea, el campo referenciado debe ser `PRIMARY KEY` o `UNIQUE`.

---
⬅️ [Volver al índice de SQL](./README.md)
