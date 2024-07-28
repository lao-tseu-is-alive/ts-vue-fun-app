<style scoped>
</style>

<template>
  <mysvg :pname="pointName" :px="px" :py="py" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import mysvg from './components/svg.vue'

const pointName=ref("P1")
const px=ref(5)
const py=ref(36)

</script>
