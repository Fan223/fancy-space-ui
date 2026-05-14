<template>
  <div
    flex
    items-center
    justify-between
    p-1
    :class="{
      'is-disabled': disabled,
      'has-prefix': !!$slots.prefix,
      'has-suffix': !!$slots.suffix,
    }"
    style="border: 1px solid red"
  >
    <div flex>
      <slot name="prefix" />
    </div>

    <div w-full style="border: 1px solid blue">
      <input
        p-4
        v-model="value"
        :disabled="disabled"
        :placeholder="placeholder"
        v-bind="attrs"
        w-full
        bg="[var(--background-secondary)]"
        outline-none
      />
    </div>

    <div style="border: 1px solid orange">
      <slot name="suffix" />
    </div>
  </div>
</template>

<script setup lang="ts">
interface Props {
  modelValue?: string;
  placeholder?: string;
}

const props = withDefaults(defineProps<Props>(), {
  modelValue: "",
  disabled: false,
  placeholder: "",
});

const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

const attrs = useAttrs();

const value = computed({
  get: () => props.modelValue,
  set: (value: string) => emit("update:modelValue", value),
});
</script>

<style scoped lang="scss"></style>
