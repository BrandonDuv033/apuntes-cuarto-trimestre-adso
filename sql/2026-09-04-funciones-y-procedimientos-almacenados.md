---
Fecha: 2026-09-04
Tema: Funciones Almacenadas
---

# 🧮 Funciones Almacenadas en MySQL

> Sesión enfocada en la creación de **funciones deterministas** en MySQL: su sintaxis, cuándo usarlas y cómo se encadenan unas con otras. Al final se muestra un procedimiento almacenado solo como una **forma de aplicar** las funciones creadas (no es el tema central de la clase).

## 📑 Tabla de Contenido

- [¿Qué es una Función Almacenada?](#-qué-es-una-función-almacenada)
- [Sintaxis General](#-sintaxis-general)
- [Conceptos Clave](#-conceptos-clave)
- [¿Por qué DETERMINISTIC?](#-por-qué-deterministic)
- [Diferencia entre Parámetro y Función](#-diferencia-entre-parámetro-y-función)
- [Función vs. Procedimiento Almacenado](#-función-vs-procedimiento-almacenado)
- [Buenas Prácticas y Errores Comunes](#-buenas-prácticas-y-errores-comunes)
- [Ejercicios Básicos](#-ejercicios-básicos)
  - [Ejercicio 1: Precio Base de un Producto](#ejercicio-1-precio-base-de-un-producto)
  - [Ejercicio 2: Total según Cantidad](#ejercicio-2-total-según-cantidad)
- [Caso de Estudio: Nómina de Empleados](#-caso-de-estudio-nómina-de-empleados)
  - [Salario Mensual](#salario-mensual)
  - [Auxilio de Transporte](#auxilio-de-transporte)
  - [Comisión](#comisión)
  - [Salud y Pensión](#salud-y-pensión)
  - [Total Devengado](#total-devengado)
- [Aplicación Práctica: Procedimiento Registrar Empleado](#-aplicación-práctica-procedimiento-registrar-empleado)

---

## 🧠 ¿Qué es una Función Almacenada?

Una **función almacenada** (*stored function*) es un bloque de código SQL guardado en la base de datos que:

- Recibe cero o más parámetros de **entrada** (nunca de salida, a diferencia de un procedimiento).
- **Siempre** devuelve un único valor con `RETURN`, del tipo declarado en `RETURNS`.
- Se puede usar **dentro de una consulta**, como si fuera una columna calculada: en un `SELECT`, un `WHERE`, un `SET`, etc.
- Sirve para **encapsular una fórmula o cálculo repetitivo** (ej. liquidar un salario, calcular un impuesto, formatear un dato) para no reescribirlo cada vez.

## 🧾 Sintaxis General

```sql
CREATE FUNCTION nombreFuncion(parametro1 TIPO, parametro2 TIPO, ...)
RETURNS tipoDeDato
DETERMINISTIC
BEGIN
    DECLARE variable TIPO;
    -- lógica / cálculos
    RETURN variable;
END
```

| Parte | Función dentro de la sintaxis |
|---|---|
| `CREATE FUNCTION nombre(...)` | Define el nombre y los parámetros de entrada (sin `IN`/`OUT`, se asume que todos son de entrada) |
| `RETURNS tipoDeDato` | Obligatorio: indica qué tipo de dato va a devolver la función (`INT`, `DOUBLE`, `VARCHAR`, etc.) |
| `DETERMINISTIC` | Obligatorio para que MySQL permita crear/ejecutar la función (ver sección siguiente) |
| `DECLARE` | Crea variables locales que solo existen dentro del cuerpo de la función |
| `RETURN` | Corta la ejecución y devuelve el valor final; toda función debe terminar en un `RETURN` |

---

## 🔑 Conceptos Clave

| Elemento | Descripción |
|---|---|
| `RETURNS` | Declara el tipo de dato que la función va a devolver |
| `DETERMINISTIC` | **Obligatorio** en una función para que MySQL permita su ejecución |
| Parámetros `IN` | En funciones no se usan `IN`, `OUT` ni `INOUT`; solo se reciben parámetros y se retorna un valor con `RETURN` |
| Invocación | Se llama con `SELECT`, ejemplo: `nombreBaseDatos.nombreFuncion(parametros)` |
| `SELECT ... INTO` | Forma de guardar el resultado de una consulta directamente en una variable declarada |

## ⚙️ ¿Por qué DETERMINISTIC?

- MySQL exige clasificar una función como **`DETERMINISTIC`** o **`NOT DETERMINISTIC`**.
- Una función es *determinista* cuando, dados los **mismos parámetros de entrada**, siempre devuelve el **mismo resultado** (ej. `salarioMensual(2000000, 30)` siempre da lo mismo).
- Si el binary logging está activado (`bin log`) y no se declara ninguna de las dos opciones, MySQL puede **bloquear la creación de la función** por razones de replicación/seguridad — por eso en clase se usa `DETERMINISTIC` siempre, aunque técnicamente no todas las funciones lo sean al 100%.

## ⚖️ Diferencia entre Parámetro y Función

- **Parámetro:** se usa varias veces dentro de distintos contextos o cálculos.
- **Función:** se crea para automatizar algo que se repite de forma constante (ej. un cálculo diario).

## 🆚 Función vs. Procedimiento Almacenado

| | Función | Procedimiento |
|---|---|---|
| Retorno | Obligatorio, un solo valor (`RETURN`) | Opcional, puede no devolver nada |
| Parámetros | Solo de entrada | `IN`, `OUT`, `INOUT` |
| Se puede usar en un `SELECT` | ✅ Sí | ❌ No |
| Se ejecuta con | `SELECT funcion(...)` | `CALL procedimiento(...)` |
| Uso típico | Cálculos y fórmulas reutilizables | Acciones (insertar, actualizar, procesos completos) |

> Regla rápida: si necesitas **un valor** para usarlo en otra parte → función. Si necesitas **hacer algo** (insertar, actualizar varias tablas, ejecutar lógica compleja) → procedimiento.

## 🧯 Buenas Prácticas y Errores Comunes

- No nombrar la variable local **igual** que la función (ej. `DECLARE salarioMensual DOUBLE;` dentro de `salarioMensual(...)`) puede generar confusión o conflicto; es más seguro usar un nombre distinto (ej. `v_salarioMensual`).
- Toda función debe garantizar que llegue a un `RETURN`; si hay condicionales (`IF`), asegurarse de cubrir todos los casos.
- Al encadenar funciones dentro de otras (como `salarioMensual()` dentro de `comision()`), verificar que los **tipos de parámetros coincidan exactamente** en orden y tipo de dato.
- Usar `ROUND()` cuando se trabaja con dinero, para evitar decimales largos poco realistas.

---

## 🔑 Conceptos Clave

| Elemento | Descripción |
|---|---|
| `RETURNS` | Declara el tipo de dato que la función va a devolver |
| `DETERMINISTIC` | **Obligatorio** en una función para que MySQL permita su ejecución |
| Parámetros `IN` | En funciones no se usan `IN`, `OUT` ni `INOUT`; solo se reciben parámetros y se retorna un valor con `RETURN` |
| Invocación | Se llama con `SELECT`, ejemplo: `nombreBaseDatos.nombreFuncion(parametros)` |

## ⚖️ Diferencia entre Parámetro y Función

- **Parámetro:** se usa varias veces dentro de distintos contextos o cálculos.
- **Función:** se crea para automatizar algo que se repite de forma constante (ej. un cálculo diario).

---

## 🧩 Ejercicios Básicos

### Ejercicio 1: Precio Base de un Producto

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `funcionUno`(idP INT) RETURNS double
    DETERMINISTIC
BEGIN

DECLARE pre DECIMAL(10,2);
SELECT precioBase INTO pre
FROM productos
WHERE idProducto = idP;

RETURN pre;
END
```

### Ejercicio 2: Total según Cantidad

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `funcionDos`(codProducto INT, cantidadProducto INT) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE precio DOUBLE;
DECLARE total DOUBLE;
SELECT precioBase INTO precio
FROM productos
WHERE idProducto = codProducto;

SET total = precio * cantidadProducto;
RETURN total;
END
```

---

## 👔 Caso de Estudio: Nómina de Empleados

Conjunto de funciones encadenadas para calcular la nómina de un empleado a partir de su `salario` y sus `diasTrabajados`.

### Salario Mensual

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `salarioMensual`(salario DOUBLE, diasTrabajados INT) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE salarioMensual DOUBLE;
SET salarioMensual = (salario / 30) * diasTrabajados;
RETURN salarioMensual;
END
```

### Auxilio de Transporte

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `auxilioTransporte`(salario DOUBLE) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE salarioMinimo DOUBLE;
DECLARE auxilioTransporte DOUBLE;
SET salarioMinimo = 1750905 * 2;
IF salario <= salarioMinimo THEN
SET auxilioTransporte = 249095;
ELSE
SET auxilioTransporte = 0;
END IF;
RETURN auxilioTransporte;
END
```

> El auxilio de transporte solo aplica si el salario es menor o igual a **2 salarios mínimos**.

### Comisión

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `comision`(salario DOUBLE, 
diasTrabajados INT) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE comision DOUBLE;
SET comision = ROUND((salarioMensual(salario , diasTrabajados) * 0.02), 0);
RETURN comision;
END
```

### Salud y Pensión

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `saludYPension`(salario DOUBLE, diasTrabajados INT) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE saludYPension DOUBLE;
SET saludYPension = ROUND(salarioMensual(salario , diasTrabajados) * 0.04, 0);
RETURN saludYPension;
END
```

### Total Devengado

```sql
CREATE DEFINER=`root`@`localhost` FUNCTION `totalDevengado`(salario DOUBLE, diasTrabajados INT) RETURNS double
    DETERMINISTIC
BEGIN
DECLARE salarioTotal DOUBLE;
SET salarioTotal = salarioMensual(salario, diasTrabajados) + 
auxilioTransporte(salario) +
comision(salario, diasTrabajados) -
(saludYPension (salario, diasTrabajados) * 2);

RETURN salarioTotal;
END
```

> La salud y pensión se descuenta **dos veces** (`* 2`) porque representa tanto el aporte a salud como el aporte a pensión.

---

## 🛠️ Aplicación Práctica: Procedimiento Registrar Empleado

> [!NOTE]
> Este procedimiento **no es el tema de la clase**, se muestra únicamente como ejemplo de una forma de **usar en conjunto** todas las funciones creadas arriba, insertando un registro completo en una sola llamada.

Procedimiento que inserta un nuevo registro en la tabla `empleados`, reutilizando todas las funciones anteriores para calcular automáticamente cada columna derivada.

```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `registrarEmpleado`(
IN id INT,
IN nombre VARCHAR(50),
IN salarioEmpleado DOUBLE,
IN diasT INT
)
BEGIN
INSERT INTO empleados VALUES(
id, nombre, salarioEmpleado, diasT,
auxilioTransporte(salarioEmpleado),
comision (salarioEmpleado, diasT),
saludYPension (salarioEmpleado, diasT),
saludYPension (salarioEmpleado, diasT),
totalDevengado(salarioEmpleado, diasT));
END
```

---

⬅️ [Volver al índice de SQL](../README.md)