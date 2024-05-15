<template>
  <div class="inventory-header">
    <div class="inventory-header__navigation inventory-navigation">
      <button
        class="inventory-navigation__tab"
        :class="{ 'inventory-navigation__tab_active': isTabActive(tab) }"
        v-for="tab in navigationTabs"
        :key="tab.value"
        @click="setActiveTab(tab)"
      >
        {{ tab.label }}
      </button>
    </div>
    <button
      class="inventory-navigation__close-button"
      @click="onCloseClick"
    >
      <img :src="closeIcon" alt="close"/>
    </button>
  </div>
</template>

<script setup>
import closeIcon from '@/assets/images/icons/close.svg'

const emit = defineEmits(['close'])

const navigationTabs = [
  {
    label: 'Backpack',
    value: 'BACKPACK'
  },
  {
    label: 'Nexus',
    value: 'NEXUS'
  },
]

const activeTab = defineModel('tab');

function isTabActive(tab) {
  return tab.value === activeTab.value;
}

function setActiveTab(tab) {
  activeTab.value = tab.value;
}

function onCloseClick() {
  emit('close')
}
</script>

<style lang="scss">
.inventory-header {
  display: flex;

  &__navigation {
    flex-grow: 1;
  }
}

.inventory-navigation {
  display: flex;

  &__tab {
    flex-grow: 1;

    padding: 12rem;

    font-family: var(--font-family-accent);
    font-weight: 900;
    font-size: 24rem;
    text-transform: uppercase;
    color: rgba(255, 255, 255, 0.47);

    border: 1px solid #000;
    background: #393839;

    cursor: pointer;

    transition: all 200ms ease-out;

    &_active, &:hover {
      background-color: #242223;
      color: #fff;
    }
  }

  &__close-button {
    display: flex;
    justify-content: center;
    align-items: center;

    height: 100%;
    aspect-ratio: 1;

    border: 1px solid #000;
    background: #393839;

    cursor: pointer;

    &, * {
      transition: all 200ms ease-out;
    }

    img {
      width: 50%;
      height: 50%;
      opacity: 0.5;
    }

    &:hover {
      background-color: #242223;

      img {
        opacity: 1;
      }
    }
  }
}
</style>