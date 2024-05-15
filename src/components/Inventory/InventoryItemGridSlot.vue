<template>
  <div
    class="inventory-item-grid-slot"
    :style="style"
    @mouseenter="onMouseEnter"
    @mouseleave="onMouseLeave"
  >
    <template v-if="item">
      <div class="inventory-item-grid-slot__content">
        <img
          class="inventory-item-grid-slot__image"
          :src="item.imageUrl"
          :alt="item.name"
          v-if="item.imageUrl"
        />
        <p class="inventory-item-grid-slot__charges" v-if="item.charges">
          <span>{{ item.charges }}</span>
          <span v-if="item.maxCharges"> / {{ item.maxCharges }}</span>
        </p>
        <p class="inventory-item-grid-slot__count" v-if="item.count">
          x{{ item.count }}
        </p>
      </div>
      <Transition name="fade">
        <div class="inventory-item-grid-slot__cooldown" v-if="remainingCooldown > 0">
          <img class="inventory-item-grid-slot__cooldown-icon" :src="cooldownIcon" alt="cooldown"/>
          <p class="inventory-item-grid-slot__cooldown-label">
            {{ formatMilliseconds(remainingCooldown) }}
          </p>
        </div>
      </Transition>
      <InventoryItemGridSlotTooltip v-if="isTooltipVisible">
        {{ item.name }}
      </InventoryItemGridSlotTooltip>
    </template>
  </div>
</template>

<script setup>
import cooldownIcon from '@/assets/images/icons/cooldown.svg';
import {computed, onMounted, ref, watch} from "vue";
import InventoryItemGridSlotTooltip from "@/components/Inventory/InventoryItemGridSlotTooltip.vue";

const props = defineProps({
  item: {
    type: Object,
    default: null,
    required: false,
  }
})

const remainingCooldown = ref(0);

const isTooltipVisible = ref(false);

function onMouseEnter() {
  isTooltipVisible.value = true;
}

function onMouseLeave() {
  isTooltipVisible.value = false;
}

const backlightColor = computed(() => {
  switch (props.item?.type) {
    case 'armor':
      return '#367CCE';
    case 'weapon':
      return '#7F59CE';
    default:
      return null;
  }
})

const style = computed(() => {
  let background = '';
  if (backlightColor.value)
    background += `radial-gradient(59.14% 59.14% at 50% 50%, ${backlightColor.value} 0%, ${backlightColor.value}10 95%), `;
  background += '#1a1a1a';

  return {
    background
  }
})

let interval = null;
function checkCooldown() {
  if (interval) {
    clearInterval(interval);
  }

  if (props.item?.cooldown) {
    remainingCooldown.value = props.item?.cooldown - Date.now();
    const interval = setInterval(() => {
      remainingCooldown.value = props.item?.cooldown - Date.now();

      if (remainingCooldown.value < 0) {
        clearInterval(interval);
      }
    }, 1000)
  }
}
onMounted(() => {
  checkCooldown();
})
watch(() => props.item, () => {
  checkCooldown();
})

function formatMilliseconds(ms) {
  const MILLISECONDS_IN_SECOND = 1000;
  const MILLISECONDS_IN_MINUTE = MILLISECONDS_IN_SECOND * 60;
  const MILLISECONDS_IN_10_MINUTES = MILLISECONDS_IN_MINUTE * 10;
  const MILLISECONDS_IN_HOUR = MILLISECONDS_IN_MINUTE * 60;
  const MILLISECONDS_IN_DAY = MILLISECONDS_IN_HOUR * 24;
  const MILLISECONDS_IN_WEEK = MILLISECONDS_IN_DAY * 7;
  const MILLISECONDS_IN_MONTH = MILLISECONDS_IN_DAY * 30; // Approximating 1 month as 30 days

  const absMs = Math.abs(ms);

  let result = '';
  if (absMs >= MILLISECONDS_IN_MONTH) {
    const months = Math.round(absMs / MILLISECONDS_IN_MONTH);
    result = `${months}mon`;
  } else if (absMs >= MILLISECONDS_IN_WEEK) {
    const weeks = Math.round(absMs / MILLISECONDS_IN_WEEK);
    result = `${weeks}w`;
  } else if (absMs >= MILLISECONDS_IN_DAY) {
    const days = Math.round(absMs / MILLISECONDS_IN_DAY);
    result = `${days}d`;
  } else if (absMs >= MILLISECONDS_IN_HOUR) {
    const hours = Math.round(absMs / MILLISECONDS_IN_HOUR);
    result = `${hours}h`;
  } else if (absMs >= MILLISECONDS_IN_10_MINUTES) {
    const minutes = Math.round(absMs / MILLISECONDS_IN_MINUTE);
    result = `${minutes}m`;
  } else {
    const seconds = Math.round(absMs / MILLISECONDS_IN_SECOND);
    result = `${seconds}s`;
  }

  return ms < 0 ? `-${result}` : result;
}
</script>

<style lang="scss">
.inventory-item-grid-slot {
  position: relative;

  border: 1px solid #454545;

  &__content {
    display: flex;
    align-items: center;
    justify-content: center;

    width: 100%;
    height: 100%;
  }

  &__image {
    width: 85%;
    max-height: 85%;
    object-fit: cover;
  }

  &__charges {
    position: absolute;
    left: 0;
    top: 0;

    padding-left: 2rem;
    padding-top: 1rem;

    isolation: isolate;

    font-family: var(--font-family-text);
    font-weight: 500;
    font-size: 17rem;
    color: #fff;

    &::before {
      content: "";
      position: absolute;
      left: 0;
      top: 0;

      width: 200%;
      height: 150%;

      background: linear-gradient(to bottom right, rgba(6, 6, 6, 0.65) 0%, rgba(6, 6, 6, 0.65) 50%, transparent 50%, transparent 100%);
      z-index: -1;
    }
  }

  &__count {
    font-family: var(--font-family-text);
    font-weight: 500;
    font-size: 17rem;
    text-align: right;
    color: #fff;

    position: absolute;
    bottom: 1rem;
    right: 4rem;
  }

  &__cooldown {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2rem;

    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;

    font-family: var(--font-family-accent);
    font-weight: 900;
    font-size: 20rem;
    color: #fff;

    background-color: color-mix(in srgb, #1a1a1a, transparent 25%);
  }
}
</style>