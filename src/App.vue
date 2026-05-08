<script setup lang="ts">
import { ref } from 'vue'
const video = ref(null);
const currentUrl = ref<string | null>(null)

const selectVideoFile = (event: Event) => {
  const input = event.target as HTMLInputElement;
  const file = input.files?.[0];
  if (!file) return;

  if (currentUrl.value) {
    URL.revokeObjectURL(currentUrl.value);
  }
  const url = URL.createObjectURL(file);
  currentUrl.value = url;
  video.value.src = url;

}

</script>

<template>
  <div class="wrap" role="application">
    <header>
      <h1>Генератор превью</h1>
      <div class="controls">
        <label class="file" title="Выберите видео файл" @change="selectVideoFile">
          <input type="file" accept="video/*" />
        </label>
        <button class="btn secondary" disabled>Предпросмотр</button>
        <button class="btn" disabled>Скачать превью</button>
      </div>
    </header>

    <div class="grid">
      <section class="panel" aria-labelledby="playerLabel">
        <header><h2>Проигрыватель</h2></header>
        <div class="body">
          <video controls playsinline ref="video"></video>
        </div>
      </section>

      <aside class="panel" aria-labelledby="previewLabel">
        <header><h2>Превью</h2></header>
        <div class="body">
          <canvas ref="preview"></canvas>
        </div>
      </aside>
    </div>
  </div>
</template>

<style>
:root {
  --bg: #0f172a;
  --card: #111827;
  --muted: #94a3b8;
  --text: #e5e7eb; /* cyan-400 */
  --accent-2: #60a5fa;
  --border: #1f2937;
  --danger: #ef4444;
}
html,
body {
  height: 100%;
}
body {
  margin: 0;
  font-family:
    ui-sans-serif,
    system-ui,
    -apple-system,
    Segoe UI,
    Roboto,
    Ubuntu,
    Cantarell,
    Noto Sans,
    'Helvetica Neue',
    Arial,
    'Apple Color Emoji',
    'Segoe UI Emoji';
  color: var(--text);
  background:
    radial-gradient(1200px 700px at 20% -10%, rgba(34, 211, 238, 0.12), transparent 60%),
    radial-gradient(1000px 600px at 120% 10%, rgba(96, 165, 250, 0.12), transparent 60%), var(--bg);
  display: grid;
  place-items: center;
  padding: 24px;
  box-sizing: border-box;
}
.wrap {
  width: min(1100px, 100%);
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.02), rgba(255, 255, 255, 0.01));
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 20px 20px 28px;
  box-shadow:
    0 10px 30px rgba(0, 0, 0, 0.35),
    inset 0 1px 0 rgba(255, 255, 255, 0.06);
  backdrop-filter: blur(6px);
}
header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  margin-bottom: 14px;
}
h1 {
  font-size: 20px;
  margin: 0;
  letter-spacing: 0.2px;
}
.controls {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
}
.file {
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 10px 12px;
  border: 1px dashed var(--border);
  border-radius: 10px;
  background: rgba(17, 24, 39, 0.4);
}
.file input[type='file'] {
  appearance: none;
  border: none;
  color: var(--muted);
  background: transparent;
}

.btn {
  appearance: none;
  border: 1px solid var(--border);
  color: #08121a;
  font-weight: 700;
  padding: 10px 14px;
  border-radius: 10px;
  cursor: pointer;
  transition:
    transform 0.08s ease,
    filter 0.2s ease,
    opacity 0.2s ease;
  text-shadow: 0 1px 0 rgba(255, 255, 255, 0.2);
}
.btn:hover {
  filter: brightness(1.05);
}
.btn:active {
  transform: translateY(1px);
}
.btn[disabled] {
  opacity: 0.5;
  cursor: not-allowed;
}
.btn.secondary {
  color: #0b1220;
}
.grid {
  display: grid;
  grid-template-columns: 1fr 380px;
  gap: 16px;
  margin-top: 12px;
}
@media (max-width: 980px) {
  .grid {
    grid-template-columns: 1fr;
  }
}

.panel {
  border: 1px solid var(--border);
  border-radius: 12px;
  overflow: clip;
  background: rgba(2, 6, 23, 0.4);
}
.panel header {
  padding: 10px 12px;
  border-bottom: 1px solid var(--border);
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.03), rgba(255, 255, 255, 0));
}
.panel header h2 {
  font-size: 14px;
  margin: 0;
  color: var(--muted);
  font-weight: 600;
}
.panel .body {
  padding: 12px;
}

video,
canvas {
  width: 100%;
  height: auto;
  display: block;
  background: #000;
}
.meta {
  color: var(--muted);
  font-size: 12px;
  margin-top: 6px;
}
.error {
  color: var(--danger);
  font-weight: 600;
}
.stack {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  align-items: center;
}
.stack {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  align-items: center;
}
</style>
