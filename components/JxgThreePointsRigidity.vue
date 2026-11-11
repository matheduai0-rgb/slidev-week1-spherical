<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '260px' },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.45, 1.38, 1.45, -1.38]

// Mode: false = valid (phi(p) == psi(p)), true = RAA assumption (phi(p) != psi(p))
const raaMode = ref(true)
const bisectorAngle = ref(25) // deg of hypothetical G_{phi(p), psi(p)}

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

  board.create('circle', [[0, 0], 1.15], {
    strokeColor: '#58a6ff',
    strokeWidth: 1.8,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Hypothetical equidistant great circle G_{phi(p), psi(p)}
  board.create(
    'curve',
    [
      (t: number) => {
        const a = (bisectorAngle.value * Math.PI) / 180
        const x = 1.15 * Math.cos(t)
        const y = 0.28 * Math.sin(t)
        return x * Math.cos(a) - y * Math.sin(a)
      },
      (t: number) => {
        const a = (bisectorAngle.value * Math.PI) / 180
        const x = 1.15 * Math.cos(t)
        const y = 0.28 * Math.sin(t)
        return x * Math.sin(a) + y * Math.cos(a)
      },
      0,
      2 * Math.PI,
    ],
    {
      strokeColor: '#f87171',
      strokeWidth: 2.4,
      dash: 2,
      visible: () => raaMode.value,
    }
  )

  // Three non-collinear points phi(p1), phi(p2), phi(p3)
  const q1 = board.create('point', [-0.65, -0.25], {
    name: 'φ(p₁)',
    size: 4,
    fillColor: '#38bdf8',
    strokeColor: '#ffffff',
    label: { color: '#38bdf8', offset: [-18, -14], fontSize: 11 },
  })
  const q2 = board.create('point', [0.55, 0.22], {
    name: 'φ(p₂)',
    size: 4,
    fillColor: '#38bdf8',
    strokeColor: '#ffffff',
    label: { color: '#38bdf8', offset: [8, 6], fontSize: 11 },
  })
  const q3 = board.create('point', [-0.25, 0.72], {
    name: 'φ(p₃)',
    size: 4,
    fillColor: '#34d399',
    strokeColor: '#ffffff',
    label: { color: '#34d399', offset: [8, 6], fontSize: 11 },
  })

  // Triangle connecting phi(p1), phi(p2), phi(p3) showing they are non-collinear
  board.create('polygon', [q1, q2, q3], {
    fillColor: '#38bdf8',
    fillOpacity: 0.12,
    borders: { strokeColor: '#38bdf8', strokeWidth: 1.3 },
  })

  // Hypothetical distinct images phi(p) and psi(p)
  board.create(
    'point',
    [
      () => {
        const a = ((bisectorAngle.value + 90) * Math.PI) / 180
        return (raaMode.value ? 0.55 : 0.0) * Math.cos(a)
      },
      () => {
        const a = ((bisectorAngle.value + 90) * Math.PI) / 180
        return (raaMode.value ? 0.55 : 0.0) * Math.sin(a) - 0.15
      },
    ],
    {
      name: () => (raaMode.value ? 'φ(p)' : 'φ(p)=ψ(p)'),
      fixed: true,
      size: 4,
      fillColor: '#fbbf24',
      strokeColor: '#ffffff',
      label: { color: '#fbbf24', offset: [8, 6], fontSize: 11 },
    }
  )

  board.create(
    'point',
    [
      () => {
        const a = ((bisectorAngle.value - 90) * Math.PI) / 180
        return 0.55 * Math.cos(a)
      },
      () => {
        const a = ((bisectorAngle.value - 90) * Math.PI) / 180
        return 0.55 * Math.sin(a) - 0.15
      },
    ],
    {
      name: 'ψ(p)',
      fixed: true,
      size: 4,
      fillColor: '#f87171',
      strokeColor: '#ffffff',
      visible: () => raaMode.value,
      label: { color: '#f87171', offset: [8, -10], fontSize: 11 },
    }
  )
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

watch([raaMode, bisectorAngle], () => {
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
      <button class="jxg-btn" :class="{ active: raaMode }" @click="raaMode = true">
        Hipotesis RAA: φ(p) ≠ ψ(p)
      </button>
      <button class="jxg-btn" :class="{ active: !raaMode }" @click="raaMode = false">
        Kesimpulan: φ(p) = ψ(p)
      </button>
      <label v-if="raaMode">Rotasi G<sub>φ(p),ψ(p)</sub>:
        <input v-model.number="bisectorAngle" type="range" min="-60" max="60" style="width: 65px; vertical-align: middle" />
      </label>
    </div>
    <div class="jxg-readout">
      <span v-if="raaMode" style="color: #f87171">
        <b>Kontradiksi RAA:</b> Garis G<sub>φ(p),ψ(p)</sub> (merah putus-putus) tidak mungkin memuat ketiga titik tak-segaris φ(p₁), φ(p₂), φ(p₃) sekaligus!
      </span>
      <span v-else style="color: #34d399">
        <b>Kekakuan Tiga Titik:</b> Karena φ(pᵢ) = ψ(pᵢ) pada 3 titik tak-segaris, maka wajib φ(p) = ψ(p) di seluruh S².
      </span>
    </div>
  </div>
</template>
