<template>
  <!-- desktop / tablet rail -->
  <nav class="rail">
    <div style="font-weight: 700; font-size: 15px; letter-spacing: 0.04em">SB</div>
    <a
      v-for="item in items"
      :key="item.id"
      class="rl"
      :class="{ active: active === item.id }"
      :href="'#' + item.id"
    >
      <span class="n">{{ item.n }}</span>{{ item.label }}
    </a>
    <div
      style="
        margin-top: auto;
        writing-mode: vertical-rl;
        font-size: 8px;
        letter-spacing: 0.2em;
        color: rgba(237, 233, 233, 0.35);
      "
    >
      DRIFTING · EST.2023
    </div>
  </nav>

  <!-- mobile bar + hamburger -->
  <div class="mrail">
    <div class="mrail-bar">
      <div style="font-weight: 700; font-size: 15px; letter-spacing: 0.04em">SB</div>
      <button
        class="mburger"
        :class="{ open: isOpen }"
        @click="isOpen = !isOpen"
        :aria-expanded="isOpen"
        aria-label="Toggle menu"
      >
        <i class="orbit"></i>
        <span></span><span></span><span></span>
      </button>
    </div>

    <div class="mmenu" :class="{ open: isOpen }">
      <a
        v-for="item in items"
        :key="item.id"
        class="mmenu-link"
        :class="{ active: active === item.id }"
        :href="'#' + item.id"
        @click="isOpen = false"
      >
        <span class="n">{{ item.n }}</span>{{ item.label }}
      </a>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

const props = defineProps({
  active: {
    type: String,
    default: "hero",
  },
});

const items = [
  { id: "hero", n: "00", label: "Home" },
  { id: "about", n: "01", label: "About" },
  { id: "skills", n: "02", label: "Skills" },
  { id: "path", n: "03", label: "Path" },
  { id: "work", n: "04", label: "Work" },
  { id: "contact", n: "05", label: "Contact" },
];

const isOpen = ref(false);

watch(isOpen, (open) => {
  document.body.style.overflow = open ? "hidden" : "";
});

watch(
  () => props.active,
  () => {
    isOpen.value = false;
  }
);
</script>
