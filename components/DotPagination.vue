<template>
  <div class="dot-pagination">
    <div class="dot active"></div>
    <span class="page-number">{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
  </div>
</template>

<script setup>
</script>

<style scoped>
.dot-pagination {
  position: fixed;
  bottom: 1rem;
  right: 1rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  z-index: 100;
  opacity: 0.7;
  transition: opacity 0.3s ease;
}

.dot-pagination:hover {
  opacity: 1;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background-color: #3b82f6;
  box-shadow: 0 0 6px rgba(59, 130, 246, 0.5);
  animation: pulse 2s ease-in-out infinite;
}

.page-number {
  font-size: 0.7rem;
  color: #64748b;
  font-family: 'Fira Code', monospace;
  font-weight: 500;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.2);
    opacity: 0.8;
  }
}
</style>
