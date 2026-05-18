<template>
  <div
    flex
    items-center
    bg="[var(--background-secondary)]"
    border-b-1
    rounded-t-1
    :class="
      focused ? 'border-b-[var(--text-accent)]' : 'border-b-[var(--text-dim)]'
    "
  >
    <!-- 前缀插槽 -->
    <div v-if="$slots.prefix" ml-2>
      <slot name="prefix" />
    </div>

    <!-- 输入框 -->
    <input
      v-model="value"
      :placeholder="placeholder"
      outline-none
      flex-1
      w-0
      p="x-2 y-4"
      @focus="focused = true"
      @blur="focused = false"
      @keydown.enter="emit('enter')"
      @keydown.escape="handleClear"
    />

    <div v-if="(clearable && value) || $slots.suffix" flex gap-2 mr-2>
      <!-- 清除按钮 -->
      <div
        v-if="clearable && value"
        i-ic:twotone-close
        cursor-pointer
        class="text-[var(--text-dim)] hover:text-[var(--text-accent)]"
        @click.stop="handleClear"
      />

      <!-- 后缀插槽 -->
      <slot name="suffix" />
    </div>
  </div>
</template>

<script setup lang="ts">
interface Props {
  modelValue?: string;
  placeholder?: string;
  clearable?: boolean;
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: "",
  placeholder: "搜索",
  clearable: true,
});

const focused = ref(false);

const emit = defineEmits<{
  "update:modelValue": [value: string];
  enter: [];
}>();

const value = computed({
  get: () => props.modelValue,
  set: (val: string) => emit("update:modelValue", val),
});

function handleClear() {
  emit("update:modelValue", "");
}
</script>
