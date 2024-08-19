<template>
  <q-page class="flex flex-center">
    <TicTacToe />
  </q-page>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import TicTacToe from 'components/TicTacToe.vue'
export default defineComponent({
  name: 'IndexPage',
  components: { TicTacToe }
})
</script>
<style scoped>
.q-page {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>
