<template>
  <div class="inventory-item-filter">
    <button
      class="inventory-item-filter__category"
      :class="{'inventory-item-filter__category_active': isCategoryActive(category)}"
      v-for="category in categories"
      :key="category.value"
      @click="setActiveCategory(category)"
    >
      <img :src="category.iconSrc" :alt="category.name"/>
    </button>
  </div>
</template>

<script setup>
import allItemsIcon from "@/assets/images/icons/all-items.svg";
import armorIcon from "@/assets/images/icons/armor.svg";
import weaponsIcon from "@/assets/images/icons/weapons.svg";
import miscIcon from "@/assets/images/icons/misc.svg";

const categories = [
  {
    iconSrc: allItemsIcon,
    name: 'All Items',
    value: null,
  },
  {
    iconSrc: armorIcon,
    name: 'Armor',
    value: 'armor',
  },
  {
    iconSrc: weaponsIcon,
    name: 'Weapons',
    value: 'weapon',
  },
  {
    iconSrc: miscIcon,
    name: 'Misc',
    value: 'misc',
  },
]

const activeCategory = defineModel('category');

function isCategoryActive(category) {
  return activeCategory.value == category.value;
}
function setActiveCategory(category) {
  return activeCategory.value = category.value;
}
</script>

<style lang="scss">
.inventory-item-filter {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 32rem;

  padding: 16rem;
  box-sizing: border-box;

  background-color: #393839;
  border: 1px solid #000;

  &__category {
    display: block;

    background: none;
    border: none;
    appearance: none;

    width: 36rem;

    opacity: 0.5;

    cursor: pointer;
    transition: opacity 200ms ease-out;

    &_active, &:hover {
      opacity: 1;
    }

    img {
      display: block;
      width: 100%;
    }
  }
}
</style>