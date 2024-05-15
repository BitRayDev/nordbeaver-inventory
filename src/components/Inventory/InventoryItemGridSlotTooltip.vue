<template>
  <Teleport to="body">
    <div
      class="inventory-item-grid-slot-tooltip"
      ref="tooltipEl"
    >
      <slot/>
    </div>
  </Teleport>
</template>

<script setup>
import {onMounted, ref} from "vue";

const tooltipEl = ref(null);

function onMouseMove(e) {
  if (!tooltipEl.value)
    return;

  let tooltipWidth = tooltipEl.value.offsetWidth;
  let tooltipHeight = tooltipEl.value.offsetHeight;
  let pageWidth = document.documentElement.clientWidth;
  let pageHeight = document.documentElement.clientHeight;

  let x = e.pageX + 10;
  let y = e.pageY + 10;

  if (x + tooltipWidth > pageWidth) {
    x = e.pageX - tooltipWidth - 10;
  }
  if (y + tooltipHeight > pageHeight) {
    y = e.pageY - tooltipHeight - 10;
  }

  tooltipEl.value.style.left = `${x}px`;
  tooltipEl.value.style.top = `${y }px`;

  // Можно было использовать реактивность и ассигнить эти значения через style prop, но это влияет на перфоманс, поэтому стайлим напрямую DOM элемент
}

onMounted(() => {
  document.addEventListener('mousemove', onMouseMove)
})
</script>

<style lang="scss">
.inventory-item-grid-slot-tooltip {
  position: absolute;

  width: 120rem;
  padding: 6rem;

  font-family: var(--font-family-accent);
  font-weight: 900;
  font-size: 16rem;
  text-transform: uppercase;
  text-align: center;
  color: #fff;

  border: 3rem solid #6c6c6c;
  border-radius: 0 3rem 3rem 3rem;
  background-color: #1E1E1E;
  box-shadow: 0 0 4rem #1E1E1E;
}
</style>