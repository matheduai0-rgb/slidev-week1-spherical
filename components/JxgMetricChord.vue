<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'
import JXG from 'jsxgraph'

const props = defineProps({
  height: { type: String, default: '275px' },
  showBisector: { type: Boolean, default: true },
})

const boxId = 'jxg-' + Math.random().toString(36).slice(2, 10)
const boardEl = ref<HTMLElement | null>(null)
let board: any = null
let resizeObs: ResizeObserver | null = null
const BBOX: [number, number, number, number] = [-1.45, 1.4, 1.55, -1.4]

const dSpherical = ref(1.396) // ~80 deg in rad
const dChord = ref(1.286)
const dotProd = ref(0.174)
const degVal = ref(80.0)

let p1Glider: any = null
let p2Glider: any = null

function resetPoints() {
  if (!board || !p1Glider || !p2Glider) return
  p1Glider.moveTo([Math.cos((40 * Math.PI) / 180), Math.sin((40 * Math.PI) / 180)])
  p2Glider.moveTo([Math.cos((-40 * Math.PI) / 180), Math.sin((-40 * Math.PI) / 180)])
  board.update()
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
    size: 3,
    fillColor: '#e6edf3',
    strokeColor: '#e6edf3',
    label: { color: '#e6edf3', offset: [-14, -4], fontSize: 12 },
  })

  const unitCircle = board.create('circle', [O, 1], {
    strokeColor: '#58a6ff',
    strokeWidth: 2,
    fillColor: '#1b2533',
    fillOpacity: 0.35,
    highlight: false,
  })

  p1Glider = board.create('glider', [Math.cos(0.7), Math.sin(0.7), unitCircle], {
    name: 'p₁',
    size: 5,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [8, 8], fontSize: 13 },
  })

  p2Glider = board.create('glider', [Math.cos(-0.7), Math.sin(-0.7), unitCircle], {
    name: 'p₂',
    size: 5,
    fillColor: '#fbbf24',
    strokeColor: '#ffffff',
    label: { color: '#fbbf24', offset: [8, -12], fontSize: 13 },
  })

  // Radii 0-p1 and 0-p2
  board.create('segment', [O, p1Glider], {
    strokeColor: '#8b949e',
    strokeWidth: 1.6,
  })
  board.create('segment', [O, p2Glider], {
    strokeColor: '#8b949e',
    strokeWidth: 1.6,
  })

  // Midpoint M of chord p1-p2
  const M = board.create(
    'point',
    [
      () => 0.5 * (p1Glider.X() + p2Glider.X()),
      () => 0.5 * (p1Glider.Y() + p2Glider.Y()),
    ],
    {
      name: 'M',
      size: 3,
      fillColor: '#34d399',
      strokeColor: '#34d399',
      visible: props.showBisector,
      label: { color: '#34d399', offset: [-16, -2], fontSize: 11 },
    }
  )

  if (props.showBisector) {
    board.create('segment', [O, M], {
      strokeColor: '#34d399',
      strokeWidth: 1.8,
      dash: 2,
    })
  }

  // Euclidean chord d(p1, p2)
  board.create('segment', [p1Glider, p2Glider], {
    strokeColor: '#f87171',
    strokeWidth: 2.8,
  })

  // Shorter spherical arc d_S2(p1, p2)
  board.create(
    'curve',
    [
      (t: number) => {
        const a1 = Math.atan2(p1Glider.Y(), p1Glider.X())
        const a2 = Math.atan2(p2Glider.Y(), p2Glider.X())
        let diff = a2 - a1
        while (diff > Math.PI) diff -= 2 * Math.PI
        while (diff < -Math.PI) diff += 2 * Math.PI
        return Math.cos(a1 + t * diff)
      },
      (t: number) => {
        const a1 = Math.atan2(p1Glider.Y(), p1Glider.X())
        const a2 = Math.atan2(p2Glider.Y(), p2Glider.X())
        let diff = a2 - a1
        while (diff > Math.PI) diff -= 2 * Math.PI
        while (diff < -Math.PI) diff += 2 * Math.PI
        return Math.sin(a1 + t * diff)
      },
      0,
      1,
    ],
    {
      strokeColor: '#38bdf8',
      strokeWidth: 3.4,
    }
  )

  board.on('update', () => {
    const x1 = p1Glider.X()
    const y1 = p1Glider.Y()
    const x2 = p2Glider.X()
    const y2 = p2Glider.Y()
    const ip = Math.max(-1, Math.min(1, x1 * x2 + y1 * y2))
    const theta = Math.acos(ip)
    const chord = Math.hypot(x1 - x2, y1 - y2)
    dotProd.value = Number(ip.toFixed(3))
    dSpherical.value = Number(theta.toFixed(3))
    dChord.value = Number(chord.toFixed(3))
    degVal.value = Number(((theta * 180) / Math.PI).toFixed(1))
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

onBeforeUnmount(() => {
  if (resizeObs) resizeObs.disconnect()
  if (board) JXG.JSXGraph.freeBoard(board)
})
</script>

<template>
  <div class="jxg-card">
    <div :id="boxId" ref="boardEl" class="jxgbox" :style="{ width: '100%', height: props.height }"></div>
    <div class="jxg-controls">
      <span style="color: #8b949e">Geser titik <b style="color: #fbbf24">p₁, p₂</b> pada lingkaran:</span>
      <button class="jxg-btn" @click="resetPoints">Reset 80°</button>
    </div>
    <div class="jxg-readout">
      <div>
        <span style="color: #38bdf8">θ = d<sub>S²</sub>(p₁,p₂) = {{ dSpherical }} rad ({{ degVal }}°)</span>
        &nbsp;|&nbsp;
        <span>⟨p₁,p₂⟩ = cos θ = {{ dotProd }}</span>
      </div>
      <div style="color: #f87171; margin-top: 2px">
        d<sub>E³</sub>(p₁,p₂) = |p₁ − p₂| = 2 sin(θ/2) = {{ dChord }}
      </div>
    </div>
  </div>
</template>
