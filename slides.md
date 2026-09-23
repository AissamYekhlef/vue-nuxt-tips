---
theme: seriph
background: cover-background.svg
title: Vue & Nuxt Tips
info: |
  ## Vue & Nuxt Tips
  Tips for Vue.js, Nuxt.js
  and Vite developers.

  Learn more at [Sli.dev](https://sli.dev)
class: text-center
drawings:
  persist: false
transition: slide-left
comark: true
mdc: true
duration: 35min
lineNumbers: false
hideInToc: true
seoMeta:
  ogTitle: Vue, Nuxt & TypeScript Tips
  ogDescription: Practical Vue, Nuxt and TypeScript tips for modern web development.
  ogImage: https://aissamyekhlef.github.io/vue-nuxt-tips/og-image.png
  ogUrl: https://aissamyekhlef.github.io/vue-nuxt-tips/
  twitterCard: summary_large_image
  twitterTitle: Vue, Nuxt & TypeScript Tips
  twitterDescription: Practical Vue, Nuxt and TypeScript tips for modern web development.
  twitterImage: https://aissamyekhlef.github.io/vue-nuxt-tips/og-image.png
---

# Vue & Nuxt Tips

practical tips for Vite, Vue and Nuxt developers

<div class="pt-8 opacity-70 text-sm">
  Eng.
  <a href="https://aissamyekhlef.github.io/" target="_blank">Aissam Yekhlef</a>
</div>

<!-- <div class="abs-br m-6 flex gap-2">
Credits:
  <a href="https://vuejstips.com/" target="_blank" class="text-xl opacity-50 hover:opacity-100">
    vuejstips.com
  </a>
</div> -->

<!--
Personal reference deck. Descriptions and code examples are rewritten
from the source site's numbered tip format.
-->

---
layout: default
hideInToc: true
---

# Agenda

<Toc columns="3" maxDepth="3" class="text-xs"  />

---
layout: section
hideInToc: true
---

# 01 – 05
Vite imports & core reactivity

---

# 01 · Import a file as text with `?raw`

<tag name="vite" />

Vite can import a file's contents directly as a string. This is useful
for displaying source code or loading text-based assets.

```ts
import example from './examples/Counter.vue?raw'
import shader from './shaders/gradient.glsl?raw'

console.log(example)
console.log(shader)
```

For an asset URL, use `?url` instead.

---

# 02 · Accept a value, ref, or getter in one composable

<tag name="vue" />

`toValue()` supports plain values, refs, and getter functions. Use it
inside a reactive computation when you want changes to remain tracked.

```ts
import { computed, toValue } from 'vue'
import type { MaybeRefOrGetter } from 'vue'

export function useGreeting(name: MaybeRefOrGetter<string>) {
  return computed(() => `Hello, ${toValue(name)}!`)
}
```

---

# 03 · Pause a watcher during a batch update

<tag name="vue" />

Vue 3.5+ watchers expose `pause()`, `resume()`, and `stop()`. This is
useful when several changes should not trigger the watcher
independently.

```ts {1,8,11}
const { pause, resume } = watch(
  filters,
  value => {
    appliedFilters.value = { ...value }
  },
  { deep: true }
)

function resetFilters() {
  pause()
  filters.value.query = ''
  filters.value.page = 1
  resume()
}
```

---

# 04 · Run a watcher only once

<tag name="vue" />

Vue 3.4+ supports `once: true`, which automatically stops a watcher
after its first callback.

```ts {3}
watch(hasEdited, () => {
  showHint.value = false
}, { once: true })
```

---

# 05 · Limit deep watcher traversal for arrays

<tag name="vue" />

In Vue 3.5+, numeric `deep` values let you control how far Vue traverses
nested structures. `deep: 1` can observe array-level changes without
watching every property inside each item.

```ts {3}
watch(items, () => {
  console.log('Array changed')
}, { deep: 1 })
```

---
layout: section
hideInToc: true
---

# 06 – 12
Nuxt structure & watcher cleanup

---

# 06 · Share utilities and types through `shared/`

<tag name="nuxt" />

Nuxt 3.14+ provides a `shared/` directory for code that can be used from
both server and client contexts.

```text
shared/
  utils/
    format.ts
  types/
    index.ts

server/
  api/
    users.get.ts
```

```ts
// shared/utils/format.ts
export function formatCurrency(amount: number, currency = 'USD') {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency
  }).format(amount)
}
```

---

# 07 · Control when `NuxtLink` prefetches

<tag name="nuxt" />

Nuxt 3.13+ allows link prefetching to be configured around viewport
visibility and user interaction.

```vue
<NuxtLink to="/about">About</NuxtLink>

<NuxtLink to="/heavy-page" prefetch-on="interaction">
  Heavy page
</NuxtLink>

<NuxtLink
  to="/dashboard"
  :prefetch-on="{ visibility: true, interaction: true }"
>
  Dashboard
</NuxtLink>
```

You can also configure defaults in `nuxt.config.ts`.

---

# 08 · Organize routes with route groups

<tag name="nuxt" />

Nuxt 3.13+ supports parenthesized route groups. The folders organize
your source tree without becoming part of the URL.

<Row>
  <template #left>

  ### before

  ```text {1,2,3|all}
  pages/
    marketing/
      about.vue       → /marketing/about
      contact.vue     → /marketing/contact
    shop/
      products.vue    → /shop/products
      cart.vue        → /shop/cart
    auth/
      login.vue       → /auth/login
  ```

  </template>

  <template #right>

  ###  after
  ```text {1,2,3|all}
  pages/
    (marketing)/
      about.vue       → /about
      contact.vue     → /contact
    (shop)/
      products.vue    → /products
      cart.vue        → /cart
    (auth)/
      login.vue       → /login
  ```

  </template>

</Row>

---

# 09 · Use preview mode for draft content

<tag name="nuxt" />

Nuxt's `usePreviewMode()` can expose preview state for draft content.

```vue
<script setup>
const { enabled, state } = usePreviewMode()
</script>

<template>
  <div v-if="enabled">
    Preview mode is active
  </div>
</template>
```

The composable can also be customized with `shouldEnable` and
`getState`.

---

# 10 · Lazy-hydrate async components

<tag name="vue" />

Vue 3.5+ provides hydration strategies for async components. Hydration
can happen when an element becomes visible, when the browser is idle, or
after interaction.

```ts {8|12|16|all}
import { 
 defineAsyncComponent, hydrateOnVisible, 
hydrateOnIdle, hydrateOnInteraction
} from 'vue'

const HeavyChart = defineAsyncComponent({
  loader: () => import('./HeavyChart.vue'),
  hydrate: hydrateOnVisible()
})

const AdBanner = defineAsyncComponent({
  loader: () => import('./AdBanner.vue'),
  hydrate: hydrateOnIdle(5000)
})

const Dropdown = defineAsyncComponent({
  loader: () => import('./Dropdown.vue'),
  hydrate: hydrateOnInteraction(['click'])
})
```

---

# 11 · Defer a `<Teleport>` target

<tag name="vue" />

Vue 3.5+ supports `defer`, allowing a teleport target rendered later in
the same render cycle.

```vue
<Teleport defer to="#container">
  <p>Teleported content</p>
</Teleport>

<div id="container"></div>
```

---

# 12 · Clean up stale watcher work

<tag name="vue" />

`onWatcherCleanup()` is useful for cancelling asynchronous work started
by a watcher, such as an obsolete API request.

```ts {5,7,9}
watch(userId, id => {
  const controller = new AbortController()

  fetch(`/api/users/${id}`, {
    signal: controller.signal
  })

  onWatcherCleanup(() => {
    controller.abort()
  })
})
```

The same pattern can be used with `watchEffect()`.

---
layout: section
hideInToc: true
---

# 13 – 21
Refs, props, slots & scoped styles

---

# 13 · Generate stable unique IDs with `useId()`

<tag name="vue" />

Vue 3.5+ includes `useId()`, which creates IDs that remain consistent
between server and client rendering.

```vue
<script setup>
import { useId } from 'vue'

const id = useId()
</script>

<template>
  <label :for="id">Email</label>
  <input :id="id" type="email">
</template>
```

This is particularly useful for accessible form controls.

---

# 14 · Use `useTemplateRef()` for template refs

<tag name="vue" />

Vue 3.5+ provides `useTemplateRef()` so the JavaScript variable does not
have to match the template ref name.

```vue
<script setup>
import { onMounted, useTemplateRef } from 'vue'

const input = useTemplateRef('email-input')

onMounted(() => {
  input.value?.focus()
})
</script>

<template>
  <input ref="email-input">
</template>
```

---

# 15 · Use reactive destructured prop defaults

<tag name="vue" />

Vue 3.5+ supports native destructuring with defaults while retaining
reactive prop behavior.

```ts
const {
  count = 0,
  message = 'hello'
} = defineProps<{
  count?: number
  message?: string
}>()
```

This can replace many `withDefaults()` use cases.

---

# 16 · Use `defineModel()` for two-way binding

<tag name="vue" />

Vue 3.4+ provides `defineModel()` as a concise way to implement
component `v-model`.

```vue
<script setup>
const model = defineModel()
</script>

<template>
  <input v-model="model">
</template>
```

It avoids manually declaring `modelValue` and `update:modelValue` for
the common case.

---

# 17 · Don't make everything reactive

<tag name="vue" />

Only use `ref()` or another reactive API when the value actually needs
to participate in Vue's reactivity system.

```ts
// Static data does not need ref()
const links = [
  { name: 'About', href: '/about' },
  { name: 'Terms', href: '/terms' }
]

// UI state does need reactivity
const tabs = ref([
  { name: 'Privacy', isActive: true },
  { name: 'Permissions', isActive: false }
])
```

---

# 18 · Use same-name `v-bind` shorthand

<tag name="vue" />

Vue 3.4+ allows a shorter form when the attribute name and JavaScript
variable name are identical.

```html
<!-- Before -->
<img :id="id" :src="src" :alt="alt">

<!-- Short form -->
<img :id :src :alt>
```

---

# 19 · Use `shallowRef()` for large objects

<tag name="vue" />

`shallowRef()` tracks replacement of the `.value`, but does not make
nested properties reactive.

```ts
const state = shallowRef({ count: 1 })

// Nested mutation is not tracked
state.value.count = 2

// Replacing the value is tracked
state.value = { count: 2 }
```

---

# 20 · Type component emits with TypeScript

<tag name="vue" />

Typed emits improve editor support and catch incorrect event payloads.

```ts
const emit = defineEmits<{
  change: [id: number]
  update: [value: string]
}>()
```

---

# 21 · Bind reactive values directly from CSS

<tag name="vue" />

Vue's `v-bind()` can expose component state to a `<style>` block.

```vue
<style scoped>
button {
  background-color: v-bind(backgroundColor);
}
</style>
```

When `backgroundColor` changes, the generated CSS variable is updated.

---
layout: section
hideInToc: true
---

# 22 – 28
Data fetching & directives

---

# 22 · Manage request state with TanStack Vue Query

<tag name="vue" />

TanStack Vue Query can handle fetching, loading/error states, caching,
and query identity.

```ts
import { useQuery } from '@tanstack/vue-query'

async function fetchPosts() {
  const response = await fetch('/api/posts')

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`)
  }
  return response.json()
}

