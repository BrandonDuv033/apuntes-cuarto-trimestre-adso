---
Fecha: 2026-08-20
Tema: useState y manejo de inputs
---

# useState

`useState` es un Hook de React que permite crear y controlar estados dentro de un componente. Cuando el estado cambia, React vuelve a renderizar el componente.

```jsx
import { useState } from "react";

const [correo, setCorreo] = useState("");
```

- `correo` → valor actual del estado.
- `setCorreo` → función para modificar el estado.
- `""` → valor inicial.

## 📝 Manejo de inputs

Para controlar un `<input>` con React se utilizan principalmente:

- `value` → valor actual del campo.
- `onChange` → detecta cuando el usuario modifica el campo.

```jsx
<input
  name="correo"
  value={correo}
  onChange={manejarCambio}
/>
```

La función que recibe el cambio:

```jsx
function manejarCambio(e) {
  const valor = e.target.value;
  setCorreo(valor);
}
```

### 🔹 `e.target.value`

Obtiene el valor que actualmente tiene el input.

## ✅ Validación de correo

```jsx
if (!valor.includes("@")) {
  setError("Correo inválido");
  return;
}

setError("");
```

### `includes()`

Comprueba si una cadena contiene determinado texto.

```jsx
"correo@gmail.com".includes("@"); // true
```

Si no contiene `@`, se muestra el mensaje de error:

```jsx
{error && <p> {error} </p>}
```

> 💡 **¿Qué significa `error &&`?** Significa: si `error` tiene algún valor, muestra el `<p>`.

## 🔄 Flujo de funcionamiento

```
Usuario escribe
      ↓
onChange
      ↓
manejarCambio()
      ↓
e.target.value
      ↓
setCorreo()
      ↓
React actualiza el estado
      ↓
Se muestra el resultado
```

## 📌 Estados utilizados en la clase

**Estado de correo**

```jsx
const [correo, setCorreo] = useState("");
```

Guarda lo que escribe el usuario.

**Estado de error**

```jsx
const [error, setError] = useState("");
```

Guarda el mensaje de error. Por ejemplo:

```jsx
setError("Correo inválido");
```

Para eliminar el error:

```jsx
setError("");
```

## 🎨 Estilos directamente en JSX

En React los estilos en línea se escriben como un objeto JavaScript:

```jsx
<div style={{ padding: "20px", fontFamily: "sans-serif" }}>
```

```jsx
<p style={{ color: "red" }}>Correo inválido</p>
```

> ⚠️ A diferencia del CSS normal, algunas propiedades utilizan camelCase: `font-family` en CSS se escribe `fontFamily` en JSX.

## 🧠 Otros conceptos vistos en el código

### `onClick`

Ejecuta una función al hacer clic:

```jsx
<button onClick={actualizar}>Enviar</button>
```

### `onSubmit`

Se ejecuta cuando se envía un formulario:

```jsx
<form onSubmit={manejarEnvio}>
```

### `e.preventDefault()`

Evita el comportamiento predeterminado del formulario, como recargar la página:

```jsx
function manejarEnvio(e) {
  e.preventDefault();
}
```

### Checkbox controlado

```jsx
<input
  type="checkbox"
  checked={acepta}
  onChange={(e) => setAcepta(e.target.checked)}
/>
```

- `checked` → indica si está marcado.
- `e.target.checked` → obtiene `true` o `false`.

## ⭐ Ejemplo completo de la clase

```jsx
import { useState } from "react";

function App() {
  const [correo, setCorreo] = useState("");
  const [error, setError] = useState("");

  function manejarCambio(e) {
    const valor = e.target.value;

    setCorreo(valor);

    if (!valor.includes("@")) {
      setError("Correo inválido");
      return;
    }

    setError("");
  }

  return (
    <div>
      <label>Correo electrónico:</label>

      <input
        name="correo"
        value={correo}
        onChange={manejarCambio}
      />

      {error && <p> {error} </p>}
    </div>
  );
}

export default App;
```

## 📝 Idea clave

`useState` guarda información que puede cambiar. `onChange` detecta los cambios del usuario y `setEstado()` actualiza el estado para que React vuelva a mostrar la información actualizada.
