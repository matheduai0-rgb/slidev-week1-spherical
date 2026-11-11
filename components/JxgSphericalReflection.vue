<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '265px' },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.45, 1.38, 1.45, -1.38]

const mirrorAngle = ref(15) // angle of mirror plane P in degrees
const innerProdVal = ref(0.58)
let pPoint: any = null

function getNormal() {
  const rad = ((mirrorAngle.value + 90) * Math.PI) / 180
  return [Math.cos(rad), Math.sin(rad)]
}

function reflectVec(x: number, y: number) {
  const [ux, uy] = getNormal()
  const ip = x * ux + y * uy
  return [x - 2 * ip * ux, y - 2 * ip * uy]
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
    label: { color: '#e6edf3', offset: [-12, -10], fontSize: 11 },
  })

  board.create('circle', [O, 1.05], {
    strokeColor: '#58a6ff',
    strokeWidth: 1.8,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  // Mirror great circle G (projected as diameter + perspective ellipse)
  board.create(
    'curve',
    [
      (t: number) => {
        const a = (mirrorAngle.value * Math.PI) / 180
        const x = 1.05 * Math.cos(t)
        const y = 0.24 * Math.sin(t)
        return x * Math.cos(a) - y * Math.sin(a)
      },
      (t: number) => {
        const a = (mirrorAngle.value * Math.PI) / 180
        const x = 1.05 * Math.cos(t)
        const y = 0.24 * Math.sin(t)
        return x * Math.sin(a) + y * Math.cos(a)
      },
      0,
      2 * Math.PI,
    ],
    { strokeColor: '#34d399', strokeWidth: 2.6 }
  )

  // Unit normal pole u
  const U = board.create(
    'point',
    [() => 1.05 * getNormal()[0], () => 1.05 * getNormal()[1]],
    {
      name: 'u (Kutub)',
      fixed: true,
      size: 3.5,
      fillColor: '#60a5fa',
      strokeColor: '#ffffff',
      label: { color: '#60a5fa', offset: [6, 6], fontSize: 11 },
    }
  )
  board.create('arrow', [O, U], { strokeColor: '#60a5fa', strokeWidth: 1.8 })

  // Draggable point p
  pPoint = board.create('point', [0.45, 0.82], {
    name: 'p₁',
    size: 5,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [8, 6], fontSize: 12 },
  })

  // Reflected point r_G(p1) = p2
  const rPoint = board.create(
    'point',
    [
      () => reflectVec(pPoint.X(), pPoint.Y())[0],
      () => reflectVec(pPoint.X(), pPoint.Y())[1],
    ],
    {
      name: 'p₂ = r̄_G(p₁)',
      fixed: true,
      size: 5,
      fillColor: '#f87171',
      strokeColor: '#ffffff',
      label: { color: '#f87171', offset: [8, -10], fontSize: 12 },
    }
  )

  // Perpendicular bisector chord p1 - p2
  board.create('segment', [pPoint, rPoint], {
    strokeColor: '#e6edf3',
    strokeWidth: 1.6,
    dash: 2,
  })

  // Orthogonal projection midpoint on mirror G
  board.create(
    'point',
    [
      () => 0.5 * (pPoint.X() + rPoint.X()),
      () => 0.5 * (pPoint.Y() + rPoint.Y()),
    ],
    {
      name: '',
      fixed: true,
      size: 2.5,
      fillColor: '#34d399',
      strokeColor: '#34d399',
    }
  )

  board.on('update', () => {
    const [ux, uy] = getNormal()
    innerProdVal.value = Number((pPoint.X() * ux + pPoint.Y() * uy).toFixed(3))
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

watch(mirrorAngle, () => {
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
      <span style="color: #8b949e">Geser <b style="color: #fbbf24">p₁</b> atau putar cermin <b style="color: #34d399">G</b>:</span>
      <input v-model.number="mirrorAngle" type="range" min="-60" max="60" style="width: 90px; vertical-align: middle" />
      <span>{{ mirrorAngle }}°</span>
    </div>
    <div class="jxg-readout">
      <span style="color: #34d399">Fix(r̄<sub>G</sub>) = G</span> &nbsp;|&nbsp;
      <span style="color: #f87171">r̄<sub>G</sub>(p₁) = p₁ − 2⟨p₁,u⟩u = p₂</span>
      &nbsp;(⟨p₁,u⟩ = {{ innerProdVal }})
    </div>
  </div>
</template>
