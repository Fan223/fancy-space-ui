<template>
  <ul style="border: 1px solid red" px-2>
    <li v-for="node in data" :key="(node as any).id">
      <div
        class="flex items-center gap-1 cursor-pointer select-none hover:text-blue-500"
        :class="[
          'hover:text-blue-500',
          isSelected(node) && 'text-blue-600 font-medium bg-blue-50',
        ]"
        @click="handleClick(node)"
      >
        <!-- 内容 -->
        <div>
          <!-- 把 node 传给外部 -->
          <slot name="node" :node="node">
            <!-- 默认内容（如果外部没传 slot，就用这个） -->
            {{ (node as any)[labelKey] }}
          </slot>
        </div>

        <!-- 展开图标 -->
        <span class="w-4 inline-block text-center">
          <template v-if="hasChildren(node)">
            {{ isExpanded(node) ? "▼" : "▶" }}
          </template>
        </span>
      </div>

      <!-- 子节点 -->
      <FancyTree
        v-if="hasChildren(node) && isExpanded(node)"
        :data="getChildren(node)"
        :children-key="childrenKey"
        :label-key="labelKey"
        :expanded-keys="expandedKeys"
        @toggle="emit('toggle', $event)"
        @select="emit('select', $event)"
      >
        <!-- ⭐ 关键：透传 slot -->
        <template #node="slotProps">
          <slot name="node" v-bind="slotProps" />
        </template>
      </FancyTree>
    </li>
  </ul>
</template>

<script setup lang="ts" generic="T">
interface Props<T> {
  labelKey?: keyof T;
  data: T[];
  childrenKey?: keyof T;

  expandedKeys?: (string | number)[];
  selectedKey?: string | number;
}

const props = withDefaults(defineProps<Props<T>>(), {
  childrenKey: "children",
  labelKey: "label",
  expandedKeys: () => [],
});

/**
 * 判断是否选中
 */
const isSelected = (node: T) => {
  return props.selectedKey === (node as any).id;
};

/**
 * 定义事件
 */
const emit = defineEmits<{
  (e: "toggle", node: T): void;
  (e: "select", node: T): void;
}>();

/**
 * 判断是否展开
 */
const isExpanded = (node: T) => {
  return props.expandedKeys.includes((node as any).id);
};

/**
 * 获取子节点
 */
const getChildren = (node: T): T[] | undefined => {
  return node[props.childrenKey] as unknown as T[];
};

/**
 * 是否有子节点
 */
const hasChildren = (node: T) => {
  return !!getChildren(node)?.length;
};

/**
 * 点击节点：
 * - 有 children → toggle
 * - 没 children → select
 */
const handleClick = (node: T) => {
  if (hasChildren(node)) {
    emit("toggle", node);
  } else {
    emit("select", node);
  }
};
</script>
