<template>
  <div class="inventory-item-grid">
    <InventoryItemGridSlot
      class="inventory-item-grid__slot"
      v-for="i in items.length < 40 ? 40 : Math.ceil(items.length / 5) * 5"
      :item="items[i]"
    />
  </div>
</template>

<script setup>
import InventoryItemGridSlot from "@/components/Inventory/InventoryItemGridSlot.vue";

const props = defineProps({
  items: {
    type: Array,
    default: () => [],
    required: true,
  }
})
</script>

<style lang="scss">
.inventory-item-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);

  &__slot {
    aspect-ratio: 1;
  }
}
</style>