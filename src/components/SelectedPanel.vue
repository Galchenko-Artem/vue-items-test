<template>
  <div class="panel">
    <div class="panel__head">
      <div class="panel__title">{{ title }}</div>
      <div class="panel__meta">{{ meta }}</div>
    </div>

    <div class="panel__items">
      <ItemCard
        v-for="item in selectedItems"
        :key="item.id"
        :item="item"
        :isActive="true"
        @toggle="$emit('remove', item.id)"
      />
    </div>

    <p v-if="emptyText && selectedItems.length === 0" class="panel__meta">
      {{ emptyText }}
    </p>
  </div>
</template>

<script setup>
import ItemCard from "./ItemCard.vue";

defineProps({
  title: { type: String, required: true },
  meta: { type: String, default: "" },
  emptyText: { type: String, default: "" },
  selectedItems: { type: Array, required: true },
});

defineEmits(["remove"]);
</script>
