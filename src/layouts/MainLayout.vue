<template>
<q-layout view="hHh lpR fFf">
<!-- 
    <q-header class="bg-white">
      <q-toolbar>
        
      </q-toolbar>
    </q-header> -->

    <q-page-container>
      <router-view />
    </q-page-container>

    <!-- <q-footer class="bg-white">
      <q-toolbar>
      </q-toolbar>
    </q-footer> -->

  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'


export default defineComponent({
  name: 'MainLayout',

  components: {
   
  },

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
