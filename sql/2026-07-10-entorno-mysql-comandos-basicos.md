---
Fecha: 2026-07-10
Tema: Instalación del entorno de trabajo, conexión a MySQL y comandos básicos
---

# 🗓️ 10 de julio de 2026 (Viernes)

> **Tema:** Instalación del entorno de trabajo, conexión a MySQL y comandos básicos para crear y manipular bases de datos.

## 📑 Contenido
- [¿Qué es XAMPP?](#-qué-es-xampp)
- [Otros servidores de aplicaciones](#-otros-servidores-de-aplicaciones)
- [¿Qué es SQL?](#-qué-es-sql)
- [Tipos de bases de datos](#-tipos-de-bases-de-datos)
- [Conexión a MySQL desde la consola](#-conexión-a-mysql-desde-la-consola)
- [Comandos DML](#-comandos-sql-dml)
- [Registrar comandos con `tee`](#-registrar-todos-los-comandos-con-tee)
- [Comandos básicos de base de datos](#-comandos-básicos-de-base-de-datos)
- [Insertar, consultar y actualizar registros](#-insertar-consultar-y-actualizar-registros)
- [Modificar la estructura de una tabla](#-modificar-la-estructura-de-una-tabla)
- [Filtros: `BETWEEN`, `LIKE`, `COUNT()`](#-filtros-between-like-count)

---

## 🔹 ¿Qué es XAMPP?

XAMPP es un servidor de aplicaciones que permite desarrollar y probar aplicaciones web de forma local.

| Componente | Función |
|---|---|
| Apache | Servidor web local |
| MySQL (MariaDB) | Sistema gestor de bases de datos |
| PHP | Lenguaje de programación para desarrollo web |
| Tomcat | Servidor para aplicaciones Java |

> ⚠️ Para trabajar con MySQL, el servicio **MySQL** debe estar iniciado (color verde en XAMPP).

## 🔹 Otros servidores de aplicaciones

Alternativas a XAMPP que integran Apache, PHP y MySQL/MariaDB:
- Laragon
- WampServer

## 🔹 ¿Qué es SQL?

**SQL (Structured Query Language)** es el lenguaje utilizado para crear, consultar, modificar y administrar bases de datos relacionales. Permite:

- Crear bases de datos
- Crear tablas
- Insertar información
- Consultar datos
- Actualizar registros
- Eliminar información

## 🔹 Tipos de bases de datos

| Tipo | Descripción | Ejemplos |
|---|---|---|
| **Relacionales** | Organizan la información en tablas relacionadas entre sí | Microsoft Access, MySQL, MariaDB |
| **No relacionales (NoSQL)** | Almacenan información sin utilizar tablas tradicionales | MongoDB, Cassandra, Neo4j |

## 🔹 Conexión a MySQL desde la consola

```bash
cd\
cd xampp
cd mysql
cd bin
mysql -u root -p
```

| Parámetro | Explicación |
|---|---|
| `mysql` | Ejecuta el cliente de MySQL |
| `-u` | Usuario |
| `root` | Usuario administrador |
| `-p` | Solicita contraseña |

> 💡 En XAMPP normalmente `root` no tiene contraseña: basta con presionar Enter.

## 🔹 Comandos SQL (DML)

Los comandos **DML (Data Manipulation Language)** manipulan la información almacenada en las tablas:

- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`

### 📌 Notas importantes
- `LIKE` se utiliza para buscar coincidencias en **cadenas de texto**, no en números.
- Generalmente `WHERE` no se usa junto con `HAVING`: `WHERE` filtra registros **antes** de agrupar y `HAVING` filtra resultados **después** de agrupar (`GROUP BY`).

## 🔹 Registrar todos los comandos con `tee`

```sql
tee C:\Users\Usuario\Downloads\PrimerArchivoMySQL.txt
```

`tee` crea un archivo `.txt` con todo lo escrito y los resultados obtenidos durante la sesión — útil como evidencia del trabajo realizado.

### Comentarios
```sql
## Esto es un comentario
```

## 🔹 Comandos básicos de base de datos

**Mostrar bases de datos:**
```sql
show databases;
```

**Crear una base de datos:**
```sql
create database registro_cliente;
```

**Seleccionar una base de datos:**
```sql
use registro_cliente;
```

**Crear una tabla:**
```sql
create table cliente(
    idCliente INT PRIMARY KEY NOT NULL,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    direccion VARCHAR(60),
    telefono INT,
    correo VARCHAR(50),
    fechaNacimiento DATE
);
```

| Tipo de dato | Función |
|---|---|
| `INT` | Números enteros |
| `VARCHAR(n)` | Texto con longitud variable |
| `DATE` | Fechas |

| Restricción | Función |
|---|---|
| `PRIMARY KEY` | Identifica de forma única cada registro; no se puede repetir |
| `NOT NULL` | Hace obligatorio el campo; no permite valores vacíos |

**Describir una tabla:**
```sql
describe cliente;
-- o su forma abreviada
desc cliente;
```
Muestra campos, tipos de datos, llaves primarias, restricciones y valores por defecto.

## 🔹 Insertar, consultar y actualizar registros

**Insertar:**
```sql
insert into cliente
values(
    1, "Lina", "Lopez", "calle 80",
    12345, "lina@gmail.com", "2000/06/28"
);
```
> Los valores deben respetar el orden de los campos. `VARCHAR` va entre comillas y `DATE` en formato `AAAA/MM/DD` o `AAAA-MM-DD`.

**Consultar:**
```sql
select * from cliente;
```

**Actualizar:**
```sql
update cliente
set apellido = "Trujillo"
where idCliente = 1;
```
> ⚠️ Si se omite `WHERE`, el cambio se aplica a **todos** los registros.

## 🔹 Modificar la estructura de una tabla

```sql
ALTER TABLE nombre_tabla;
```

**Modificar una columna existente:**
```sql
ALTER TABLE cliente MODIFY COLUMN telefono VARCHAR(30);
```

**Agregar una nueva columna:**
```sql
ALTER TABLE cliente
ADD COLUMN edad INT;
```
> Los valores de la nueva columna quedan en `NULL` hasta que se actualicen.

**Actualizar la nueva columna:**
```sql
UPDATE cliente SET edad = 18 WHERE idCliente = 1;
```

## 🔹 Filtros: `BETWEEN`, `LIKE`, `COUNT()`

**Seleccionar columnas específicas + `BETWEEN`:**
```sql
SELECT nombre, apellido, edad
FROM cliente
WHERE edad BETWEEN 17 AND 19;
```

**`LIKE` — búsqueda por patrón de texto:**
```sql
SELECT nombre, apellido FROM cliente WHERE nombre LIKE "L%";
```

| Patrón | Significado | Ejemplo |
|---|---|---|
| `"L%"` | Empieza por L | Lina, Luis |
| `"%L"` | Termina en L | Raúl, Anabel |
| `"%L%"` | Contiene la letra L | Carlos, Milena, Laura |

> `LIKE` solo aplica a texto (`VARCHAR`, `CHAR`, `TEXT`), no a números.

**`COUNT()` — contar registros:**
```sql
SELECT COUNT(*) AS total_clientes
FROM cliente
WHERE apellido = "Perez";
```

---
⬅️ [Volver al índice de SQL](./README.md)
