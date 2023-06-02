<template>
  <q-page class="flex flex-center q-gutter-lg q-pa-lg">

      <q-btn
        class="bubbleBtns"
        glossy
        color="green"
        label="Курсор"
      />
      <q-btn
        class="bubbleBtns"
        glossy
        color="green"
        label="Смещение"
      />
      <q-btn
        class="bubbleBtns"
        glossy
        color="green"
        label="Вращение"
      />
      <q-btn
        class="bubbleBtns"
        color="green"
        glossy
        label="Масштабирование"
      />
      <canvas id="renderCanvas" touch-action="none"></canvas>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  mounted() {
    const canvas = document.querySelector("canvas")
  }
})
</script>
<style>
  #renderCanvas {
        width: 100%;
        height: 100%;
        touch-action: none;
      }
</style>