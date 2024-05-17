<template>
  <div class="inventory-item-grid">
    <InventoryItemGridSlot
      class="inventory-item-grid__slot"
      v-for="(_, i) in items.length < 40 ? 40 : Math.ceil(items.length / 5) * 5"
      :key="items[i]?.id ?? i"
      :item="items[i]"
    />
    <Transition name="fade">
      <div class="inventory-item-grid__loading-overlay" v-if="loading">
        <LoadingSpinner class="inventory-item-grid__loading-spinner"/>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import InventoryItemGridSlot from "@/components/Inventory/InventoryItemGridSlot.vue";
import LoadingSpinner from "@/components/LoadingSpinner.vue";

const props = defineProps({
  items: {
    type: Array,
    default: () => [],
    required: true,
  },
  loading: {
    type: Boolean,
    default: false,
    required: false,
  }
})
</script>

<style lang="scss">
.inventory-item-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  position: relative;

  &__slot {
    aspect-ratio: 1;
  }

  &__loading-overlay {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;

    background-color: rgba(0, 0, 0, 0.6);
    backdrop-filter: blur(1rem);
  }

  &__loading-spinner {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
</style>