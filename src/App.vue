<template>
  <div>
    <Loading />
    <div class="void"></div>
    <div class="void b"></div>

    <Rail :active="activeSection" />

    <div class="content">
      <Hero />
      <About />
      <Skills />
      <Path />
      <Work />
      <Contact />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import Loading from "./components/Loading.vue";
import Rail from "./components/Rail.vue";
import Hero from "./components/Hero.vue";
import About from "./components/About.vue";
import Skills from "./components/Skills.vue";
import Path from "./components/Path.vue";
import Work from "./components/Work.vue";
import Contact from "./components/Contact.vue";

const activeSection = ref("hero");

let revealObserver = null;
let navObserver = null;

onMounted(() => {
  revealObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("in");
          if (entry.target.id === "stage") entry.target.classList.add("flew");
        }
      });
    },
    { threshold: 0.15 }
  );
  document.querySelectorAll(".reveal, #bars, #stage").forEach((el) => revealObserver.observe(el));

  const sections = ["hero", "about", "skills", "path", "work", "contact"];
  navObserver = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) activeSection.value = entry.target.id;
      });
    },
    { threshold: 0.5 }
  );
  sections.forEach((id) => {
    const el = document.getElementById(id);
    if (el) navObserver.observe(el);
  });
});

onUnmounted(() => {
  if (revealObserver) revealObserver.disconnect();
  if (navObserver) navObserver.disconnect();
});
</script>
