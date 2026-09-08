---
Fecha: 2026-09-08
Tema: Funciones de Encriptación y Compresión
---

# 🔐 Funciones de Encriptación y Compresión

> [!IMPORTANT]
> **Pre Sustentación SQL:** 18 de septiembre. Revisar todo lo visto hasta la fecha (procedimientos, funciones almacenadas, triggers y encriptación).

## 📑 Tabla de Contenido

- [🧠 Conceptos Generales](#-conceptos-generales)
- [🛡️ Protección de la Información](#️-protección-de-la-información)
  - [Hash (No reversible)](#hash-no-reversible)
  - [Cifrado (Reversible)](#cifrado-reversible)
- [🔑 AES_ENCRYPT()](#-aes_encrypt)
- [🔓 AES_DECRYPT()](#-aes_decrypt)
- [💻 Ejemplo Práctico en Consola](#-ejemplo-práctico-en-consola)
- [✅ Buenas Prácticas](#-buenas-prácticas)

---

## 🧠 Conceptos Generales

> Varias funciones de encriptación y compresión devuelven cadenas cuyo resultado puede contener **valores de bytes arbitrarios**. Si se desea almacenar esos resultados en una tabla, lo recomendable es usar una columna con un **tipo de dato de cadena binaria**: `VARBINARY` o `BLOB`.

---

## 🛡️ Protección de la Información

Existen dos grandes enfoques para proteger información en una base de datos:

| Enfoque | Reversible | Objetivo principal |
|---|---|---|
| **Hash** | ❌ No | Verificar integridad / almacenar contraseñas |
| **Cifrado** | ✅ Sí | Ocultar información que luego debe recuperarse |

### Hash (No reversible)

Una vez aplicado, **no existe** una función inversa que devuelva el texto original.

| Algoritmo | Estado | Observación |
|---|---|---|
| `MD5` | ⚠️ Obsoleto | Vulnerable a colisiones |
| `SHA-1` | ⚠️ Obsoleto | Vulnerable a colisiones |
| `SHA-256` / `SHA-512` | ✅ Recomendado | Usado comúnmente vía `SHA2()` |

### Cifrado (Reversible)

| Tipo | Algoritmos | N.º de claves | Detalle |
|---|---|---|---|
| **Simétrico** | `AES`, `DES` | 1 (misma clave) | Se usa la **misma clave** para cifrar y descifrar |
| **Asimétrico** | `RSA` | 2 (pública/privada) | Una clave **pública** y otra **privada**; distintas entre sí |

> `DES` se considera legado por su longitud de clave corta; en la práctica se prefiere `AES`.

---

## 🔑 AES_ENCRYPT()

- Utiliza el algoritmo oficial **AES** (Advanced Encryption Standard).
- Permite varias longitudes de clave; **por defecto implementa AES con una longitud de clave de 128 bits**.
- Si cualquiera de los argumentos es `NULL`, la función devuelve `NULL`.

**Sintaxis:**

```sql
SELECT AES_ENCRYPT(cadena, llave_cadena);
```

| Parámetro | Descripción |
|---|---|
| `cadena` | Texto a cifrar |
| `llave_cadena` | Clave utilizada para el cifrado |
| **Retorno** | Cadena **binaria** con la salida cifrada |

## 🔓 AES_DECRYPT()

Función inversa a `AES_ENCRYPT()`. Recibe la cadena cifrada y la **misma clave** usada para cifrar, y devuelve el texto original en claro.

```sql
SELECT AES_DECRYPT(cadena_cifrada, llave_cadena);
```

---

## 💻 Ejemplo Práctico en Consola

Ejemplo trabajado en clase: generar una clave derivada con `SHA2()` y usarla para cifrar/descifrar con AES.

```sql
-- Generar una clave a partir de un texto usando SHA2 (512 bits)
SET @clave = SHA2('Zaida Patricia', 512);

-- Cifrar el texto usando la clave generada
SET @textoC = AES_ENCRYPT('ya casi salimos', @clave);

-- Ver el resultado cifrado (bytes arbitrarios)
SELECT @textoC;
-- +------------------+
-- | @textoC          |
-- +------------------+
-- | .ýdDÇQ]HáÎ╬K²=Õ |
-- +------------------+

-- Descifrar usando la misma clave
SELECT AES_DECRYPT(@textoC, @clave);
-- +------------------------------+
-- | AES_DECRYPT(@textoC, @clave) |
-- +------------------------------+
-- | ya casi salimos              |
-- +------------------------------+
```

> [!NOTE]
> `SHA2()` aquí no cifra el texto: se usa como **derivador de clave** (hash de un texto legible para convertirlo en una llave de longitud fija) que luego alimenta a `AES_ENCRYPT()`.

---

## ✅ Buenas Prácticas

- Usar `VARBINARY` o `BLOB` para almacenar resultados de `AES_ENCRYPT()` (contienen bytes arbitrarios, no texto plano).
- Preferir `SHA-256`/`SHA-512` sobre `MD5`/`SHA-1` para hashing.
- Nunca guardar la clave de cifrado en la misma tabla que el dato cifrado.
- Verificar siempre que ni la cadena ni la clave sean `NULL` antes de cifrar (la función devolvería `NULL` silenciosamente).

### ❌ Errores comunes

- Confundir **hash** (irreversible) con **cifrado** (reversible): un hash nunca debe usarse si se necesita recuperar el dato original.
- Usar `AES_DECRYPT()` con una clave distinta a la usada en `AES_ENCRYPT()` → devuelve `NULL`, no un error explícito.

---

[⬅ Volver al índice de SQL](./README.md)