const {
  data: posts,
  isPending,
  isError,
  error
} = useQuery({
  queryKey: ['posts'],
  queryFn: fetchPosts
})
```

---

# 23 · Apply one global selector inside scoped styles

<tag name="vue" />

Use `:global()` when a single rule needs to escape scoped-style
behavior.

```vue
<style scoped>
:global(.red) {
  color: red;
}
</style>
```

---

# 24 · Reduce font-related layout shift with Fontaine

<tag name="nuxt" />

The Nuxt Fontaine module can generate local font fallbacks to reduce
cumulative layout shift.

```bash
npm install -D @nuxtjs/fontaine
```

```ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/fontaine']
})
```

---

# 25 · Provide defaults for type-only props

<tag name="vue" />

`withDefaults()` lets TypeScript-based `defineProps()` declarations have
runtime defaults.

```ts
interface Props {
  variant?: 'primary' | 'secondary'
  disabled?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'primary',
  disabled: false
})
```

---

# 26 · Search Nuxt Content collections

<tag name="nuxt" />

Nuxt Content v3 exposes `queryCollectionSearchSections()` for obtaining
searchable sections.

```ts
const search = ref('')

const { data: sections } = await useAsyncData(
  'content-search',
  () => queryCollectionSearchSections('content')
)
const results = computed(() => {
  const query = search.value.trim().toLowerCase()

  if (!query) return []

  return (sections.value ?? []).filter(section =>
    `${section.title} ${section.content}`
      .toLowerCase()
      .includes(query)
  )
})
```

For large collections, prefer server-side search instead of downloading
all searchable sections to the browser.

---

# 27 · Get previous and next Nuxt Content entries

<tag name="nuxt" />

`queryCollectionItemSurroundings()` can retrieve the entries immediately
before and after the current document.

```ts
const route = useRoute()

