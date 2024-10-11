<template>
  <div class="hello">{{vari}}</div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup(){
    const vari = ref('hello')

    return {vari}
  }
}
</script>

<style>

</style>
