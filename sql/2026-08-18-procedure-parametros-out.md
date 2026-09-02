---
fecha: 2026-08-18
materia: SQL
tema: Procedimiento almacenado con parámetros IN y OUT
---

# 🗓️ 18 de agosto de 2026

## 📑 Contenido
- [Procedure `totalVendido`](#-procedure-totalvendido)

---

## 🔹 Procedure `totalVendido`

Procedimiento que recibe el código de un vendedor (`IN`) y calcula, mediante parámetros de salida (`OUT`), la cantidad de comprobantes y el valor total vendido.

```sql
CREATE DEFINER=`root`@`localhost` PROCEDURE `totalVendido`
(IN codigo VARCHAR(20), OUT cantComprobantes INT, OUT valorTotal DOUBLE)
BEGIN

SELECT COUNT(co.cod_comprobante) AS cantComprobantes,
       v.id_vendedor,
       SUM(dc.precio_total) AS valorTotalVendido
FROM vendedor v
INNER JOIN comprobante co
    ON v.id_vendedor = co.cod_vendedor
INNER JOIN detalle_comprobante dc
    ON dc.cod_comprobante = co.cod_comprobante
WHERE v.id_vendedor = codigo;

END
```

| Parámetro | Tipo | Función |
|---|---|---|
| `codigo` | `IN VARCHAR(20)` | Código del vendedor a consultar |
| `cantComprobantes` | `OUT INT` | Cantidad de comprobantes emitidos por el vendedor |
| `valorTotal` | `OUT DOUBLE` | Valor total vendido por ese vendedor |

> 💡 A diferencia de `IN` (dato de entrada), `OUT` se usa para que el procedimiento **devuelva** un valor calculado que puede reutilizarse fuera del procedimiento.

---
⬅️ [Volver al índice de SQL](./README.md)