const { data: surround } = await useAsyncData(
  `surround-${route.path}`,
  () => queryCollectionItemSurroundings('content', route.path)
    .order('title', 'ASC')
)
```

Render the surrounding items conditionally because the first or last
page has only one neighbor.

---

# 28 · Define a local custom directive

<tag name="vue" />

Inside `<script setup>`, an object beginning with `v` can be used as a
local directive.

```vue
<script setup>
const vFocus = {
  mounted: el => el.focus()
}
</script>

<template>
  <input v-focus>
</template>
```

---
layout: section
hideInToc: true
---

# 29 – 37
Tooling, SEO feeds & scoped CSS

---

# 29 · Configure a Vite + TypeScript import alias

<tag name="vite" />

The alias needs to be known by both Vite and TypeScript.

```ts
// vite.config.ts
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```
---

 
Then add the matching TypeScript path:

```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

---
---

# 30 · Add Nuxt Content pages to a sitemap

<tag name="nuxt" />

Nuxt Sitemap can consume a server endpoint that returns page paths from
Nuxt Content.

```bash
npm install @nuxtjs/sitemap
```

```ts
// server/api/sitemap-urls.ts
import { queryCollection } from '@nuxt/content/server'

export default defineEventHandler(async event => {
  const pages = await queryCollection(event, 'content')
    .select('path')
    .all()

  return pages.map(page => ({
    loc: page.path
  }))
})
```

