<template>
  <section id="skills" style="padding: 120px 40px; max-width: 1120px; margin: 0 auto">
    <div class="reveal">
      <div class="eyebrow">02 / Skills</div>
      <div class="cmt" style="margin-top: 8px">// Skills &amp; tools</div>
    </div>
    <div class="reveal" style="display: flex; gap: 38px; flex-wrap: wrap; align-items: flex-start; margin-top: 34px">
      <div style="flex: 1; min-width: 320px">
        <div style="font-size: 11px; letter-spacing: 0.16em; color: rgba(237, 233, 233, 0.5); margin-bottom: 22px">
          / TECH STACK — hover a glyph
        </div>
        <div style="display: flex; gap: 30px; flex-wrap: wrap">
          <div v-for="item in techStack" :key="item.id" class="tech">
            <div class="pop"><p>{{ item.name }}</p></div>
            <img :src="item.img" :alt="item.name" />
            <span style="font-size: 9px; margin-top: 8px; color: rgba(237, 233, 233, 0.55)">{{ item.short }}</span>
          </div>
        </div>
      </div>
      <div style="flex: 1; min-width: 320px" id="bars">
        <div style="font-size: 11px; letter-spacing: 0.16em; color: rgba(237, 233, 233, 0.5); margin-bottom: 22px">
          / PROFICIENCY
        </div>
        <div style="display: flex; flex-direction: column; gap: 15px">
          <div v-for="skill in proficiency" :key="skill.label">
            <div style="display: flex; justify-content: space-between; font-size: 11px; margin-bottom: 6px">
              <span>{{ skill.label }}</span>
              <span style="color: #e8973a">{{ skill.value }}</span>
            </div>
            <div class="bar"><i :style="{ '--w': skill.value + '%' }"></i></div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import imgVue from "../assets/images/vue.svg";
import imgReact from "../assets/images/react.svg";
import imgTs from "../assets/images/ts.svg";
import imgJs from "../assets/images/js.svg";
import imgTailwind from "../assets/images/tailwind.svg";
import imgScss from "../assets/images/sass.svg";

const techStack = [
  { img: imgReact, name: "React.js", short: "React", id: "react" },
  { img: imgTs, name: "TypeScript", short: "TS", id: "ts" },
  { img: imgVue, name: "Vue.js", short: "Vue", id: "vue" },
  { img: imgJs, name: "JavaScript", short: "JS", id: "js" },
  { img: imgTailwind, name: "Tailwind", short: "Tailwind", id: "tailwind" },
  { img: imgScss, name: "SCSS", short: "SCSS", id: "scss" },
];

const proficiency = [
  { label: "React / TypeScript", value: 90 },
  { label: "Vue / TresJS · 3D", value: 85 },
  { label: "Tailwind / SCSS", value: 88 },
  { label: "Testing · Vitest / RTL", value: 78 },
];
</script>
