<template>
  <section id="work" style="padding: 110px 0 60px; position: relative; overflow: hidden">
    <div style="max-width: 1120px; margin: 0 auto; padding: 0 40px" class="reveal">
      <div class="eyebrow">04 / Work</div>
      <div class="cmt" style="margin-top: 8px">// Projects — pulled from the drawer</div>
      <p style="color: rgba(237, 233, 233, 0.55); font-size: 12px; margin: 10px 0 0">
        Each folder is a project. Hover to bring it into focus.
      </p>
    </div>
    <div class="stage-wrap">
      <div id="stage" class="stage" ref="stageEl">
        <img class="cabinet" src="../assets/images/cabinet.svg" alt="filing cabinet" />

        <div
          v-for="(project, i) in projects"
          :key="project.name"
          class="folder"
          :class="{ feat: i === 0 }"
          :style="folderStyle(project, i)"
        >
          <div class="fbody">
            <div class="ftab">{{ project.name }}</div>
            <div class="fshot">
              <img :src="project.img" :alt="project.name" />
              <div class="brk" style="left: 4px; top: 4px; border-left: 2px solid #000; border-top: 2px solid #000"></div>
              <div class="brk" style="right: 4px; top: 4px; border-right: 2px solid #000; border-top: 2px solid #000"></div>
              <div class="brk" style="left: 4px; bottom: 4px; border-left: 2px solid #000; border-bottom: 2px solid #000"></div>
              <div class="brk" style="right: 4px; bottom: 4px; border-right: 2px solid #000; border-bottom: 2px solid #000"></div>
            </div>
            <div class="fmeta">
              <div class="fdesc">Desc: {{ project.desc }}</div>
              <div class="flinks">
                <div v-if="project.live" class="btn solid"><a :href="project.live" target="_blank">Live</a></div>
                <div v-if="project.code" class="btn ghost"><a :href="project.code" target="_blank">Code</a></div>
                <div v-if="!project.live && !project.code" class="btn ghost"><a>Internal tool</a></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

import imgQueisV2 from "../assets/images/screenshots/queistal-v2.png";
import imgMonitor from "../assets/images/screenshots/monitor.png";
import imgHow2own from "../assets/images/screenshots/how2ow.png";
import imgFakturka from "../assets/images/screenshots/fakturka.png";
import imgMoody from "../assets/images/screenshots/moody.png";

const projects = [
  {
    img: imgQueisV2,
    name: "Queis Tal v2",
    desc: "Bilingual (PL/DE) marketing site — React 19 + TypeScript",
    live: "https://queistal.pl",
    code: "https://github.com/szymon-bosiak/queistal-v2",
    tx: "30px",
    ty: "60px",
    rot: "-6deg",
    delay: ".05s",
  },
  {
    img: imgMonitor,
    name: "Tender Monitor",
    desc: "Public-tender monitoring app — React 19 + TS, Supabase",
    live: null,
    code: null,
    tx: "378px",
    ty: "26px",
    rot: "5deg",
    delay: ".13s",
  },
  {
    img: imgHow2own,
    name: "How2OW",
    desc: "Overwatch 2 game guide",
    live: "https://how2ow.com",
    code: "https://github.com/szymon-bosiak/how2ow/",
    tx: "66px",
    ty: "344px",
    rot: "-3deg",
    delay: ".21s",
  },
  {
    img: imgFakturka,
    name: "Fakturka",
    desc: "Invoice template app",
    live: "https://szymon-bosiak.github.io/Fakturka/",
    code: "https://github.com/szymon-bosiak/Fakturka",
    tx: "384px",
    ty: "312px",
    rot: "5deg",
    delay: ".29s",
  },
  {
    img: imgMoody,
    name: "The Moody Cloud",
    desc: "Weather app",
    live: "https://szymon-bosiak.github.io/The_Moody_Cloud/",
    code: "https://github.com/szymon-bosiak/The_Moody_Cloud",
    tx: "214px",
    ty: "474px",
    rot: "-2deg",
    delay: ".37s",
  },
];

const ORIGIN = { x: "700px", y: "340px" };

const folderStyle = (project) => ({
  "--tx": project.tx,
  "--ty": project.ty,
  "--rot": project.rot,
  "--ox": ORIGIN.x,
  "--oy": ORIGIN.y,
  transitionDelay: project.delay,
});

const stageEl = ref(null);
let onResize = null;

const scaleStage = () => {
  const wrap = stageEl.value?.parentElement;
  const stage = stageEl.value;
  if (!wrap || !stage) return;
  const s = Math.min(1, wrap.clientWidth / 1000);
  stage.style.transform = `scale(${s})`;
  wrap.style.height = `${720 * s + 8}px`;
};

onMounted(() => {
  scaleStage();
  onResize = () => scaleStage();
  window.addEventListener("resize", onResize);
});

onUnmounted(() => {
  if (onResize) window.removeEventListener("resize", onResize);
});
</script>
