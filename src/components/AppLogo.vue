<template>
  <div
    class="inline-flex flex-none items-center justify-center rounded-xl bg-gradient-to-br from-emerald-500 to-teal-700 text-white shadow-panel"
    :style="{ width: `${size}px`, height: `${size}px` }"
  >
    <svg
      :width="Math.round(size * 0.64)"
      :height="Math.round(size * 0.64)"
      viewBox="0 0 24 24"
      fill="none"
      aria-hidden="true"
    >
      <!-- 学士帽帽板 -->
      <path d="M12 3.2 21.4 7.8 12 12.4 2.6 7.8Z" fill="currentColor" />
      <!-- 帽身 -->
      <path
        d="M6.8 10.8v3.1c0 1.7 2.3 3 5.2 3s5.2-1.3 5.2-3v-3.1L12 13.6Z"
        fill="currentColor"
        opacity="0.72"
      />
      <!-- 流苏 -->
      <path
        d="M20.4 8.8v4.4"
        stroke="currentColor"
        stroke-width="1.4"
        stroke-linecap="round"
      />
      <circle cx="20.4" cy="15" r="1.1" fill="currentColor" />
      <!-- 对话气泡 -->
      <path
        d="M4.2 16.6h4.4l-1.2 2.4c-.2.4.2.8.6.6l3.6-3"
        stroke="currentColor"
        stroke-width="1.3"
        stroke-linecap="round"
        stroke-linejoin="round"
        opacity="0.9"
      />
    </svg>
  </div>
</template>

<script setup>
defineProps({
  size: { type: Number, default: 40 },
})
</script>
