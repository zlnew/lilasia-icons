<script setup lang="ts">
import 'lilasia-icons/dist/style.css'
import Icon from 'lilasia-icons'
import { ref } from 'vue'

const name = ref()
</script>

<template>
  <div class="container">
    <h1>Lilasia Icons</h1>
    <input v-model="name" type="text" placeholder="Type for icon name" />
    <Icon :name="name" :size="50" />
  </div>
</template>
