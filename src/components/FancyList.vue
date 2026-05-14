<template>
  <ul>
    <li
      v-for="item in items"
      :key="item.id"
      my-1
      p-2
      hover:bg="[var(--background-secondary)]"
      transition-colors
      cursor-pointer
    >
      <slot :item="item" />
    </li>
  </ul>
</template>

<script setup lang="ts" generic="T extends { id: string | number }">
interface Props<T> {
  items: T[];
}

defineProps<Props<T>>();
</script>
