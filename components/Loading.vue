<template>
  <div v-if="isLoading" class="loading-container">
    <div class="loader"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLoading: true
    };
  },
  methods: {
    toggleLoader(){
      this.isLoading = !this.isLoading
    }
  },
};
</script>
<style scoped>
.loader {
  height: 60px;
  aspect-ratio: 1;
  --c: no-repeat linear-gradient(#fff 0 0);
  background: var(--c), var(--c), var(--c), var(--c);
  background-size: 33.4% 33.4%;
  animation: l7 1.5s infinite linear;
}
@keyframes l7 {
  0%,
  5% {
    background-position: 0 0, 50% 0, 0 50%, 50% 50%;
  }
  25% {
    background-position: 0 0, 100% 0, 0 100%, 50% 50%;
  }
  50% {
    background-position: 50% 0, 100% 0, 0 100%, 0 50%;
  }
  75% {
    background-position: 50% 0, 100% 50%, 0 100%, 0 0;
  }
  95%,
  100% {
    background-position: 50% 0, 50% 50%, 0 50%, 0 0;
  }
}
</style>
