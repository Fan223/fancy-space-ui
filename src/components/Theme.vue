<template>
  <div :style="{ height: props.size, width: props.size }" cursor-pointer>
    <SizedBox
      class="i-ic:twotone-contrast"
      v-if="'os' === theme"
      @click="changeTheme('light')"
      fill
    />
    <SizedBox
      class="i-line-md:moon-filled-to-sunny-filled-loop-transition"
      v-if="'light' === theme"
      @click="changeTheme('dark')"
      fill
    />
    <SizedBox
      class="i-line-md:sunny-filled-loop-to-moon-filled-loop-transition"
      v-if="'dark' === theme"
      @click="changeTheme('light')"
      fill
    />
  </div>
</template>

<script setup lang="ts">
import theme from "@/assets/ts/theme";
import { type Theme } from "@/assets/ts/theme";

const props = withDefaults(defineProps<{ size?: string }>(), {
  size: "2rem",
});

function changeTheme(val: Theme) {
  theme.value = val;
}
</script>
