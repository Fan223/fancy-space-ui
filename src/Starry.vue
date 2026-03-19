<template>
  <div class="starry-container">
    <div class="starry starry1"></div>
    <div class="starry starry2"></div>
    <div class="starry starry3"></div>
  </div>
</template>

<script setup lang="ts">
let handler: (e: MouseEvent) => void;

onMounted(() => {
  let ticking = false;
  let lastX = 0;

  handler = (e: MouseEvent) => {
    lastX = e.clientX;

    if (!ticking) {
      requestAnimationFrame(() => {
        const base = (lastX / window.innerWidth - 0.5) * 20;

        document.documentElement.style.setProperty(
          "--mouse-x-sm",
          `${base * 0.5}px`,
        );
        document.documentElement.style.setProperty("--mouse-x-md", `${base}px`);
        document.documentElement.style.setProperty(
          "--mouse-x-lg",
          `${base * 1.8}px`,
        );

        ticking = false;
      });

      ticking = true;
    }
  };

  window.addEventListener("mousemove", handler);
});

onBeforeUnmount(() => {
  window.removeEventListener("mousemove", handler);
});
</script>

<style scoped lang="scss">
@use "sass:math";
@use "sass:string";

/* ===== 随机生成星星 ===== */
@function generate-shadows($n, $color) {
  $shadows: "#{math.random(100)}vw #{math.random(100)}vh #{$color}";

  @for $i from 2 through $n {
    $shadows: "#{$shadows}, #{math.random(100)}vw #{math.random(100)}vh #{$color}";
  }

  @return string.unquote($shadows);
}

/* ===== 容器 ===== */
.starry-container {
  position: fixed;
  inset: 0;
  background: #1a1a1a;
  overflow: hidden;
  pointer-events: none;
  z-index: -999;
}

/* ===== 银河雾（高级感关键） ===== */
.starry-container::before {
  content: "";
  position: absolute;
  inset: 0;
  background:
    radial-gradient(ellipse at 30% 20%, rgb(120 160 255 / 8%), transparent 60%),
    radial-gradient(ellipse at 70% 80%, rgb(255 220 160 / 6%), transparent 60%);
  pointer-events: none;
}

/* ===== 基础星层 ===== */
.starry {
  position: absolute;
  border-radius: 50%;

  &::after {
    content: "";
    position: absolute;
    top: 100vh;
    width: inherit;
    height: inherit;
    border-radius: inherit;
    box-shadow: inherit;
  }
}

/* ===== 小星（主数量） ===== */
.starry1 {
  --mx: var(--mouse-x-sm);

  will-change: transform;
  width: 1px;
  height: 1px;
  box-shadow: generate-shadows(300, #d6e4ff);
  animation: move-up 25s linear infinite;
  opacity: 0.5;
}

/* ===== 中星 ===== */
.starry2 {
  --mx: var(--mouse-x-md);

  will-change: transform, opacity, filter;
  width: 1.5px;
  height: 1.5px;
  box-shadow: generate-shadows(100, #fff);
  animation:
    move-up 50s linear infinite,
    twinkle-md 6s ease-in-out infinite alternate;
  animation-delay: calc(-1s * #{math.random(60)});
}

/* ===== 大星（少量暖色点缀） ===== */
.starry3 {
  --mx: var(--mouse-x-lg);

  will-change: transform, opacity, filter;
  width: 2.5px;
  height: 2.5px;
  box-shadow: generate-shadows(20, #ffe9a3), generate-shadows(10, #fff);
  animation:
    move-up 80s linear infinite,
    twinkle-lg 9s ease-in-out infinite alternate;
  animation-delay: calc(-1s * #{math.random(60)});
}

/* ===== 上移动画（带轻微横向漂移） ===== */
@keyframes move-up {
  from {
    transform: translate3d(var(--mx, 0), 0, 0);
  }

  to {
    transform: translate3d(calc(var(--mx, 0px) + 1vw - 2vw), -100vh, 0);
  }
}

/* ===== 中等闪烁（GPU友好） ===== */
@keyframes twinkle-md {
  0% {
    opacity: 0.3;
    transform: scale(0.9);
  }

  50% {
    opacity: 1;
    transform: scale(1.1);
  }

  100% {
    opacity: 0.6;
    transform: scale(1);
  }
}

/* ===== 强闪烁 ===== */
@keyframes twinkle-lg {
  0% {
    opacity: 0.4;
    transform: scale(0.85);
  }

  50% {
    opacity: 1;
    transform: scale(1.25);
  }

  100% {
    opacity: 0.7;
    transform: scale(1.05);
  }
}
</style>
