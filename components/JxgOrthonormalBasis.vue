<script setup lang="ts">
import { ref, computed, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '245px' },
  presetExample: { type: Boolean, default: false },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.42, 1.35, 1.42, -1.35]

// Spherical angles of normal u3: theta (polar from z-axis in deg), phi (azimuth in deg)
const thetaDeg = ref(props.presetExample ? 135 : 25)
const phiDeg = ref(props.presetExample ? 45 : 30)
const alphaDeg = ref(0) // rotation of u1, u2 inside plane P = u3^perp

function setWorkedExample() {
  // For P: x + y - sqrt(2)z = 0, u3 = (1/2, 1/2, -sqrt(2)/2) -> theta = 135 deg, phi = 45 deg
  thetaDeg.value = 135
  phiDeg.value = 45
  alphaDeg.value = 0
}

function setStandardEquatorial() {
  thetaDeg.value = 0
  phiDeg.value = 0
  alphaDeg.value = 0
}

const triad = computed(() => {
  const th = (thetaDeg.value * Math.PI) / 180
  const ph = (phiDeg.value * Math.PI) / 180
  const al = (alphaDeg.value * Math.PI) / 180

  const u3 = [Math.sin(th) * Math.cos(ph), Math.sin(th) * Math.sin(ph), Math.cos(th)]
  // Base tangent vectors e1, e2 orthogonal to u3
  const e1 = [Math.sin(ph), -Math.cos(ph), 0]
  // u3 x e1
  const e2 = [
    u3[1] * e1[2] - u3[2] * e1[1],
    u3[2] * e1[0] - u3[0] * e1[2],
    u3[0] * e1[1] - u3[1] * e1[0],
  ]
  const ca = Math.cos(al)
  const sa = Math.sin(al)
  const u1 = [ca * e1[0] + sa * e2[0], ca * e1[1] + sa * e2[1], ca * e1[2] + sa * e2[2]]
  const u2 = [-sa * e1[0] + ca * e2[0], -sa * e1[1] + ca * e2[1], -sa * e1[2] + ca * e2[2]]
  return { u1, u2, u3 }
})

const householderDiag = computed(() => {
  const { u3 } = triad.value
  const h11 = (1 - 2 * u3[0] * u3[0]).toFixed(2)
  const h22 = (1 - 2 * u3[1] * u3[1]).toFixed(2)
  const h33 = (1 - 2 * u3[2] * u3[2]).toFixed(2)
  return `diag=[${h11}, ${h22}, ${h33}], tr=1.00, det=-1.00`
})

function proj3D(v: number[]) {
  const az = (-22 * Math.PI) / 180
  const el = (24 * Math.PI) / 180
  const X = v[0] * Math.cos(az) - v[1] * Math.sin(az)
  const Y = (v[0] * Math.sin(az) + v[1] * Math.cos(az)) * Math.sin(el) + v[2] * Math.cos(el)
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
    label: { color: '#e6edf3', offset: [-12, -8], fontSize: 11 },
  })

  board.create('circle', [O, 1], {
    strokeColor: '#58a6ff',
    strokeWidth: 1.8,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Great circle G = span{u1, u2} \cap S^2
  board.create(
    'curve',
    [
      (t: number) => {
        const { u1, u2 } = triad.value
        const v = [
          Math.cos(t) * u1[0] + Math.sin(t) * u2[0],
          Math.cos(t) * u1[1] + Math.sin(t) * u2[1],
          Math.cos(t) * u1[2] + Math.sin(t) * u2[2],
        ]
        return proj3D(v)[0]
      },
      (t: number) => {
        const { u1, u2 } = triad.value
        const v = [
          Math.cos(t) * u1[0] + Math.sin(t) * u2[0],
          Math.cos(t) * u1[1] + Math.sin(t) * u2[1],
          Math.cos(t) * u1[2] + Math.sin(t) * u2[2],
        ]
        return proj3D(v)[1]
      },
      0,
      2 * Math.PI,
    ],
    { strokeColor: '#34d399', strokeWidth: 2.2 }
  )

  // Orthonormal vectors u1, u2, u3
  const U1 = board.create(
    'point',
    [() => proj3D(triad.value.u1)[0], () => proj3D(triad.value.u1)[1]],
    {
      name: 'u₁∈P',
      fixed: true,
      size: 3.5,
      fillColor: '#38bdf8',
      strokeColor: '#ffffff',
      label: { color: '#38bdf8', offset: [6, 4], fontSize: 11 },
    }
  )
  const U2 = board.create(
    'point',
    [() => proj3D(triad.value.u2)[0], () => proj3D(triad.value.u2)[1]],
    {
      name: 'u₂∈P',
      fixed: true,
      size: 3.5,
      fillColor: '#38bdf8',
      strokeColor: '#ffffff',
      label: { color: '#38bdf8', offset: [6, -8], fontSize: 11 },
    }
  )
  const U3 = board.create(
    'point',
    [() => proj3D(triad.value.u3)[0], () => proj3D(triad.value.u3)[1]],
    {
      name: 'u₃⊥P',
      fixed: true,
      size: 4,
      fillColor: '#f87171',
      strokeColor: '#ffffff',
      label: { color: '#f87171', offset: [6, 6], fontSize: 11 },
    }
  )

  board.create('arrow', [O, U1], { strokeColor: '#38bdf8', strokeWidth: 2.2 })
  board.create('arrow', [O, U2], { strokeColor: '#38bdf8', strokeWidth: 2.2 })
  board.create('arrow', [O, U3], { strokeColor: '#f87171', strokeWidth: 2.4 })
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

watch([thetaDeg, phiDeg, alphaDeg], () => {
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
      <label>θ: <input v-model.number="thetaDeg" type="range" min="0" max="180" style="width: 55px; vertical-align: middle" /></label>
      <label>φ: <input v-model.number="phiDeg" type="range" min="0" max="360" style="width: 55px; vertical-align: middle" /></label>
      <label>Rot P: <input v-model.number="alphaDeg" type="range" min="0" max="360" style="width: 55px; vertical-align: middle" /></label>
      <button class="jxg-btn" @click="setWorkedExample">Contoh x+y−√2z=0</button>
      <button class="jxg-btn" @click="setStandardEquatorial">Ekuator z=0</button>
    </div>
    <div class="jxg-readout">
      <div>
        <span style="color: #f87171">u₃ = ({{ triad.u3[0].toFixed(2) }}, {{ triad.u3[1].toFixed(2) }}, {{ triad.u3[2].toFixed(2) }})</span>
        &nbsp;|&nbsp;
        <span style="color: #34d399">[r̄<sub>P</sub>]<sub>ℬ</sub> = diag(1,1,−1)</span>
      </div>
      <div style="color: #38bdf8; margin-top: 2px">
        H<sub>u₃</sub> = I₃ − 2u₃u₃ᵀ: {{ householderDiag }}
      </div>
    </div>
  </div>
</template>
