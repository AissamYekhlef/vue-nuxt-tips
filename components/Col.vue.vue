<script setup lang="ts">
interface Props {
  cols?: number
}

const props = withDefaults(defineProps<Props>(), {
  cols: 1,
})
</script>

<template>
  <div
    class="col"
    :style="{ flex: props.cols }"
  >
    <slot />
  </div>
</template>

<style scoped>
.col {
  min-width: 0;
}
</style>