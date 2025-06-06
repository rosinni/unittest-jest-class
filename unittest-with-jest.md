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