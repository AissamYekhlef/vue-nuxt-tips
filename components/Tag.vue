<template>
  <span class="text-xs font-semibold tracking-wide px-2.5 py-0.5 rounded-full" :class="colorClasses[name]">
    {{ name }}
  </span>
</template>

<script lang="ts" setup>
defineOptions({
  name: 'Tag'
})
type Tagname = 'vue' | 'nuxt' | 'vite' | 'ts'

interface Props {
  name?: Tagname
}

withDefaults(defineProps<Props>(), {
  name: 'vue'
})

const colorClasses: Record<Tagname, string> = {
  vue: 'bg-emerald-500/15 text-emerald-400',
  vite: 'bg-purple-500/15 text-purple-400',
  nuxt: 'bg-teal-500/15 text-teal-400',
  ts: 'bg-teal-500/10 text-blue-400'
}
</script>