Only expose public pages, and use real modification dates when supplying
`lastmod`.

---

# 31 · Generate an RSS feed from Nuxt Content

<tag name="nuxt" />

A server route can query Nuxt Content and build an RSS document.

```bash
npm install rss
npm install -D @types/rss
```
---


```ts
// server/routes/rss.xml.get.ts
import RSS from 'rss'
import { queryCollection } from '@nuxt/content/server'

export default defineEventHandler(async event => {
  const siteUrl = 'https://example.com'

  const feed = new RSS({
    title: 'Engineering notes',
    site_url: siteUrl,
    feed_url: `${siteUrl}/rss.xml`
  })

  const pages = await queryCollection(event, 'content').all()

  for (const page of pages) {
    const url = new URL(page.path, siteUrl).href

    feed.item({
      title: page.title,
      description: page.description ?? '',
      url,
      guid: url
    })
  }

  setHeader(event, 'content-type', 'application/rss+xml; charset=utf-8')

  return feed.xml({ indent: true })
})
```

---

# 32 · Reach child component elements with `:deep()`

<tag name="vue" />

Scoped CSS normally stops at component boundaries. `:deep()` lets a
scoped selector target nested child markup.

```vue
<style scoped>
.wrapper :deep(.child-element) {
  /* styles */
}
</style>
```

