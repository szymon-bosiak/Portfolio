<template>
  <section id="path" style="padding: 120px 40px; max-width: 1120px; margin: 0 auto">
    <div class="reveal">
      <div class="eyebrow">03 / Path</div>
      <div class="cmt" style="margin-top: 8px">// The trajectory</div>
    </div>
    <div class="reveal" style="margin-top: 40px; display: flex; gap: 18px">
      <div style="display: flex; flex-direction: column; align-items: center; padding-top: 5px">
        <template v-for="(step, i) in steps" :key="step.title">
          <div class="node"></div>
          <div
            v-if="i < steps.length - 1"
            style="width: 2px; flex: 1; background: rgba(237, 233, 233, 0.2); min-height: 64px"
          ></div>
        </template>
      </div>
      <div style="flex: 1; display: flex; flex-direction: column; gap: 44px">
        <div v-for="step in steps" :key="step.title">
          <div class="eyebrow" style="font-size: 11px">{{ step.period }}</div>
          <div style="font-weight: 700; font-size: 16px; margin: 5px 0">{{ step.title }}</div>
          <p style="margin: 0; color: rgba(237, 233, 233, 0.65); font-size: 13px; line-height: 1.6; max-width: 560px">
            {{ step.desc }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const steps = [
  {
    period: "NOV 2024 — NOW",
    title: "Frontend Developer @ Codetain",
    desc: "Lead frontend contributor on two products: a B2B EV fleet charging platform (React 19, TypeScript, MUI 7 + MUI X, TanStack Router/Query/Table, Vitest + react-testing-library) and the new company website (Next.js 15, Storybook 9, Tailwind 4, Contentful). Code review, documented conventions, Git flow.",
  },
  {
    period: "APR — NOV 2024",
    title: "Freelance frontend",
    desc: "Websites and web apps for clients — a timber-construction company site, an invoicing app, a weather app (Vue 3, Tailwind, SCSS, Vite). Requirements, estimates and delivery handled end-to-end.",
  },
  {
    period: "2022 — 2024",
    title: "Learning the craft",
    desc: "The self-taught route: JavaScript, HTML/CSS and the front-end ecosystem — turning tutorials into shipped side projects like a game guide and this 3D portfolio.",
  },
  {
    period: "BEFORE CODE",
    title: "Business first",
    desc: "Master of Laws (University of Wrocław), then co-founded and ran a product brand. The business and client experience now feeds a product-minded approach to development.",
  },
];
</script>
