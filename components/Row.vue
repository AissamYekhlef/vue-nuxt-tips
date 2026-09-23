<template>
  <div class="two-column">
    <div class="left">
      <slot name="left" />
    </div>

    
    <div class="center" v-if="$slots.center || $slots.default">
      <slot />
    </div>

    <div class="right">
      <slot name="right" />
    </div>
  </div>
</template>

<style scoped>
.two-column {
  display: flex;
  flex-direction: row;
  width: 100%;
  gap: 10px;
}

.left,
.center,
.right {
  flex: 1;
}
</style>
