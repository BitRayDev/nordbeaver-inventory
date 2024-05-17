<template>
  <div class="inventory" id="inventory">
    <InventoryHeader
      class="inventory__header"
      v-model:tab="selectedTab"
    />
    <div class="inventory__content">
      <InventoryItemFilter
        class="inventory__item-filter"
        v-model:category="selectedCategory"
      />
      <div class="inventory__items">
        <p class="inventory__selected-category-label">
          {{ selectedCategoryLabel }}
        </p>
        <div class="inventory__item-grid-container">
          <InventoryItemGrid
            class="inventory__item-grid"
            :items="filteredItems"
            :loading="loading"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import '@/assets/fonts/apercu-pro/stylesheet.css';
import '@/assets/fonts/commuters-sans/stylesheet.css';

import InventoryHeader from "./InventoryHeader.vue";
import InventoryItemFilter from "@/components/Inventory/InventoryItemFilter.vue";
import {computed, ref} from "vue";
import InventoryItemGrid from "@/components/Inventory/InventoryItemGrid.vue";

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

const selectedTab = ref();
const selectedCategory = ref();

const selectedCategoryLabel = computed(() => {
  if (!selectedCategory.value)
    return 'All Items';

  return selectedCategory.value;
})

const filteredItems = computed(() => {
  if (!selectedCategory.value)
    return props.items;

  return props.items.filter(item => item.type === selectedCategory.value);
})
</script>

<style lang="scss">
.inventory {
  display: flex;
  flex-direction: column;

  height: 100dvh;

  &__header {
    width: 100%;
  }

  &__content {
    display: flex;
    flex-grow: 1;

    overflow: hidden;
  }

  &__item-filter {
    flex-shrink: 0;
    overflow: auto;
  }

  &__items {
    display: flex;
    flex-direction: column;

    position: relative;

    flex-grow: 1;

    border: 1px solid #000;
    background: #242223;

    overflow: hidden;
  }

  &__selected-category-label {
    margin: 6rem 12rem 0;

    font-family: var(--font-family-accent);
    font-weight: 900;
    font-size: 20rem;
    text-transform: uppercase;
    color: #fff;
  }

  &__item-grid-container {
    overflow: auto;

    padding: 0 6rem;
    margin: 6rem 6rem 12rem;
  }
}
</style>