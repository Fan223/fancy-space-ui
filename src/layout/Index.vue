<template>
  <Header />

  <main :style="{ gridTemplateColumns: gridTemplate }" grid gap-6 m-8>
    <aside v-if="hasLeft" style="border: 1px solid red">
      <RouterView name="left" />
    </aside>

    <div class="content" style="border: 1px solid blue">
      <RouterView />
    </div>

    <aside v-if="hasRight" style="border: 1px solid orange">
      <RouterView name="right" />
    </aside>
  </main>
</template>

<script lang="ts" setup>
import Header from "./Header.vue";

const route = useRoute();
// 取最后一个即当前子路由
const currentMatched = computed(() => route.matched.at(-1));
const hasLeft = computed(() => !!currentMatched.value?.components?.left);
const hasRight = computed(() => !!currentMatched.value?.components?.right);

const gridTemplate = computed(() => {
  if (hasLeft.value && hasRight.value) {
    return "240px minmax(0,1fr) 300px";
  } else if (hasLeft.value) {
    return "240px minmax(0,1fr)";
  } else if (hasRight.value) {
    return "minmax(0,1fr) 270px";
  }
  return "minmax(0,1fr)";
});
</script>

<style lang="scss" scoped></style>
