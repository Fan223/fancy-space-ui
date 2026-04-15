<template>
  <div h-500 class="article">
    <div v-for="article in articles" :key="article.id" h-50 w-50>
      <div
        style="background: var(--main-gradient)"
        rd-4
        op-50
        mix-blend-screen
        z-1
        h-full
      />
      <!-- 遮盖层 -->
      <div
        :style="{
          background: `var(--article-cover-vertical)`,
        }"
        z-2
      />

      {{ article.name }}
    </div>
  </div>
</template>

<script lang="ts" setup>
const articles = [
  {
    name: "列表1",
    id: "1",
  },
  {
    name: "列表1",
    id: "2",
  },
  {
    name: "列表1",
    id: "3",
  },
  {
    name: "列表1",
    id: "4",
  },
];
</script>

<style lang="scss" scoped>
.article {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
}
</style>
