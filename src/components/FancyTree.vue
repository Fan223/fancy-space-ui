<template>
  <ul>
    <li v-for="node in data" :key="node.id">
      <div
        flex
        gap-1
        justify-between
        items-center
        cursor-pointer
        hover:bg="[var(--background-secondary)]"
        transition-colors
        my-1
        p="r-2 y-2"
        :style="{ paddingLeft: `calc(0.5rem + ${props.level} * 1rem)` }"
        :class="{
          'bg-[var(--background-secondary)] text-[var(--text-accent)]':
            isSelected(node),
        }"
        @click="handleClick(node)"
      >
        <!-- 作用域插槽, 把 node 传给外部 -->
        <slot name="node" :node="node">
          <!-- 如果外部没传 slot, 默认节点值 -->
          <div>{{ (node as any)[props.label] }}</div>
        </slot>

        <!-- 展开图标 -->
        <div v-if="hasChildren(node)" size-6 flex-shrink-0>
          <div
            class="i-ic:twotone-chevron-right"
            v-if="!isExpanded(node)"
            size-full
          />
          <div
            class="i-ic:twotone-keyboard-arrow-down"
            v-if="isExpanded(node)"
            size-full
          />
        </div>
      </div>

      <!-- 子节点 -->
      <FancyTree
        v-if="hasChildren(node) && isExpanded(node)"
        :label="props.label"
        :data="getChildren(node) || []"
        :children="props.children"
        :selected="props.selected"
        :expanded="mergedExpanded"
        @expanded="toggleNode(node)"
        :level="props.level + 1"
        @toggle="emit('toggle', $event)"
        @select="emit('select', $event)"
      >
        <!-- 透传 slot -->
        <template #node="{ node }">
          <slot name="node" :node="node" />
        </template>
      </FancyTree>
    </li>
  </ul>
</template>

<script setup lang="ts" generic="T extends { id: string | number }">
interface Props<T> {
  label?: string;
  data: T[];
  children?: string;
  expanded?: (string | number)[];
  selected?: string | number;
  level?: number;
}

const props = withDefaults(defineProps<Props<T>>(), {
  label: "label",
  children: "children",
  level: 0,
});

defineSlots<{
  node(props: { node: T }): T;
}>();

const emit = defineEmits<{
  (e: "toggle", node: T): void;
  (e: "select", node: T): void;
}>();

/**
 * 判断是否选中
 */
const isSelected = (node: T) => {
  return props.selected === node.id;
};

const innerExpanded = ref<(string | number)[]>([]);

const mergedExpanded = computed(() => {
  return props.expanded ?? innerExpanded.value;
});

/**
 * 判断是否展开
 */
const isExpanded = (node: T) => {
  return mergedExpanded.value.includes(node.id);
};

const toggleNode = (node: T) => {
  // 如果外部没传 expanded → 用内部状态
  if (props.expanded === undefined) {
    const i = innerExpanded.value.indexOf(node.id);
    if (i > -1) innerExpanded.value.splice(i, 1);
    else innerExpanded.value.push(node.id);
  } else {
    emit("toggle", node);
  }
};

/**
 * 获取子节点
 */
const getChildren = (node: T): T[] | undefined => {
  return node[props.children as keyof T] as unknown as T[];
};

/**
 * 是否有子节点
 */
const hasChildren = (node: T) => {
  return !!getChildren(node)?.length;
};

/**
 * 点击节点
 */
const handleClick = (node: T) => {
  if (hasChildren(node)) {
    toggleNode(node);
  } else {
    emit("select", node);
  }
};
</script>
