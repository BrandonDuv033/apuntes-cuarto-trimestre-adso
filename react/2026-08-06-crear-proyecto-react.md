---
Fecha: 2026-08-06
Tema: Creación de un proyecto React (Create React App y Vite)
---

# Crear un proyecto React

React es una biblioteca de JavaScript utilizada principalmente para crear interfaces de usuario.

## 🔹 Crear React con Create React App

1. Crear una carpeta y abrir la terminal en Visual Studio Code.
2. Ejecutar el comando de creación del proyecto:

```bash
npx create-react-app "Carpeta de trabajo"
```

3. Ingresar a la carpeta del proyecto:

```bash
cd ejercicio0
```

4. Ejecutar el servidor de desarrollo:

```bash
npm start
```

**Ejemplo en terminal:**

```
PS C:\Users\Aprendiz\Desktop\Taller01> cd ejercicio0
PS C:\Users\Aprendiz\Desktop\Taller01\ejercicio0> npm start
```

Esto inicia el servidor de desarrollo de React.

### Carpeta `node_modules`

Contiene las dependencias instaladas que necesita el proyecto. Si se elimina, puede volver a generarse instalando nuevamente las dependencias:

```bash
npm install
```

### Salir de una carpeta

```bash
cd ..
```

> `..` significa subir un nivel en la estructura de carpetas.

## ⚡ Crear un proyecto React con Vite

Vite es una herramienta de desarrollo frontend que permite crear proyectos de manera rápida.

| Paso | Acción |
|---|---|
| 1 | Buscar "Vite JS" |
| 2 | En la terminal: `npm create vite@latest` |
| 3 | Escribir el nombre del proyecto |
| 4 | Seleccionar framework: **React** |
| 5 | Seleccionar variante: **JavaScript** |
| 6 | Seleccionar linter: **Oxlint** y confirmar |

```bash
npm create vite@latest
```

## 📁 Carpeta `src`

> ⭐ **Importante:** la carpeta `src` será donde se realiza gran parte del trabajo de desarrollo. Aquí se encuentran principalmente los archivos donde se construye la aplicación.

Los componentes de React pueden escribirse en archivos `.jsx`.

**JSX** permite escribir una sintaxis parecida a HTML dentro de JavaScript para construir componentes de React.

## 📌 Nota importante

En la clase se trabajaron dos formas de crear proyectos React: **Create React App** y **Vite**. Para proyectos nuevos, Vite es actualmente una opción muy utilizada; los comandos y la estructura pueden variar dependiendo de cuál de las dos herramientas se utilice.
