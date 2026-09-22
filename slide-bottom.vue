<template>
  <footer>
    <div class="fixed bottom-2 left-10 text-xs opacity-40 tabular-nums z-10">
        <button class="icon-btn" title="Next" @click="prev">
            <div class="i-carbon:arrow-left" />
        </button>
    </div>
    <div class="fixed bottom-2 right-50% text-xs opacity-40 tabular-nums z-10">
        {{ currentPage }} / {{ total}}
    </div>
    <div class="fixed bottom-2 right-10 text-xs opacity-40 tabular-nums z-10">
        <button class="icon-btn" title="Next" @click="next">
            <div class="i-carbon:arrow-right" />
        </button>
    </div>
  </footer>
</template>

<script setup>
import {  useNav } from '@slidev/client'

const { currentPage, total, next, prev } = useNav()
</script>