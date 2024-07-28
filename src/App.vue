<style scoped>
</style>

<template>
  <mysvg :pname="pointName" :px="px" :py="py" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import mysvg from './components/svg.vue'

const pointName=ref("P1")
const px=ref(2)
const py=ref(3)

</script>
