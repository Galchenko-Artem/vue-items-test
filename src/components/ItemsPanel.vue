<template>
  <div class="panel">
    <div class="panel__head">
      <div class="panel__title">{{ title }}</div>
      <div class="panel__meta">{{ meta }}</div>
    </div>

    <div class="panel__items">
      <ItemCard
        v-for="item in items"
        :key="item.id"
        :item="item"
        :isActive="isActive(item.id)"
        @toggle="$emit('toggle', item.id)"
      />
    </div>

    <p v-if="warning" class="panel__warning">{{ warning }}</p>

    <button v-if="showClear" class="btn" type="button" @click="$emit('clear')">
      Clear selection
    </button>
  </div>
</template>

<script setup>
import ItemCard from "./ItemCard.vue";

defineProps({
  title: { type: String, required: true },
  meta: { type: String, default: "" },
  items: { type: Array, required: true },
  isActive: { type: Function, required: true },
  warning: { type: String, default: "" },
  showClear: { type: Boolean, default: false },
});

defineEmits(["toggle", "clear"]);
</script>
