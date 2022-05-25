<script setup>
import NotePad from "./components/NotePad.vue";
</script>

<template>
  <div class="container">
    <NotePad />
  </div>
</template>

<style>
@import "./assets/base.css";

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
}
</style>