---

# 33 · Style slot content with `:slotted()`

<tag name="vue" />

Use `:slotted()` when scoped CSS needs to target content supplied
through a slot.

```vue
<style scoped>
:slotted(div) {
  color: red;
}
</style>
```

---

# 34 · Preserve component state with `<KeepAlive>`

<tag name="vue" />

Dynamic components normally unmount when replaced. `<KeepAlive>` caches
component instances so their local state can survive switching.

```vue
<KeepAlive>
  <component :is="activeComponent" />
</KeepAlive>
```

---

# 35 · Use multiple named slots

<tag name="vue" />

Named slots let a reusable component expose multiple insertion points.

````md magic-move
```vue
<!-- Child -->
<template>
  <div class="input-wrapper">
    <label>
      <slot name="label" />
    </label>

    <input>

    <div class="input-icon">
      <slot name="icon" />
    </div>
  </div>
</template>
```

```vue
<!-- Parent -->
<Input>
  <template #label>Email</template>
  <template #icon><EmailIcon /></template>
</Input>
```
````

---

# 36 · Coordinate async dependencies with `<Suspense>`

<tag name="vue" />

`<Suspense>` can display a fallback while nested async dependencies
resolve.

```vue
<Suspense>
  <Dashboard />

  <template #fallback>
    Loading...
  </template>
</Suspense>
```

Check the current Vue documentation for the feature's current stability
and API details.

---

# 37 · Render server-only components with Nuxt Islands

<tag name="nuxt" />

Nuxt Islands can keep component JavaScript on the server for components
that do not need client-side interactivity.

```ts
// nuxt.config.ts
export default defineNuxtConfig({
  experimental: {
    componentIslands: true
  }
})
```

Place the appropriate component in the islands directory and render it
with `NuxtIsland`.

```vue
<NuxtIsland name="Hello">
</NuxtIsland>
```

---
layout: section
hideInToc: true
---

# 38 – 45
Teleport, DX & v-model modifiers

---

# 38 · Teleport UI outside the component hierarchy

<tag name="vue" />

`<Teleport>` is useful for modals, overlays, menus, and similar UI that
needs to render elsewhere in the DOM.

```html
<Teleport to="body">
  <div v-if="open" class="modal">
    <p>Hello from the modal.</p>
    <button @click="open = false">Close</button>
  </div>
</Teleport>
```

---

# 39 · Enable Vue performance tracing in development

<tag name="vue" />

Vue can expose component performance information to browser development
tools.

```ts {3|all|d}
const app = createApp(App)

app.config.performance = true

app.mount('#app')
```

Use this during development rather than enabling it as a production
optimization.

---

# 40 · Store component definitions in `shallowRef()`

<tag name="vue" />

`shallowRef()` is appropriate for reactive references to component
definitions.

```vue {maxHeight:'260px'}
<script setup>
import { shallowRef } from 'vue'
import UserSettings from './UserSettings.vue'
import UserNotifications from './UserNotifications.vue'

const activeComponent = shallowRef(UserSettings)
</script>

<template>
  <button @click="activeComponent = UserSettings">
    Settings
  </button>

  <button @click="activeComponent = UserNotifications">
    Notifications
  </button>

  <component :is="activeComponent" />
</template>
```

If the component's local state must survive switching, combine this
pattern with `<KeepAlive>`.

---

# 41 · Run code once with `callOnce()`

<tag name="nuxt" />

Nuxt's `callOnce()` is useful for initialization that should execute
once in the relevant rendering/navigation lifecycle.

```ts
const config = useState('config')

await callOnce(async () => {
  config.value = await $fetch('/api/website-config')
})
```

