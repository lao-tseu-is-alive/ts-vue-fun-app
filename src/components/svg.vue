<style scoped>
svg {
  border: 2px solid red;
  width: 100%;
  background-color: azure;
}

.black-line-thin {
  stroke: #222222;
  stroke-width: 1px;
  fill: none;
}

.black-line-thick {
  stroke: #222222;
  stroke-width: 3px;
  fill: none;
}

.black-fill {
  fill: #222222;
}

.primary-line-thin {
  stroke: #2f94d3;
  stroke-width: 1px;
  fill: none;
}

.primary-line-thick {
  stroke: #2f94d3;
  stroke-width: 3px;
  fill: none;
}

.primary-fill {
  fill: #2f94d3;
}
</style>

<template>
  <svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" :width="svgWidth" :height="svgHeight"
    id="svgpoint" :viewBox="svgViewBox">
    <line id="axex" class="black-line-thick" x1="0" :y1="svgCenterY" :x2="svgWidth" :y2="svgCenterY" />
    <path id="axexarrow2" class="black-fill" d="M500 245 L500 255 L520 250 Z" />
    <path id="axexarrow" class="black-fill" :d="`M${svgHeight} ${svgCenterY - 5} L${svgHeight} 30 L${svgCenterY} 10 Z`"/>
    <template v-for="n in (Math.round((svgMaxScale / svgTick)) - 1)" v-bind:key="n">
      <path id="axexoneunit" class="black-line-thin" :d="`M${svgCenterX + ((n) * svgTick * svgScale)} 245
                   L${svgCenterX + ((n) * svgTick * svgScale)} 255 `" />
      <g font-size="12" font-family="sans-serif" fill="black" stroke="none" text-anchor="middle">
        <text :x="svgCenterX + ((n) * svgTick * svgScale)" y="250" dx="-25" dy="0"
          :transform="`rotate(270, ${svgCenterX + ((n) * svgTick * svgScale)}, 250 )`">
          {{ `${svgTickDisplay(svgTick, n)}` }}
        </text>
      </g>
    </template>
    <line id="axey" class="black-line-thick" :x1="svgCenterX" y1="15" :x2="svgCenterX" y2="490" />
    <path id="axeyarrow" class="black-fill" :d="`M${svgCenterX - 5} 30 L${svgCenterX + 5} 30 L250 10 Z`" />
    <template v-for="n in (Math.round((svgMaxScale / svgTick)) - 1)" v-bind:key="n">
      <path id="axexoneunit" class="black-line-thin" :d="`M${svgCenterX - 10} ${svgCenterY - ((n) * svgTick * svgScale)}
                   L${svgCenterX + 10} ${svgCenterY - ((n) * svgTick * svgScale)}`" />
      <g font-size="12" font-family="sans-serif" fill="black" stroke="none" text-anchor="middle">
        <text :x="`${svgCenterX - 20}`" :y="svgCenterY - ((n) * svgTick * svgScale)" dx="-15" dy="0">
          {{ `${svgTickDisplay(svgTick, n)}` }}
        </text>
      </g>
    </template>
    <circle class="black-line-thin" :cx="svgCenterX" :cy="svgCenterY" r="5" />
    <line id="pointr" class="primary-line-thick" :x1="svgCenterX" :y1="svgCenterY" :x2="svgPointX" :y2="svgPointY" />
    <circle class="primary-line-thick" :cx="svgPointX" :cy="svgPointY" r="3" />
    <text :x="svgPointX + 10" :y="svgPointY - 10" dx="0" text-anchor="middle">
      {{ `${getPointNameTruncaded}:(${point.x}, ${point.y})` }}
    </text>
  </svg>

</template>

<script setup lang="ts">
import { reactive, ref, computed, defineProps } from 'vue'
import { Point } from 'ts-simple-2d-geometry';
// SVG DATA
const svgWidth = ref(500);
const svgHeight = ref(500);
const props = defineProps<{
  pname?: string,
  px: number,
  py: number,
}>()
// Point definition
const point = reactive<Point>(new Point(props.px, props.py, props.pname,));

const maxUnit = (x: number): number => {
  const theNumber = Number(x).toFixed(0);
  return (10 ** (theNumber.length - 1)) * (Number(theNumber.charAt(0)) + 1);
};

//COMPUTED SECTION
const getDistanceFromOrigin = computed((): number => {
  console.log(`IN COMPUTED getDistanceFromOrigin : (${point.x},${point.y})`);
  return point.getDistanceFromOrigin();
});
const getAngleinDegreeWithXAxis = computed((): number => {
  console.log(`IN COMPUTED getAngleinDegreeWithXAxis : (${point.x},${point.y})`);
  return point.getAngleDeg();
});
const getPointNameTruncaded = computed((): string => {
  console.log('IN COMPUTED getPointNameTruncaded()');
  const baseName = '';
  if ((typeof point === 'undefined') || (point.name === null)) {
    return baseName;
  }
  return `${baseName}${point.name}`;
});
const svgCenterX = computed((): number => {
  console.log(`IN COMPUTED getCenterX wxh: (${svgWidth.value}x${svgHeight.value})`);
  return Math.round(svgWidth.value / 2);
});
const svgCenterY = computed((): number => {
  console.log(`IN COMPUTED getCenterY wxh: (${svgWidth.value}x${svgHeight.value})`);
  return Math.round(svgHeight.value / 2);
});
const svgViewBox = computed((): string => {
  console.log(`IN COMPUTED svgViewBox wxh: (${svgWidth.value}x${svgHeight.value})`);
  return `0 0 ${svgWidth.value} ${svgHeight.value}`;
});
const svgMaxScale = computed((): number => {
  console.log(`IN COMPUTED svgMaxScale x,y: (${point.x}, ${point.y})`);
  const maxVal = Math.abs(point.x) > Math.abs(point.y) ? Math.abs(point.x) : Math.abs(point.y);
  let maxScale = maxUnit(maxVal);
  if ((maxVal > 0) && (maxVal < 1)) maxScale = 1;
  if ((maxVal > 1) && (maxVal < 11)) maxScale = 10;
  console.log(`IN COMPUTED svgMaxScale maxVal:${maxVal}, maxUnit(maxVal):${maxUnit(maxVal)} maxScale:${maxScale}`);
  return maxScale;
});
const svgTick = computed((): number => {
  // console.log(`IN COMPUTED svgTick x,y: (${point.x}, ${point.y})`);
  let tick = 0.1;
  if (svgMaxScale.value > 1 && svgMaxScale.value < 11) tick = 1;
  if (svgMaxScale.value > 10 && svgMaxScale.value < 101) tick = 10;
  if (svgMaxScale.value > 100 && svgMaxScale.value < 1001) tick = 100;
  if (svgMaxScale.value > 1000 && svgMaxScale.value < 5001) tick = 500;
  if (svgMaxScale.value > 5000 && svgMaxScale.value < 10001) tick = 1000;
  if (svgMaxScale.value > 10000) tick = maxUnit(svgMaxScale.value / 10);
  console.log(`IN COMPUTED svgTick tick:${tick}`);
  return tick;
});
const svgScale = computed((): number => {
  console.log(`IN COMPUTED svgScale x,y: (${point.x}, ${point.y})`);
  const maxScale = svgMaxScale.value;
  const scale = ((svgWidth.value / 2) / +maxScale);
  console.log(`IN COMPUTED svgScale maxScale:${maxScale} scale:${scale}`);
  return scale;
});
const svgPointX = computed((): number => {
  console.log(`IN COMPUTED svgPointX x,y: (${point.x}, ${point.y})`);
  return Math.round(svgWidth.value / 2) + (svgScale.value * point.x);
});
const svgPointY = computed((): number => {
  console.log(`IN COMPUTED svgPointY x,y: (${point.x}, ${point.y})`);
  return Math.round(svgHeight.value / 2) - (svgScale.value * point.y);
});

// ####### methods

const svgTickDisplay = (tick: number, n: number): string => {
  // console.log(`svgTickDisplay(${tick}, ${n})`);
  if ((tick > 0) && (tick < 1)) {
    return Number(n * tick).toFixed(2);
  }
  return Number(n * tick).toFixed(0);
};
</script>
