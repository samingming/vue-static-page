<template>
  <div class="page">
    <!-- 별/구름 배경 -->
    <div class="stars"></div>
    <div class="cloud cloud--1"></div>
    <div class="cloud cloud--2"></div>

    <!-- 헤더 -->
    <header class="container header">
      <div class="brand">
        <span class="brand__badge">🌕</span>
        <h1 class="brand__title">한가위 복 많이 받으세요</h1>
      </div>
      <button class="btn ghost" @click="toggleBgm">
        <span v-if="bgmOn">🔊</span>
        <span v-else>🔈</span>
        분위기 사운드 {{ bgmOn ? 'ON' : 'OFF' }}
      </button>
      <audio ref="bgmRef" loop :src="bgmSrc"></audio>
    </header>

    <!-- 메인 -->
    <main class="container hero">
      <section class="moon-wrap">
        <div class="moon">
          <svg viewBox="0 0 120 120" class="rabbit">
            <path
                d="M72 46c6-8 16-16 23-13 6 2 4 10-3 17 7 1 13 6 12 12-1 7-13 12-24 11 2 5 3 10 1 14-3 6-11 9-20 9-9 0-17-3-20-9-4-8 1-18 8-26-9 3-20 1-22-6-1-6 7-11 16-12-6-6-8-13-3-16 7-4 19 6 25 14 1-4 3-8 5-11 5-8 14-10 18-7 5 3 2 11-4 18"
                fill="black" fill-opacity="0.25"
            />
          </svg>
        </div>

        <!-- 연등 -->
        <div class="lanterns">
          <div
              v-for="n in 7"
              :key="n"
              class="lantern"
              :style="{
              left: (n * 13) % 100 + '%',
              animationDelay: n * 1.2 + 's'
            }"
          >
            <div class="lantern__body"></div>
            <div class="lantern__glow"></div>
            <div class="lantern__tail"></div>
          </div>
        </div>
      </section>

      <section class="hero__text">
        <h2 class="headline">
          둥근 달처럼 <span class="accent">행복</span>도 가득 차오르길
        </h2>
        <p class="sub">
          가족과 함께, 마음은 포근하게. 송편처럼 꽉 찬 한가위 되세요.
        </p>
        <div class="cta-row">
          <a class="btn primary" href="#blessings">축원 받기</a>
          <button class="btn outline" @click="toggleNight">
            {{ night ? '새벽 감성 OFF' : '새벽 감성 ON' }}
          </button>
        </div>
      </section>
    </main>

    <!-- 축원 티커 -->
    <section id="blessings" class="ticker">
      <div class="ticker__track">
        <span v-for="(msg, i) in blessings" :key="i" class="ticker__item">
          {{ msg }}
        </span>
      </div>
    </section>

    <!-- 카드 -->
    <section class="container cards">
      <article class="card">
        <h3>🥮 송편 빚는 마음</h3>
        <p>반달을 접어 보름달을 담듯, 작은 정성들이 모여 큰 행복이 됩니다.</p>
      </article>
      <article class="card">
        <h3>🌕 달빛 안부</h3>
        <p>멀리 있어도 달은 우리를 비춥니다. 안부 한 통, 마음 한 줌 전해보세요.</p>
      </article>
      <article class="card">
        <h3>🚗 안전 귀성길</h3>
        <p>잠깐의 휴식이 오래가는 평안을 지켜줍니다. 안전운전 하세요!</p>
      </article>
    </section>

    <!-- 푸터 -->
    <footer class="container footer">
      <small>© {{ new Date().getFullYear() }} Hangawi · Vue + Vite</small>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

const night = ref(true)
const bgmOn = ref(false)
const bgmRef = ref<HTMLAudioElement | null>(null)
const bgmSrc =
    'data:audio/mp3;base64,//uQZAAAAAAAAAAAAAAAAAAAA...' // 무음 placeholder

const blessings = [
  '한가위만 같아라 🌕',
  '보름달 같은 평안이 가득하길 ✨',
  '가족과 함께 따뜻한 시간 보내세요 🧡',
  '마음 풍성, 건강 가득 🙏',
  '맛있는 송편처럼 달콤한 하루 되세요 🥮'
]

function toggleNight() {
  night.value = !night.value
  document.documentElement.dataset.theme = night.value ? 'night' : 'dawn'
}

function toggleBgm() {
  bgmOn.value = !bgmOn.value
  const el = bgmRef.value
  if (!el) return
  if (bgmOn.value) el.play().catch(() => (bgmOn.value = false))
  else el.pause()
}

onMounted(() => {
  document.documentElement.dataset.theme = night.value ? 'night' : 'dawn'
})
</script>

