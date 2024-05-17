<template>
  <div class="loading-spinner"/>
</template>

<script setup>

</script>

<style lang="scss">
.loading-spinner {
  color: white
}
.loading-spinner,
.loading-spinner:after {
  box-sizing: border-box;
}
.loading-spinner {
  display: inline-block;
  width: 80rem;
  height: 80rem;
}
.loading-spinner:after {
  content: " ";
  display: block;
  width: 64rem;
  height: 64rem;
  margin: 8rem;
  border-radius: 50%;
  border: 6.4rem solid currentColor;
  border-color: currentColor transparent currentColor transparent;
  animation: loading-spinner 1.2s linear infinite;
}
@keyframes loading-spinner {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>