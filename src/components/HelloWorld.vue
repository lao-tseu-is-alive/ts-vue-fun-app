<template>
  <v-container class="fill-height">
    <v-responsive
      class="align-centerfill-height mx-auto"
      max-width="900"
    >
      <v-row>
        <v-col cols="12">
          <point-view :pname="pointName" :px="pointX" :py="pointY"/>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="4">
          <v-text-field
            label="Point Name"
            v-model="pointName"
            clearable   
          />
        </v-col>
        <v-col cols="4">
          <v-text-field
            label="Point X"
            type="number"
            v-model="pointX"
            clearable
          />
        </v-col>
        <v-col cols="4">
          <v-text-field
            label="Point Y"
            type="number"
            v-model="pointY"
            clearable
          />
        </v-col>                
      </v-row>
    </v-responsive>
  </v-container>
</template>

<script setup lang="ts">
import  PointView from '@/components/PointView.vue'
import { ref } from 'vue'

const pointName = ref('A')
const pointX = ref(5)
const pointY = ref(3)
</script>
