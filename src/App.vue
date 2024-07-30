<script setup lang="ts">
import 'lilasia-icons/dist/style.css'
import Icon from 'lilasia-icons'
import { ref } from 'vue'

const name = ref('person')
</script>

<template>
  <div class="container">
    <h1>Lilasia Icons</h1>
    <input v-model="name" type="text" placeholder="Type for icon name" />
    <div class="result">
      <Icon :name="name" :size="48" filled />
      <button v-if="name"><Icon :name="name" /></button>
    </div>
  </div>
</template>
