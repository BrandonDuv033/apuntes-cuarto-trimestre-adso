---
Fecha: 2026-08-13
Tema: Funciones en JSX, arrays, formularios/eventos, JSX vs HTML y Bootstrap
---

# Funciones, arrays, formularios y estilos en React

## 🎲 Número aleatorio

```jsx
function obtenerNumeroSuerte() {
  return Math.trunc(Math.random() * 5) + 1;
}
```

**¿Qué hace?** Genera un número entero entre 1 y 5.

- `Math.random()` → número aleatorio entre 0 y menor que 1.
- `* 5` → amplía el rango.
- `Math.trunc()` → elimina la parte decimal.
- `+ 1` → desplaza el resultado para comenzar en 1.

## 🔹 Funciones dentro de React

Una función puede devolver JSX:

```jsx
function mostrarTitulo(tit) {
  return <h1>{tit}</h1>;
}
```

Y utilizarse dentro del componente:

```jsx
{mostrarTitulo("Hola Mundo")}
```

### Función para sumar

```jsx
function sumar(x, y) {
  return x + y;
}
```

Uso:

```jsx
<p>5 + 6 = {sumar(5, 6)}</p>
```

### Función que devuelve varios elementos

```jsx
function informacion(nombreCompleto, ciudadNacimiento, edad) {
  return (
    <>
      <p>Nombre: {nombreCompleto}</p>
      <p>Ciudad de Nacimiento: {ciudadNacimiento}</p>
      <p>Edad: {edad} años</p>
    </>
  );
}
```

> 📌 `<>...</>` → Fragment, permite agrupar varios elementos sin crear un `<div>` adicional.

## 🔹 Arrays en React

```jsx
const buscadores = [
  "https://www.google.com/",
  "http://www.bing.com",
  "http://www.yahoo.com",
];
```

Para acceder a cada elemento:

```jsx
<a href={buscadores[0]}>Google</a>
<a href={buscadores[1]}>Bing</a>
<a href={buscadores[2]}>Yahoo</a>
```

> 📌 Recuerda: la primera posición es 0.

## 🔹 Formularios y eventos en React

```jsx
function presion(e) {
  e.preventDefault();

  const v1 = parseInt(e.target.valor1.value);
  const v2 = parseInt(e.target.valor2.value);

  const suma = v1 + v2;

  alert("La suma es: " + suma);
}
```

```jsx
<form onSubmit={presion}>
  <label>Numero #1</label>
  <input type="number" name="valor1" />

  <label>Numero #2</label>
  <input type="number" name="valor2" />

  <input type="submit" value="Sumar" />
</form>
```

### Conceptos importantes

| Concepto | Descripción |
|---|---|
| `onSubmit` | Se ejecuta cuando se envía el formulario |
| `e` | Objeto del evento |
| `e.preventDefault()` | Evita el comportamiento predeterminado del formulario |
| `e.target` | Elemento que originó el evento |
| `parseInt()` | Convierte un valor a número entero |

## ⚠️ HTML vs JSX

| HTML | JSX |
|---|---|
| `<div class="botones">` | `<div className="botones">` |
| `<label for="correo">` | `<label htmlFor="correo">` |

En JSX se utiliza `className` en lugar de `class`.

## 🎨 Bootstrap en React

```jsx
<button className="btn btn-primary">
  Primary
</button>
```

| Clase | Estilo |
|---|---|
| `btn` | Establece el estilo base del botón |
| `btn-primary` | Primario |
| `btn-secondary` | Secundario |
| `btn-success` | Éxito |
| `btn-danger` | Peligro |
| `btn-warning` | Advertencia |
| `btn-info` | Información |
| `btn-light` | Claro |
