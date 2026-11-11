<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  initialCase: { type: Number, default: 2 },
  height: { type: String, default: '260px' },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.45, 1.38, 1.45, -1.38]

const activeCase = ref(props.initialCase) // 1, 2, 3, or 4
const stepApplied = ref(false)

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

  board.create('circle', [[0, 0], 1.12], {
    strokeColor: '#58a6ff',
    strokeWidth: 1.8,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Helper to draw a complete 360-degree projected great circle tilted by angleDeg
  function createGreatCircle(
    angleDeg: number,
    color: string,
    frontDash: number,
    visFn: () => boolean
  ) {
    const a = (angleDeg * Math.PI) / 180
    // Back half [0, pi]
    board.create(
      'curve',
      [
        (t: number) => {
          const x = 1.12 * Math.cos(t)
          const y = 0.24 * Math.sin(t)
          return x * Math.cos(a) - y * Math.sin(a)
        },
        (t: number) => {
          const x = 1.12 * Math.cos(t)
          const y = 0.24 * Math.sin(t)
          return x * Math.sin(a) + y * Math.cos(a)
        },
        0,
        Math.PI,
      ],
      {
        strokeColor: color,
        strokeWidth: 1.4,
        strokeOpacity: 0.65,
        dash: 2,
        visible: visFn,
      }
    )
    // Front half [pi, 2*pi]
    board.create(
      'curve',
      [
        (t: number) => {
          const x = 1.12 * Math.cos(t)
          const y = 0.24 * Math.sin(t)
          return x * Math.cos(a) - y * Math.sin(a)
        },
        (t: number) => {
          const x = 1.12 * Math.cos(t)
          const y = 0.24 * Math.sin(t)
          return x * Math.sin(a) + y * Math.cos(a)
        },
        Math.PI,
        2 * Math.PI,
      ],
      {
        strokeColor: color,
        strokeWidth: 2.4,
        dash: frontDash,
        visible: visFn,
      }
    )
  }

  // First mirror great circle G = G_{p3, phi(p3)} (horizontal equator)
  createGreatCircle(0, '#34d399', 0, () => activeCase.value >= 2)

  // Second mirror great circle H = G_{p2, phi(p2)}
  createGreatCircle(24, '#fbbf24', 2, () => activeCase.value >= 3)

  // Third mirror great circle N = G_{p1, phi(p1)} for Case 4
  createGreatCircle(-32, '#f87171', 2, () => activeCase.value === 4)

  // Fixed / reference points p1, p2, p3
  const p1 = board.create('point', [-0.58, -0.14], {
    name: () => (activeCase.value <= 3 ? 'p₁ = φ(p₁)' : 'p₁'),
    fixed: true,
    size: 4,
    fillColor: '#38bdf8',
    strokeColor: '#ffffff',
    label: { color: '#38bdf8', offset: [-28, 12], fontSize: 11 },
  })

  const p2 = board.create('point', [0.38, -0.18], {
    name: () => (activeCase.value <= 2 ? 'p₂ = φ(p₂)' : 'p₂'),
    fixed: true,
    size: 4,
    fillColor: '#38bdf8',
    strokeColor: '#ffffff',
    label: { color: '#38bdf8', offset: [6, 10], fontSize: 11 },
  })

  const p3 = board.create('point', [0.22, 0.76], {
    name: 'p₃',
    size: 4.5,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [8, 6], fontSize: 12 },
  })

  // Image phi(p3) (or after reflection r_G(phi(p3)) = p3)
  const phiP3 = board.create(
    'point',
    [
      () => p3.X(),
      () => (activeCase.value === 1 || stepApplied.value ? p3.Y() : -p3.Y()),
    ],
    {
      name: () => (stepApplied.value ? 'r̄_G(φ(p₃)) = p₃' : 'φ(p₃)'),
      fixed: true,
      size: 4.5,
      fillColor: '#f87171',
      strokeColor: '#ffffff',
      label: { color: '#f87171', offset: [8, -10], fontSize: 11 },
    }
  )

  // Equal distances from p1 and p2 to p3 and phi(p3)
  board.create('segment', [p1, p3], { strokeColor: '#8b949e', strokeWidth: 1.2 })
  board.create('segment', [p1, phiP3], { strokeColor: '#8b949e', strokeWidth: 1.2 })
  board.create('segment', [p2, p3], { strokeColor: '#8b949e', strokeWidth: 1.2 })
  board.create('segment', [p2, phiP3], { strokeColor: '#8b949e', strokeWidth: 1.2 })
  board.create('segment', [p3, phiP3], {
    strokeColor: '#e6edf3',
    strokeWidth: 1.5,
    dash: 2,
  })
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

watch([activeCase, stepApplied], () => {
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
      <button class="jxg-btn" :class="{ active: activeCase === 1 }" @click="activeCase = 1; stepApplied = false">
        Kasus 1 (3 tetap)
      </button>
      <button class="jxg-btn" :class="{ active: activeCase === 2 }" @click="activeCase = 2; stepApplied = false">
        Kasus 2 (2 tetap)
      </button>
      <button class="jxg-btn" :class="{ active: activeCase === 3 }" @click="activeCase = 3; stepApplied = false">
        Kasus 3 (1 tetap)
      </button>
      <button class="jxg-btn" :class="{ active: activeCase === 4 }" @click="activeCase = 4; stepApplied = false">
        Kasus 4 (0 tetap)
      </button>
      <button v-if="activeCase >= 2" class="jxg-btn" :class="{ active: stepApplied }" @click="stepApplied = !stepApplied">
        {{ stepApplied ? 'Reset φ(p₃)' : 'Terapkan Cermin r̄_G' }}
      </button>
    </div>
    <div class="jxg-readout">
      <span v-if="activeCase === 1" style="color: #38bdf8">
        <b>Kasus 1:</b> φ(pᵢ) = pᵢ (i=1,2,3) ⇒ φ = id<sub>S²</sub> (0 refleksi, Teorema 2.5).
      </span>
      <span v-else-if="activeCase === 2" style="color: #34d399">
        <b>Kasus 2:</b> p₁, p₂ ∈ G<sub>p₃,φ(p₃)</sub> ⇒ r̄<sub>G</sub> ∘ φ menetapkan p₁, p₂, p₃ ⇒ φ = r̄<sub>G</sub> (1 refleksi).
      </span>
      <span v-else-if="activeCase === 3" style="color: #fbbf24">
        <b>Kasus 3:</b> φ' = r̄<sub>H</sub> ∘ φ menetapkan p₁ & p₂ (H = G<sub>p₂,φ(p₂)</sub>) ⇒ reduksi ke Kasus 1/2 (≤ 2 refleksi).
      </span>
      <span v-else style="color: #f87171">
        <b>Kasus 4:</b> φ'' = r̄<sub>N</sub> ∘ φ menetapkan p₁ (N = G<sub>p₁,φ(p₁)</sub>) ⇒ reduksi ke Kasus 1/2/3 (≤ 3 refleksi).
      </span>
    </div>
  </div>
</template>
