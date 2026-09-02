---
fecha: 2026-08-25
materia: SQL
tema: Consultas de práctica (sistema hotelero) y procedimientos con parámetros IN/OUT
---

# 🗓️ 25 de agosto de 2026

> Ejercicios de práctica aplicados a un sistema de reservas de hotel: subconsultas, `INNER JOIN` y procedimientos almacenados con parámetros `IN`/`OUT`.

## 📑 Contenido
1. [Habitaciones con precio superior al promedio](#1-habitaciones-con-precio-superior-al-promedio)
2. [Servicios con precio superior al promedio](#2-servicios-con-precio-superior-al-promedio)
3. [Huéspedes con mayor número de reservas](#3-huéspedes-con-mayor-número-de-reservas)
4. [Procedure `historialReservas`](#4-procedure-historialreservas)
5. [Procedure `resumenMensualReservas12`](#5-procedure-resumenmensualreservas12)

---

## 1. Habitaciones con precio superior al promedio

```sql
SELECT h.numero, th.nombreTipo, th.precioNoche
FROM habitaciones h
INNER JOIN tipos_habitacion th
    ON th.idTipo = h.idTipo
WHERE th.precioNoche >
(
    SELECT AVG(th.precioNoche) FROM tipos_habitacion th
);
```

## 2. Servicios con precio superior al promedio

```sql
SELECT nombreServicio, precio
FROM servicios
WHERE precio >
(
    SELECT AVG(precio) FROM servicios
);
```

## 3. Huéspedes con mayor número de reservas

```sql
SELECT CONCAT(h.nombre, " ", h.apellido) AS "Huesped",
       h.fechaRegistro
FROM huespedes h
INNER JOIN reservas r
    ON h.idHuesped = r.idHuesped
WHERE h.fechaRegistro <
(
    SELECT MIN(r.fechaReserva) FROM reservas r
);
```

## 4. Procedure `historialReservas`

Recibe un año (`IN`) y devuelve el historial de reservas de ese año.

```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `historialReservas`(
    IN año INT)
BEGIN
    SELECT CONCAT(h.nombre, " ", h.apellido) AS "Huesped",
           ha.numero, r.fechaEntrada, r.fechaSalida
    FROM huespedes h
    INNER JOIN reservas r
        ON h.idHuesped = r.idHuesped
    INNER JOIN detalle_reserva dr
        ON dr.idReserva = r.idReserva
    INNER JOIN habitaciones ha
        ON dr.idHabitacion = ha.idHabitacion
    WHERE YEAR(r.fechaReserva) = año;
END
```

## 5. Procedure `resumenMensualReservas12`

Recibe un mes (`IN`) y devuelve, mediante parámetros de salida (`OUT`), la cantidad de reservas y el total de noches reservadas en ese mes.

```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `resumenMensualReservas12`(
    IN mes INT,
    OUT cantidadReservas INT,
    OUT nochesReservadas INT)
BEGIN
    SELECT COUNT(idReserva) AS "Cantidad de Reservas",
           SUM(DATEDIFF(fechaSalida, fechaEntrada)) AS "Días de Diferencia"
    INTO
        cantidadReservas,
        nochesReservadas
    FROM
        reservas
    WHERE MONTH(fechaReserva) = mes;
END
```

| Parámetro | Tipo | Función |
|---|---|---|
| `mes` | `IN INT` | Mes a consultar (1–12) |
| `cantidadReservas` | `OUT INT` | Total de reservas hechas en ese mes |
| `nochesReservadas` | `OUT INT` | Suma de noches (`DATEDIFF`) de todas esas reservas |

---
⬅️ [Volver al índice de SQL](./README.md)