<style scoped>
/* 색상 변수 */
:root {
  --bg-top: #0a0f2a;
  --bg-bottom: #1a1640;
  --moon: #ffe9a9;
  --moon-core: #fff3c9;
  --accent: #ffd166;
  --text: #eaeaf7;
  --muted: #c3c3da;
  --card: #151333cc;
  --border: #2b2854;
  --glow: rgba(255, 219, 112, 0.6);
}
:root[data-theme='dawn'] {
  --bg-top: #2a2961;
  --bg-bottom: #5a4f9c;
  --accent: #ffcf6e;
}

body, html, #app, .page {
  margin: 0; height: 100%;
  font-family: 'Noto Sans KR', sans-serif;
  background: linear-gradient(180deg, var(--bg-top), var(--bg-bottom));
  color: var(--text);
  overflow-x: hidden;
}

/* 헤더 */
.header { display: flex; justify-content: space-between; align-items: center; padding: 20px 0; }
.brand { display: flex; align-items: center; gap: 10px; }
.brand__badge { font-size: 24px; }
.brand__title { margin: 0; font-weight: 800; }

/* 달 */
.moon-wrap { position: relative; height: 400px; }
.moon {
  position: absolute; inset: 0; margin: auto;
  width: 280px; height: 280px; border-radius: 50%;
  background: radial-gradient(35% 35% at 40% 35%, var(--moon-core), var(--moon));
  box-shadow: 0 0 40px 10px var(--glow);
  animation: float 6s ease-in-out infinite;
}
.rabbit { width: 60%; position: absolute; inset: 0; margin: auto; }

/* 연등 */
.lanterns { position: absolute; inset: 0; }
.lantern {
  position: absolute; bottom: -120px; transform: translateX(-50%);
  animation: rise 16s linear infinite;
}
.lantern__body {
  width: 20px; height: 28px; border-radius: 6px 6px 8px 8px;
  background: linear-gradient(#ff9b4a, #ff7b2a);
  box-shadow: 0 8px 20px rgba(255, 140, 60, .6);
}
.lantern__glow {
  position: absolute; inset: -20px -18px;
  border-radius: 50%;
  background: radial-gradient(closest-side, rgba(255,196,98,.55), transparent 70%);
  filter: blur(10px);
}
.lantern__tail {
  position: absolute; top: 28px; left: 50%;
  width: 2px; height: 14px;
  background: #ffb66b; border-radius: 2px;
  transform: translateX(-50%);
}

/* 배경 효과 */
.stars {
  position: absolute; inset: 0;
  background: radial-gradient(2px 2px at 20% 30%, #fff, transparent 40%),
  radial-gradient(2px 2px at 70% 20%, #fff, transparent 40%),
  radial-gradient(1.5px 1.5px at 40% 70%, #fff, transparent 40%);
  animation: twinkle 3s ease-in-out infinite alternate;
}
.cloud {
  position: absolute; left: -20%; right: -20%; height: 120px;
  background: radial-gradient(ellipse at 30% 50%, #ffffff18, transparent 60%),
  radial-gradient(ellipse at 70% 45%, #ffffff12, transparent 62%);
  filter: blur(2px);
}
.cloud--1 { top: 12vh; animation: drift 60s linear infinite; }
.cloud--2 { top: 28vh; animation: drift 80s linear reverse infinite; }

/* 텍스트 */
.headline { font-size: clamp(28px, 4.2vw, 44px); }
.accent { color: var(--accent); }
.sub { color: var(--muted); }

/* 버튼 */
.btn { border: 1px solid var(--border); border-radius: 999px; padding: 8px 16px; font-weight: 700; cursor: pointer; background: transparent; color: var(--text); }
.btn.primary { background: linear-gradient(90deg, #ffb86b, #ffd166); color: #2a1b00; border: none; }
.btn.ghost { background: #ffffff14; }
.btn:hover { transform: translateY(-1px); }

/* 카드 */
.cards { display: grid; gap: 16px; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); padding: 20px 0; }
.card { background: var(--card); border: 1px solid var(--border); border-radius: 16px; padding: 20px; }

/* 티커 */
.ticker { overflow: hidden; border-block: 1px solid var(--border); background: #ffffff08; }
.ticker__track { display: flex; gap: 24px; animation: marquee 25s linear infinite; padding: 10px; }
.ticker__item { color: var(--muted); white-space: nowrap; }

/* 푸터 */
.footer { text-align: center; padding: 30px 0 40px; color: #ccc; }

/* 애니메이션 */
@keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }
@keyframes rise { 0%{opacity:0;transform:translate(-50%,0)} 10%{opacity:1} 100%{transform:translate(-50%,-120vh);opacity:0} }
@keyframes drift { 0%{transform:translateX(0)} 100%{transform:translateX(20%)} }
@keyframes twinkle { from{opacity:.7} to{opacity:1} }
@keyframes marquee { 0%{transform:translateX(0)} 100%{transform:translateX(-50%)} }
</style>
