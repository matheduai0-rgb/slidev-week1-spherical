<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '265px' },
  showSmallCircle: { type: Boolean, default: false },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.45, 1.38, 1.45, -1.38]

const paramT = ref(35) // angle of test point p along G_{p1,p2} in degrees
const camElev = ref(22) // camera elevation in degrees
const showSmall = ref(props.showSmallCircle)

const distS2P1 = ref('1.571')
const distChordP1 = ref('1.414')

function proj3D(x: number, y: number, z: number) {
  const az = (-18 * Math.PI) / 180
  const el = (camElev.value * Math.PI) / 180
  const ca = Math.cos(az)
  const sa = Math.sin(az)
  const ce = Math.cos(el)
  const se = Math.sin(el)
  const X = x * ca - y * sa
  const Y = (x * sa + y * ca) * se + z * ce
  return [X, Y]
}

function setupBoard() {
  if (!boardEl.value || board) return
  const w = boardEl.value.clientWidth
  const h = boardEl.value.clientHeight
  if (w <= 0 || h <= 0) return

  board = JXG.JSXGraph.initBoard(boardEl.value, {
    boundingbox: BBOX,
    axis: false,
    grid: false,
    showNavigation: false,
    showCopyright: false,
    keepaspectratio: true,
    resize: { enabled: false },
    pan: { enabled: false },
    zoom: { enabled: false },
  })

  const O = board.create('point', [0, 0], {
    name: '0',
    fixed: true,
    size: 2.5,
    fillColor: '#e6edf3',
    strokeColor: '#e6edf3',
    label: { color: '#e6edf3', offset: [-12, -2], fontSize: 11 },
  })

  // Sphere silhouette
  board.create('circle', [O, 1], {
    strokeColor: '#58a6ff',
    strokeWidth: 2,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Back half of Great Circle G_{p1,p2} (z = 0)
  board.create(
    'curve',
    [
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[0],
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[1],
      0,
      Math.PI,
    ],
    { strokeColor: '#8b949e', strokeWidth: 1.4, dash: 2 }
  )

  // Front half of Great Circle G_{p1,p2} (z = 0)
  board.create(
    'curve',
    [
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[0],
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[1],
      Math.PI,
      2 * Math.PI,
    ],
    { strokeColor: '#38bdf8', strokeWidth: 2.8 }
  )

  // Optional Small Circle at z = 0.58
  board.create(
    'curve',
    [
      (t: number) => {
        const r = Math.sqrt(1 - 0.58 * 0.58)
        return proj3D(r * Math.cos(t), r * Math.sin(t), 0.58)[0]
      },
      (t: number) => {
        const r = Math.sqrt(1 - 0.58 * 0.58)
        return proj3D(r * Math.cos(t), r * Math.sin(t), 0.58)[1]
      },
      Math.PI,
      2 * Math.PI,
    ],
    {
      strokeColor: '#f87171',
      strokeWidth: 2,
      visible: () => showSmall.value,
    }
  )
  board.create(
    'curve',
    [
      (t: number) => {
        const r = Math.sqrt(1 - 0.58 * 0.58)
        return proj3D(r * Math.cos(t), r * Math.sin(t), 0.58)[0]
      },
      (t: number) => {
        const r = Math.sqrt(1 - 0.58 * 0.58)
        return proj3D(r * Math.cos(t), r * Math.sin(t), 0.58)[1]
      },
      0,
      Math.PI,
    ],
    {
      strokeColor: '#f87171',
      strokeWidth: 1.2,
      dash: 2,
      visible: () => showSmall.value,
    }
  )

  // Poles p1 = (0, 0, 1) and p2 = (0, 0, -1)
  const P1 = board.create(
    'point',
    [() => proj3D(0, 0, 1)[0], () => proj3D(0, 0, 1)[1]],
    {
      name: 'p₁',
      fixed: true,
      size: 4,
      fillColor: '#fbbf24',
      strokeColor: '#ffffff',
      label: { color: '#fbbf24', offset: [6, 6], fontSize: 12 },
    }
  )

  const P2 = board.create(
    'point',
    [() => proj3D(0, 0, -1)[0], () => proj3D(0, 0, -1)[1]],
    {
      name: 'p₂',
      fixed: true,
      size: 4,
      fillColor: '#fbbf24',
      strokeColor: '#ffffff',
      label: { color: '#fbbf24', offset: [6, -10], fontSize: 12 },
    }
  )

  // Moving point p on Great Circle G_{p1,p2}
  const P = board.create(
    'point',
    [
      () => {
        const rad = ((paramT.value - 90) * Math.PI) / 180
        return proj3D(Math.cos(rad), Math.sin(rad), 0)[0]
      },
      () => {
        const rad = ((paramT.value - 90) * Math.PI) / 180
        return proj3D(Math.cos(rad), Math.sin(rad), 0)[1]
      },
    ],
    {
      name: 'p ∈ G_{p₁,p₂}',
      fixed: true,
      size: 4.5,
      fillColor: '#34d399',
      strokeColor: '#ffffff',
      label: { color: '#34d399', offset: [8, -8], fontSize: 11 },
    }
  )

  // Equal chords p-p1 and p-p2
  board.create('segment', [P, P1], { strokeColor: '#34d399', strokeWidth: 1.7, dash: 2 })
  board.create('segment', [P, P2], { strokeColor: '#34d399', strokeWidth: 1.7, dash: 2 })

  // Normal vector u = (p1 - p2)/|p1 - p2|
  board.create('arrow', [O, P1], { strokeColor: '#60a5fa', strokeWidth: 1.8 })
}

onMounted(() => {
  if (!boardEl.value) return
  setupBoard()
  resizeObs = new ResizeObserver((entries) => {
    const rect = entries[entries.length - 1]?.contentRect
    if (!rect || rect.width <= 0 || rect.height <= 0) return
    if (!board) {
      setupBoard()
    } else {
      board.resizeContainer(rect.width, rect.height, true, true)
      board.setBoundingBox(BBOX, true)
      board.fullUpdate()
    }
  })
  resizeObs.observe(boardEl.value)
})

watch([paramT, camElev, showSmall], () => {
  if (board) board.update()
})

onBeforeUnmount(() => {
  if (resizeObs) resizeObs.disconnect()
  if (board) JXG.JSXGraph.freeBoard(board)
})
</script>

<template>
  <div class="jxg-card">
    <div :id="boxId" ref="boardEl" class="jxgbox" :style="{ width: '100%', height: props.height }"></div>
    <div class="jxg-controls">
      <label>Posisi <b style="color: #34d399">p ∈ G<sub>p₁,p₂</sub></b>:
        <input v-model.number="paramT" type="range" min="-180" max="180" style="width: 75px; vertical-align: middle" />
      </label>
      <label>Kemiringan:
        <input v-model.number="camElev" type="range" min="5" max="55" style="width: 65px; vertical-align: middle" />
      </label>
      <button class="jxg-btn" :class="{ active: showSmall }" @click="showSmall = !showSmall">
        Lingkaran Kecil
      </button>
    </div>
    <div class="jxg-readout">
      <span style="color: #34d399">⟨p, p₁ − p₂⟩ = 0</span> &nbsp;⟺&nbsp;
      <span>d<sub>S²</sub>(p,p₁) = d<sub>S²</sub>(p,p₂) = {{ distS2P1 }} rad</span>
      &nbsp;(|p−p₁| = |p−p₂| = {{ distChordP1 }})
    </div>
  </div>
</template>
