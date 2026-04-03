<template>
  <footer class="app-footer">
    <div class="footer-bottom">
      <p>&copy; {{ currentYear }} AI Super Agent Platform</p>
    </div>
  </footer>
</template>

<script setup>
import { computed } from 'vue'

const currentYear = computed(() => new Date().getFullYear())
</script>

<style scoped>
.app-footer {
  background-color: rgba(17, 23, 41, 0.9);
  padding: 20px 0;
  color: rgba(255, 255, 255, 0.5);
  width: 100%;
  margin-top: auto;
  position: relative;
  z-index: 2;
}

.footer-bottom {
  text-align: center;
  font-size: 0.9rem;
}
</style>
