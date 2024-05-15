<template>
  <Inventory :items="items"/>
</template>

<script setup>
import Inventory from "@/components/Inventory/Inventory.vue";
import {onMounted, ref} from "vue";

const mockItems = [
  {
    "id": "3dde92f3bed87eb78a687ad06fe579661f3c8408",
    "type": "misc",
    "name": "Energy Dring",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/energy_potion.png?alt=media",
    "count": 50,
    "charges": 3,
    "maxCharges": 3
  },
  {
    "id": "xa4b9237bachcdf19c0760cab7aec4a8359010b0",
    "type": "armor",
    "name": "Magic Boots",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/magic_boots.png?alt=media"
  },
  {
    "id": "ea40e585fc4aba2ab50ba2e5859fa885250be57d",
    "type": "misc",
    "name": "Energy Dring",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/energy_potion.png?alt=media",
    "count": 1,
    "charges": 3,
    "maxCharges": 3
  },
  {
    "id": "da4b9237bacccdf19c0760cab7aec4a8359010b0",
    "type": "weapon",
    "name": "Double Swords",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/weapon_dark_saber_combo_icon.png?alt=media"
  },
  {
    "id": "8d8b20c6a8d6f1b7bea7b701dc450488543f479a",
    "type": "weapon",
    "name": "Drone Weapon",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/weapon_dron_shield_icon.png?alt=media"
  },
  {
    "id": "350702752173745b7ecf053d5ac7c0f3ae8c2b30",
    "type": "misc",
    "name": "Scroll",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/scroll.png?alt=media",
    "count": 2
  },
  {
    "id": "95a2b45d0959fce61de5e695235fb8edbfebad66",
    "type": "armor",
    "name": "Drone Shield",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/weapon_dron_shield_icon.png?alt=media"
  },
  {
    "id": "15140a758aaeaf5f8d2a45e619c8349fc7ea91ae",
    "type": "misc",
    "name": "Strange Potion",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/strange_potion.png?alt=media",
    "count": 8,
    "cooldown": Date.now() + 10000
  },
  {
    "id": "67140a758aaeaf5f8d2a45e619c8349fc7ea91ae",
    "type": "misc",
    "name": "Strange Potion",
    "imageUrl": "https://firebasestorage.googleapis.com/v0/b/seven-seven-bit-inhouse-helper.appspot.com/o/strange_potion.png?alt=media",
    "count": 1,
    "charges": 3,
    "maxCharges": 3
  }
]

const items = ref([]);

onMounted(() => {
  const urlParams = new URLSearchParams(window.location.search);
  const caseValue = urlParams.get('case');

  if (caseValue) {
    fetch(`https://us-central1-seven-seven-bit-inhouse-helper.cloudfunctions.net/vueDevTestTask-getInventoryState?case=${caseValue}`)
      .then(response => response.json())
      .then(data => {
        items.value = data.inventory;
      })
      .catch(e => console.error(e));
  }
})
</script>

<style>
</style>
