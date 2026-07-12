<template>
  <section
    id="hero"
    style="position: relative; min-height: 100vh; display: flex; align-items: center; padding: 0 40px"
  >
    <canvas ref="canvasEl" style="position: absolute; inset: 0; width: 100%; height: 100%; z-index: 0"></canvas>
    <div style="position: relative; z-index: 2; max-width: 640px">
      <div class="eyebrow" style="margin-bottom: 14px">Frontend developer · React &amp; TypeScript · floating somewhere</div>
      <h1 style="margin: 0; font-size: clamp(44px, 7vw, 88px); line-height: 0.98; font-weight: 700">
        Szymon<br />Bosiak
      </h1>
      <p style="margin: 22px 0 0; max-width: 400px; color: rgba(237, 233, 233, 0.7); font-size: 14px; line-height: 1.6">
        Building clean, user-friendly interfaces with a keen eye for detail — one weightless pixel at a time.
      </p>
      <div style="margin-top: 30px; display: flex; gap: 14px; flex-wrap: wrap">
        <a href="#work" class="mail" style="padding: 12px 20px; font-size: 13px">See the work</a>
        <a href="#contact" class="mail" style="padding: 12px 20px; font-size: 13px">Get in touch</a>
      </div>
    </div>
    <div
      class="chat"
      style="
        position: absolute;
        right: 6%;
        top: 16%;
        z-index: 2;
        width: 230px;
        height: 230px;
        display: none;
        align-items: center;
        justify-content: center;
      "
    >
      <img src="../assets/images/chat.svg" style="width: 100%; animation: floaty 6s ease-in-out infinite" />
      <p style="position: absolute; margin: 0; color: #ede9e9; font-size: 13px; text-align: center; line-height: 1.5">
        Hello Stranger.<br />Welcome to my<br />website.
      </p>
    </div>
    <div
      style="
        position: absolute;
        bottom: 26px;
        left: 50%;
        transform: translateX(-50%);
        z-index: 2;
        font-size: 10px;
        letter-spacing: 0.2em;
        color: rgba(237, 233, 233, 0.5);
        animation: floaty 3s ease-in-out infinite;
      "
    >
      SCROLL ↓
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import * as THREE from "three";

const canvasEl = ref(null);
let cleanup = null;

onMounted(() => {
  const canvas = canvasEl.value;
  if (!canvas) return;
  const hero = canvas.parentElement;

  const renderer = new THREE.WebGLRenderer({ canvas, antialias: true, alpha: true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(55, 1, 0.1, 200);
  camera.position.set(0, 0, 18);

  // starfield
  const starCount = 1400;
  const positions = new Float32Array(starCount * 3);
  for (let i = 0; i < starCount; i++) {
    positions[i * 3] = (Math.random() - 0.5) * 120;
    positions[i * 3 + 1] = (Math.random() - 0.5) * 90;
    positions[i * 3 + 2] = (Math.random() - 0.5) * 90;
  }
  const starGeo = new THREE.BufferGeometry();
  starGeo.setAttribute("position", new THREE.BufferAttribute(positions, 3));
  const stars = new THREE.Points(
    starGeo,
    new THREE.PointsMaterial({ color: 0xede9e9, size: 0.16, sizeAttenuation: true, transparent: true, opacity: 0.85 })
  );
  scene.add(stars);

  // drifting object: nested wireframe polyhedra
  const tilt = new THREE.Group();
  const spin = new THREE.Group();
  tilt.add(spin);
  scene.add(tilt);
  tilt.position.set(4, 0.5, 0);

  const mainGeo = new THREE.IcosahedronGeometry(4.4, 1);
  const main = new THREE.LineSegments(
    new THREE.WireframeGeometry(mainGeo),
    new THREE.LineBasicMaterial({ color: 0xede9e9, transparent: true, opacity: 0.55 })
  );
  spin.add(main);

  const core = new THREE.Mesh(new THREE.IcosahedronGeometry(2.5, 0), new THREE.MeshBasicMaterial({ color: 0x0a0a0a }));
  spin.add(core);
  const coreEdge = new THREE.LineSegments(
    new THREE.WireframeGeometry(new THREE.IcosahedronGeometry(2.5, 0)),
    new THREE.LineBasicMaterial({ color: 0xe8973a, transparent: true, opacity: 0.9 })
  );
  spin.add(coreEdge);

  const ring = new THREE.LineSegments(
    new THREE.WireframeGeometry(new THREE.TorusGeometry(6.6, 0.05, 6, 60)),
    new THREE.LineBasicMaterial({ color: 0xe8973a, transparent: true, opacity: 0.35 })
  );
  ring.rotation.x = Math.PI / 2.4;
  spin.add(ring);

  // orbiting satellite
  const sat = new THREE.LineSegments(
    new THREE.WireframeGeometry(new THREE.OctahedronGeometry(0.9, 0)),
    new THREE.LineBasicMaterial({ color: 0xede9e9, transparent: true, opacity: 0.7 })
  );
  tilt.add(sat);

  const resize = () => {
    const w = hero.clientWidth,
      h = hero.clientHeight;
    renderer.setSize(w, h, false);
    camera.aspect = w / h;
    camera.updateProjectionMatrix();
  };
  resize();
  const ro = new ResizeObserver(resize);
  ro.observe(hero);

  const pointer = { x: 0, y: 0, tx: 0, ty: 0 };
  const onMove = (ev) => {
    const r = hero.getBoundingClientRect();
    pointer.tx = ((ev.clientX - r.left) / r.width - 0.5) * 2;
    pointer.ty = ((ev.clientY - r.top) / r.height - 0.5) * 2;
  };
  const onLeave = () => {
    pointer.tx = 0;
    pointer.ty = 0;
  };
  hero.addEventListener("pointermove", onMove);
  hero.addEventListener("pointerleave", onLeave);

  let raf,
    t = 0;
  const loop = () => {
    t += 0.01;
    pointer.x += (pointer.tx - pointer.x) * 0.05;
    pointer.y += (pointer.ty - pointer.y) * 0.05;
    spin.rotation.y += 0.0016;
    spin.rotation.x += 0.0007;
    tilt.rotation.y = pointer.x * 0.5;
    tilt.rotation.x = pointer.y * 0.4;
    sat.position.set(Math.cos(t * 0.7) * 7.4, Math.sin(t * 0.9) * 3.2, Math.sin(t * 0.7) * 7.4);
    sat.rotation.x += 0.02;
    sat.rotation.y += 0.03;
    stars.rotation.y += 0.0004;
    stars.rotation.x += 0.0001;
    camera.position.x = pointer.x * 1.2;
    camera.position.y = -pointer.y * 0.8;
    camera.lookAt(3, 0, 0);
    renderer.render(scene, camera);
    raf = requestAnimationFrame(loop);
  };
  loop();

  cleanup = () => {
    cancelAnimationFrame(raf);
    ro.disconnect();
    hero.removeEventListener("pointermove", onMove);
    hero.removeEventListener("pointerleave", onLeave);
    renderer.dispose();
  };
});

onUnmounted(() => {
  if (cleanup) cleanup();
});
</script>
