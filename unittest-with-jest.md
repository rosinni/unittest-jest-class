# Pruebas unitarias con jest


> Así como los músicos afinan sus instrumentos antes de tocar una canción, los desarrolladores afinan su código con pruebas unitarias.

Imagina que quieres armar un robot pero tienes que probar todas sus piezas por separado: los brazos, las piernas, la cabeza, etc. ¿Por qué? porque si el brazo derecho del robot no suma bien, es mejor saberlo antes de que todo el robot esté construido.

![test](./imagen2.png)


## ¿Para quién son los tests unitarios?

| Perspectiva              | Explicación clara                                                                 |
|--------------------------|-----------------------------------------------------------------------------------|
| 👨‍💻 Para el programador     | Le permiten detectar errores antes de que el software llegue al usuario. Son como una red de seguridad. |
| 🎯 Para el código          | Aseguran que cada parte del sistema funciona correctamente, de forma aislada.     |
| 😊 Para el usuario         | Aunque no ve los tests, sí disfruta un producto más estable, sin errores visibles. |


# ¿Qué es Jest?

Jest es un framework de pruebas hecho por Facebook que funciona de maravilla con JavaScript. Nos ayuda a:

- Ejecutar pruebas automáticamente.
- Ver rápidamente qué pasa si algo falla.
- Aprender de nuestros errores en tiempo real.


## Enviar una notificación a un usuario

Imagina que trabajas en una app de mensajería tipo WhatsApp. Tienes una función que envía notificaciones a los usuarios.

```js
function enviarNotificacion(usuario) {
  return `📩 Notificación enviada a ${usuario.nombre}`;
}
```

## ¿Qué puede salir mal?
Supón que se llama así:

```js
const usuario = null;
enviarNotificacion(usuario);
```

Esto generará un error como:

```js
TypeError: Cannot read property 'nombre' of null
```

> ⚠️ Esto rompe la app en producción. Y lo peor es que el usuario no sabrá por qué no recibió su notificación. Tú tampoco... a menos que tengas pruebas.

## ✅ Pruebas unitarias con Jest
```js
const { enviarNotificacion } = require('./notificaciones');

test('envía notificación correctamente a un usuario con nombre', () => {
  const usuario = { nombre: 'María' };
  expect(enviarNotificacion(usuario)).toBe('📩 Notificación enviada a María');
});

test('lanza error si el usuario es null', () => {
  expect(() => enviarNotificacion(null)).toThrow('Usuario no válido');
});
```

## Mejora con control de errores

Para pasar la prueba anterior, corregimos la función:

```js
function enviarNotificacion(usuario) {
  if (!usuario || !usuario.nombre) {
    throw new Error('Usuario no válido');
  }
  return `📩 Notificación enviada a ${usuario.nombre}`;
}

module.exports = { enviarNotificacion };
```

## Testing componentes React

¿Qué necesitas instalar para testear React? En un proyecto de React hecho con Vite o Create React App, normalmente ya viene casi todo. Si no, puedes instalarlo así:

```bash
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

Y agregar en setupTests.js:

```js
// setupTests.js
import '@testing-library/jest-dom';
````

Y en package.json (si usas Vite):

```json
"jest": {
  "setupFilesAfterEnv": ["<rootDir>/setupTests.js"],
  "testEnvironment": "jsdom"
}
```

### Ejemplo pedagógico: MiComponente.jsx


```jsx
// MiComponente.jsx
import React from 'react';

function MiComponente() {
  return <h1>Hola, soy un componente!</h1>;
}

export default MiComponente;
```


### Testear que el archivo exporta una función válida
Esto lo puedes usar para reforzar la idea de “un componente es una función que retorna JSX”:

```jsx
// ComponenteExportado.test.js
import MiComponente from './MiComponente';

test('el componente exportado es una función', () => {
  expect(typeof MiComponente).toBe('function');
});
```

### Verificar que renderiza contenido HTML, sin importar el texto

```jsx
import { render, screen } from '@testing-library/react';
import MiComponente from './MiComponente';

test('el componente renderiza algo de HTML', () => {
  const { container } = render(<MiComponente />);
  expect(container.firstChild).not.toBeNull();
});
```