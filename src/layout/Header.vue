<template>
  <header class="header" flex="~ justify-between items-center" p="x-8 y-4">
    <Logo />

    <div @click="$router.push('/blog')">博客</div>
    <div @click="$router.push('/tool')">在线工具</div>

    <Theme />
  </header>
</template>

<script lang="ts" setup></script>

<style lang="scss" scoped>
.header {
  border: 1px solid red;
}
</style>
