
---
layout: custom-cover
background: vue-sticker.jpg
---

# <logos-nuxt-icon /> Nuxt - I

## 🌈 Vuenas tardes!!! 🌈


---
layout: statement
title: Nuxt
---

# <logos-nuxt-icon /> Nuxt

---
layout: two-cols
title: Nuxt - Características principales
---

<style>
ul li {
  font-size: .9rem;
}
</style>

<v-clicks depth="2">

  - **Zero-config**: Configuración mínima para empezar
  - **File-based routes**: Rutas dinámicas y anidadas
  - **Data fetching** (_opinionando_)
  - **Backend** (APIs) integrado (con Nitro)
  - **Middleware** (https://nuxt.com/docs/4.x/directory-structure/app/middleware)
  - Transiciones: animaciones entre páginas
  - **SEO** (Search Engine Optimization)
  - **Optimización de _assets_** integrada
  - **Error-handling**
  - **Layouts**
  - **Diferentes modos de renderizado**:
    - Server Side Rendering (SSR)
    - Generación de sitios estáticos (SSG, JAMStack)
    - Single Page Application (SPA)
    - Modos Híbridos (SSR + SPA)
    - Islas (Componentes que se renderizan en el servidor y no son interactivos)

</v-clicks>

::right::

<v-clicks depth="3">

  - **Vite** (servidor de desarrollo y bundler)
    - Code-splitting
    - Hot Module Reloading (HMR)
    - OXC-compatible (https://es.vite.dev/guide/rolldown)
  - **Type-safe**: TypeScript
    - Typechecking/Build
    - Auto generated types
      - [Auto Imports](https://nuxt.com/docs/4.x/guide/concepts/auto-imports)
      - [API route types](https://nuxt.com/docs/4.x/guide/concepts/server-engine#typed-api-routes)
  - **Ecosistema de módulos** (https://nuxt.com/modules)
  - Y otras características de Nuxt:
    - **Hooks**
    - **Components/Composables**
    - **Plugins**
    - **Runtime config**
    - **Layers**
    - **DevTools**

</v-clicks>


---
layout: quote
---

# Progressive Enhacement

No necesitamos todas las funcionalidades de Nuxt. Podemos usar solo lo que necesitamos y el bundle final será más pequeño.

---
layout: two-cols
title: Progressive Enhacement
---

<style>
img.max-h-xs {
  max-height: none;
}
</style>


## Estructura de carpetas inicial

<br />

<img src="/nuxt-inicio.png" class="max-h-xs" />


::right::

<v-click>

## Ejemplo de app compleja

<br />

<img src="/nuxt-completo.png" class="max-h-xs" />
</v-click>

---
layout: quote
---

# Universal rendering (SSR)


---
layout: full
title: Universal rendering
---

# Universal rendering (SSR)
<div class="flex justify-center relative">
  <img src="/ssr.svg" />
  <!-- Server -->
  <logos-nodejs-icon class="absolute top-1/2 left-0 text-6xl" v-click="1" />
  <ri-arrow-right-line class="absolute top-1/2 left-[70px] text-6xl" />
  <p class="absolute top-[calc(50%+.3rem)] left-[140px] text-xl">Document (sin JS)</p>
  <!-- Loading -->
  <ri-arrow-right-line class="absolute top-[calc(50%+1rem)] left-[310px] text-3xl" v-click="1" />
  <p class="absolute top-[calc(50%+.3rem)] left-[350px] text-2xl" v-click="1">Naveador carga<br />todo el HTML<br /> y ejecuta el JS</p>
  <!-- Hidratación -->
  <ri-arrow-right-line class="absolute top-[calc(50%+1rem)] left-[515px] text-3xl" v-click="2" />
  <img src="/js-logo.png" class="absolute top-1/3 right-0 max-w-[140px] h-auto" v-click="2" />
</div>


---
layout: quote
---

# File-based routes

---
layout: full
title: File-based routes
---

# Rutas dinámicas y anidadas

```md
-| pages/
---| index.vue                    # Página Home (URL: /)
---| [category]/                  # Nombre de parámetro dinámico (ejemplo: "/venta-de-coches")
-----| [slug].vue                 # Página para ruta dinámica (ejemplo: "/venta-de-coches/ford-mustang-cabriolet")
```
<br/>

## Ejemplo de página

```vue
<template>
  <h1>{{ product.name }}</h1>
</template>

<script setup lang="ts">
const { slug, category } = useRoute().params

const { data: product } = useAsyncData(`product-${slug}`, () => $fetch(`/api/products/${slug}`))

const { data: links } = useAsyncData(`links-${category}`, () => $fetch(`/api/categories/${category}/links`))
```

<v-clicks depth="2">

- File-based routes: Rutas dinámicas y anidadas
- `useRoute()` para obtener la ruta actual (y sus parámetros)
- `useAsyncData()` para hacer peticiones HTTP

</v-clicks>

---
layout: quote
---

# Data fetching

---
layout: two-cols
title: Data fetching (I)
---

<v-clicks depth="2">

- <span v-mark="{ at: 1, type:'underline', color: '#008f53' }">`useFetch()`</span>
  - composable para hacer peticiones HTTP a una URL de manera SSR-friendly.
  - Es un wrapper de `$fetch` (`ofetch` en Nuxt).
- <span v-mark="{ at: 4, type:'underline', color: '#008f53' }">`useAsyncData()`</span>
  - similar a `useFetch()`: composable asíncrona para hacer peticiones HTTP.
  - Nos da acceso a datos cacheados.
  - Permite hacer múltiples peticiones HTTP en paralelo.
  - Tiene otras funcionalidades como:
    - Manipulación de los datos de la respuesta.
    - Refetching manual o automático (watcher).
    - Error handling
    - etc.

</v-clicks>

::right::

<v-clicks depth="2">

- <span v-mark="{ at: 9, type:'underline', color: '#008f53' }">`$fetch`</span>
  - Función para hacer peticiones HTTP a una URL.
  - Basado en [`ofetch`](https://github.com/unjs/ofetch)
    - Engine-agnostic fetch API.
    - Parseado de respuestas automático (JSON, text, etc.).
    - JSON en el body de la petición.
    - etc.
  - Integrado con las rutas de Nuxt.

</v-clicks>

---
layout: full
title: Data fetching (II)
---

## `useFetch()` / `useAsyncData()` + `$fetch()`

```vue
<script setup lang="ts">
// ❌ Se ejecuta dos veces: una en el servidor y otra en el cliente.
const dataTwice = await $fetch('/api/item')

// ✅ Se ejecuta una vez en el servidor y se transfiere al cliente como "payload".
const { data } = await useAsyncData('item', () => $fetch('/api/item'))

// ✅ Se puede usar `useFetch()` como alias de `useAsyncData()` + `$fetch()`
const { data } = await useFetch('/api/item')

</script>
```
