# Player Inventory List

Lista de inventario de items de un jugador de videojuegos. Permite agregar items con nombre, cantidad y valor, marcarlos como “adquiridos” y eliminarlos. Calcula automáticamente el valor total del inventario y el valor de los items pendientes.

## Tecnologías

- Vue 3 (Composition API, `<script setup>`)
- Vite
- Bootstrap (CSS por CDN)

## Cómo levantar el proyecto

```bash
npm install
npm run dev
```

## Decisiones técnicas

- Cada item es un objeto con `id` único, usado como `:key` en el `v-for` para evitar problemas al eliminar o reordenar.
- Los totales (inventario completo y pendientes) se calculan con `computed`, no se guardan en variables reactivas propias.
- Se valida el formulario antes de agregar un item, mostrando mensajes de error en pantalla en lugar de `alert()`.
- El estado vacío se muestra con un mensaje claro cuando no hay items en el inventario.

## Enlace desplegado

https://zakkdruzer.github.io/player-inventory-list/