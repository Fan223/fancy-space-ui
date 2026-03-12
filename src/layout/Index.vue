<template>
  <Header />

  <main class="layout">
    <aside v-if="hasLeft" style="border: 1px solid red" class="sidebar-left">
      <RouterView name="left" />
    </aside>

    <div class="content" style="border: 1px solid blue">
      <RouterView />
    </div>

    <aside
      v-if="hasRight"
      style="border: 1px solid orange"
      class="sidebar-right"
    >
      <RouterView name="right" />
    </aside>
  </main>

  <BackTop />
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
    return "240px minmax(0,1fr) 240px";
  } else if (hasLeft.value) {
    return "240px minmax(0,1fr)";
  } else if (hasRight.value) {
    return "minmax(0,1fr) 240px";
  }
  return "minmax(0,1fr)";
});
</script>

<style lang="scss" scoped>
.layout {
  --sidebar-left-width: 240px;
  --sidebar-right-width: 240px;
  --content-max-width: 1100px;
  --layout-max-width: 1340px;

  display: grid;
  grid-template-columns:
    var(--sidebar-left-width)
    minmax(0, var(--content-max-width))
    var(--sidebar-right-width);
  gap: 32px;
  max-width: var(--layout-max-width);
  margin: 0 auto;
}

.sidebar-left {
  position: sticky;
  top: 5rem;
  height: calc(100vh - 6rem);
  overflow-y: auto;
}

.sidebar-right {
  position: sticky;
  top: 5rem;
  height: calc(100vh - 6rem);
  overflow-y: auto;
}

.content {
  width: 100%;
  max-width: 820px;
}

@media (width <= 1200px) {
  .layout {
    grid-template-columns:
      var(--sidebar-left-width)
      minmax(0, 1fr);
  }

  .sidebar-right {
    display: none;
  }
}

@media (width <= 768px) {
  .layout {
    grid-template-columns: 1fr;
  }

  .sidebar-left {
    display: none;
  }
}
</style>
