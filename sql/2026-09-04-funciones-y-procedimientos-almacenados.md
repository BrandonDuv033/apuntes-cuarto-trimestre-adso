---
Fecha: 2026-09-04
Tema: Funciones y Procedimientos Almacenados
---

# 🧮 Funciones y Procedimientos Almacenados en MySQL

> Sesión enfocada en la creación de **funciones deterministas** en MySQL y su combinación con **procedimientos almacenados** para automatizar el registro de una tabla `empleados`.

## 📑 Tabla de Contenido

- [Conceptos Clave](#-conceptos-clave)
- [Diferencia entre Parámetro y Función](#-diferencia-entre-parámetro-y-función)
- [Ejercicios Básicos](#-ejercicios-básicos)
  - [Ejercicio 1: Precio Base de un Producto](#ejercicio-1-precio-base-de-un-producto)
  - [Ejercicio 2: Total según Cantidad](#ejercicio-2-total-según-cantidad)
- [Caso de Estudio: Nómina de Empleados](#-caso-de-estudio-nómina-de-empleados)
  - [Salario Mensual](#salario-mensual)
  - [Auxilio de Transporte](#auxilio-de-transporte)
  - [Comisión](#comisión)
  - [Salud y Pensión](#salud-y-pensión)
  - [Total Devengado](#total-devengado)
- [Procedimiento: Registrar Empleado](#-procedimiento-registrar-empleado)

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

## ⚙️ Procedimiento: Registrar Empleado

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
