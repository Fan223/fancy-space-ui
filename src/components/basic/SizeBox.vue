<template>
  <component :is="as" :style="style" :class="{ fill: fill }">
    <slot />
  </component>
</template>

<script setup lang="ts">
interface Props {
  size?: string;
  height?: string;
  width?: string;
  fill?: boolean;
  as?: keyof HTMLElementTagNameMap;
}

const props = withDefaults(defineProps<Props>(), {
  as: "div",
});

const style = computed(() => ({
  height: props.height ?? props.size,
  width: props.width ?? props.size,
}));
</script>

<style lang="scss" scoped>
.fill {
  height: 100%;
  width: 100%;
}
</style>
