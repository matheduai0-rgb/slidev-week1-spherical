<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '250px' },
  showFourthLine: { type: Boolean, default: true },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.42, 1.38, 1.42, -1.38]

const aDeg = ref(55) // angle of meridian alpha on G
const bDeg = ref(15) // angle of meridian beta on G
const cDeg = ref(80) // angle of 3rd concurrent meridian gamma on G
const showDelta = ref(props.showFourthLine)

// Angle of 4th concurrent meridian delta so that r_alpha r_beta = r_gamma r_delta => a - b = c - d => d = c - (a - b)
const dDeg = computed(() => cDeg.value - (aDeg.value - bDeg.value))
const rotAngleDeg = computed(() => 2 * (aDeg.value - bDeg.value))

function proj3D(x: number, y: number, z: number) {
  const az = (-15 * Math.PI) / 180
  const el = (24 * Math.PI) / 180
  const X = x * Math.cos(az) - y * Math.sin(az)
  const Y = (x * Math.sin(az) + y * Math.cos(az)) * Math.sin(el) + z * Math.cos(el)
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
    name: '',
    fixed: true,
    size: 2,
    fillColor: '#e6edf3',
    strokeColor: '#e6edf3',
  })

  board.create('circle', [O, 1], {
    strokeColor: '#58a6ff',
    strokeWidth: 1.8,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Equator G = span{P, Q} \cap S^2 (back half [0, pi] + front half [pi, 2pi] in matching cyan)
  board.create(
    'curve',
    [
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[0],
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[1],
      0,
      Math.PI,
    ],
    { strokeColor: '#38bdf8', strokeWidth: 1.5, strokeOpacity: 0.65, dash: 2 }
  )
  board.create(
    'curve',
    [
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[0],
      (t: number) => proj3D(Math.cos(t), Math.sin(t), 0)[1],
      Math.PI,
      2 * Math.PI,
    ],
    { strokeColor: '#38bdf8', strokeWidth: 2.3 }
  )

  // North pole +eta = (0, 0, 1) and antipodal south pole -eta = (0, 0, -1)
  const Eta = board.create('point', [() => proj3D(0, 0, 1)[0], () => proj3D(0, 0, 1)[1]], {
    name: 'η',
    fixed: true,
    size: 4,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [6, 6], fontSize: 12 },
  })
  board.create('point', [() => proj3D(0, 0, -1)[0], () => proj3D(0, 0, -1)[1]], {
    name: '−η',
    fixed: true,
    size: 3,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [6, -10], fontSize: 10 },
  })
  board.create('arrow', [O, Eta], { strokeColor: '#fbbf24', strokeWidth: 1.8 })

  // Helper to draw a COMPLETE 360-degree meridian great circle passing through ±eta and ±u(deg)
  function createMeridian(
    getDeg: () => number,
    color: string,
    frontDash = 0,
    visFn?: () => boolean
  ) {
    // Back half (t in [pi, 2*pi])
    board.create(
      'curve',
      [
        (t: number) => {
          const rad = ((getDeg() - 90) * Math.PI) / 180
          return proj3D(Math.sin(t) * Math.cos(rad), Math.sin(t) * Math.sin(rad), Math.cos(t))[0]
        },
        (t: number) => {
          const rad = ((getDeg() - 90) * Math.PI) / 180
          return proj3D(Math.sin(t) * Math.cos(rad), Math.sin(t) * Math.sin(rad), Math.cos(t))[1]
        },
        Math.PI,
        2 * Math.PI,
      ],
      {
        strokeColor: color,
        strokeWidth: 1.5,
        strokeOpacity: 0.65,
        dash: 2,
        visible: visFn ? visFn : true,
      }
    )

    // Front half (t in [0, pi])
    return board.create(
      'curve',
      [
        (t: number) => {
          const rad = ((getDeg() - 90) * Math.PI) / 180
          return proj3D(Math.sin(t) * Math.cos(rad), Math.sin(t) * Math.sin(rad), Math.cos(t))[0]
        },
        (t: number) => {
          const rad = ((getDeg() - 90) * Math.PI) / 180
          return proj3D(Math.sin(t) * Math.cos(rad), Math.sin(t) * Math.sin(rad), Math.cos(t))[1]
        },
        0,
        Math.PI,
      ],
      {
        strokeColor: color,
        strokeWidth: 2.3,
        dash: frontDash,
        visible: visFn ? visFn : true,
      }
    )
  }

  createMeridian(() => aDeg.value, '#34d399', 0) // complete great circle alpha
  createMeridian(() => bDeg.value, '#f87171', 0) // complete great circle beta

  // Optional 3rd and 4th complete great circles gamma and delta
  createMeridian(() => cDeg.value, '#60a5fa', 2, () => showDelta.value)
  createMeridian(() => dDeg.value, '#fbbf24', 2, () => showDelta.value)

  // Points u(a) and u(b) on equator
  board.create(
    'point',
    [
      () => proj3D(Math.cos(((aDeg.value - 90) * Math.PI) / 180), Math.sin(((aDeg.value - 90) * Math.PI) / 180), 0)[0],
      () => proj3D(Math.cos(((aDeg.value - 90) * Math.PI) / 180), Math.sin(((aDeg.value - 90) * Math.PI) / 180), 0)[1],
    ],
    {
      name: 'u(a)∈α',
      fixed: true,
      size: 3.5,
      fillColor: '#34d399',
      strokeColor: '#ffffff',
      label: { color: '#34d399', offset: [-26, -12], fontSize: 10 },
    }
  )

  board.create(
    'point',
    [
      () => proj3D(Math.cos(((bDeg.value - 90) * Math.PI) / 180), Math.sin(((bDeg.value - 90) * Math.PI) / 180), 0)[0],
      () => proj3D(Math.cos(((bDeg.value - 90) * Math.PI) / 180), Math.sin(((bDeg.value - 90) * Math.PI) / 180), 0)[1],
    ],
    {
      name: 'u(b)∈β',
      fixed: true,
      size: 3.5,
      fillColor: '#f87171',
      strokeColor: '#ffffff',
      label: { color: '#f87171', offset: [6, -12], fontSize: 10 },
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

watch([aDeg, bDeg, cDeg, showDelta], () => {
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
      <label style="color: #34d399">a (α): <input v-model.number="aDeg" type="range" min="-80" max="80" style="width: 55px; vertical-align: middle" /> {{ aDeg }}°</label>
      <label style="color: #f87171">b (β): <input v-model.number="bDeg" type="range" min="-80" max="80" style="width: 55px; vertical-align: middle" /> {{ bDeg }}°</label>
      <button class="jxg-btn" :class="{ active: showDelta }" @click="showDelta = !showDelta">
        Garis Ke-4 δ
      </button>
      <label v-if="showDelta" style="color: #60a5fa">c (γ): <input v-model.number="cDeg" type="range" min="-80" max="80" style="width: 50px; vertical-align: middle" /></label>
    </div>
    <div class="jxg-readout">
      <div>
        <span style="color: #38bdf8">r̄<sub>α</sub> r̄<sub>β</sub> = diag(rot({{ rotAngleDeg }}°), 1)</span>
        &nbsp;(Rotasi kutub ±η sebesar 2(a−b) = {{ rotAngleDeg }}°)
      </div>
      <div v-if="showDelta" style="color: #fbbf24; margin-top: 2px">
        Garis ke-4 δ pada sudut d = c − (a − b) = {{ dDeg }}° ⇒ r̄<sub>α</sub>∘r̄<sub>β</sub> = r̄<sub>γ</sub>∘r̄<sub>δ</sub> (2(c−d) = {{ rotAngleDeg }}°)
      </div>
    </div>
  </div>
</template>
