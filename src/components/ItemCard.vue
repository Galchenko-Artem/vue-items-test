<template>
  <button
    type="button"
    class="item-card"
    :class="{ 'is-active': isActive }"
    @click="$emit('toggle')"
  >
    {{ item.name }}
  </button>
</template>

<script setup>
defineProps({
  item: { type: Object, required: true },
  isActive: { type: Boolean, default: false },
});

defineEmits(["toggle"]);
</script>
