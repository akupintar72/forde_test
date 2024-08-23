<script setup>
import { defineProps, ref } from 'vue'

defineProps({
  msg: String,
})

</script>

<template>

  <h2>{{ msg }}</h2>
  <p>xexx</p>
</template>



<style scoped>
.read-the-docs {
  color: #888;
}
</style>