---

# 42 · Shorten boolean props

<tag name="vue" />

A boolean prop whose value is explicitly `true` can use attribute
shorthand.

```vue
<!-- Short -->
<BlogPost is-published />

<!-- Equivalent -->
<BlogPost :is-published="true" />
```

---

# 43 · Use the `.lazy` `v-model` modifier

<tag name="vue" />

`.lazy` updates the model on `change` rather than on every `input`
event.

```vue
<input v-model.lazy="message">
```

---

# 44 · Convert input values to numbers

<tag name="vue" />

`.number` asks Vue to convert compatible input values to numbers.

```vue
<input v-model.number="age">
```

---

# 45 · Trim input whitespace automatically

<tag name="vue" />

`.trim` removes surrounding whitespace from text input values.

```html
<input v-model.trim="message">
```

---
layout: section
hideInToc: true
---

# 46 – 53
Dev server, exposing state & final polish

---

# 46 · Expose a Nuxt dev server through a tunnel

<tag name="vue" />

Nuxt can start its development server with tunneling enabled.

```bash
npx nuxt dev --tunnel
```

This can be useful for testing webhooks or sharing a local development
environment.

---

# 47 · Run the Nuxt development server over HTTPS

<tag name="nuxt" />

Nuxt supports starting the development server with HTTPS and a
self-signed certificate.

```bash
npx nuxt dev --https
```

---

# 48 · Expose values from `<script setup>`

<tag name="vue" />

Components using `<script setup>` are closed by default.
`defineExpose()` explicitly makes selected values accessible through a
component ref.

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

defineExpose({
  count
})
</script>
```

---

# 49 · Refresh a cookie value with `refreshCookie()`

<tag name="nuxt" />

Nuxt 3.10+ provides `refreshCookie()` for cases where a cookie has
changed outside the current `useCookie()` state.

```ts
const token = useCookie('token')

async function login() {
  await $fetch('/api/token')

  refreshCookie('token')
}
```

---

# 50 · Parent and child component classes are merged

<tag name="vue" />

When a component renders a single root element, classes passed by the
parent are merged with classes declared on the child root.

````md magic-move
```vue
<!-- Parent -->
<Table class="py-2" />
```

```vue
<!-- Table.vue -->
<template>
  <table class="border-2 border-sky-500">
    <!-- ... -->
  </table>
</template>
```
````

The rendered element receives both sets of classes.

---

# 51 · Use Lightning CSS for CSS transformation

<tag name="vite" />

Lightning CSS can be used by Vite for CSS transformation and
minification.

```bash
npm install -D lightningcss browserslist
```
---

```ts
// vite.config.ts
import { defineConfig } from 'vite'
import browserslist from 'browserslist'
import { browserslistToTargets } from 'lightningcss'

export default defineConfig({
  css: {
    transformer: 'lightningcss',
    lightningcss: {
      targets: browserslistToTargets(
        browserslist('>= 0.25%')
      )
    }
  },
  build: {
    cssMinify: 'lightningcss'
  }
})
```
Adjust the browser query to match your project's supported browsers.

---

# 52 · Enable custom formatters for Vue reactivity in DevTools

<tag name="vue" />

Vue's reactive values can be easier to inspect in Chromium DevTools when
custom formatters are enabled.

In Chrome/Chromium DevTools, enable:

```text
Console → Enable custom formatters
```

---

# 53 · Use Vue DevTools for Vue applications

<tag name="vue" />

If you want a Vue-focused developer-tools experience similar to Nuxt
DevTools, see the Vue DevTools Next project:

[devtools-next.vuejs.org](https://devtools-next.vuejs.org/)

---
layout: center
class: text-center
---

# Thank You

The tips in this deck are based on:

[VuejsTips](https://vuejstips.com/) by [Michał Kuncio](https://michalkuncio.com/)
&nbsp;·&nbsp;
[Aissam Yekhlef](https://aissamyekhlef.github.io/)
