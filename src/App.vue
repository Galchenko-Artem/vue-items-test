<template>
  <main class="page">
    <h1 class="page__title">Тестовое задание VueJS</h1>

    <section class="grid">
      <SelectedPanel
        title="Selected (user items)"
        :meta="`selected: ${selectedLeftIds.length} / 6`"
        :selectedItems="selectedLeftItems"
        emptyText="Выберите от 1 до 6 вещей снизу слева."
        @remove="toggleLeft"
      />

      <SelectedPanel
        title="SELECTED ITEM"
        meta="only 1 from right"
        :selectedItems="selectedRightItem ? [selectedRightItem] : []"
        emptyText="Выберите 1 вещь снизу справа."
        @remove="clearRight"
      />
    </section>

    <section class="grid">
      <ItemsPanel
        title="User items"
        meta="click to select/unselect"
        :items="left"
        :isActive="(id) => selectedLeftIds.includes(id)"
        :warning="leftLimitReached ? 'Достигнут лимит 6 вещей (снимите выбор с одной, чтобы выбрать другую).' : ''"
        @toggle="toggleLeft"
      />

      <ItemsPanel
        title="Items to choose"
        meta="only one active"
        :items="right"
        :isActive="(id) => selectedRightId === id"
        :showClear="true"
        @toggle="selectRight"
        @clear="clearRight"
      />
    </section>
  </main>
</template>

<script setup>
import { computed, ref } from "vue";
import { leftItems, rightItems } from "./data/items";
import ItemsPanel from "./components/ItemsPanel.vue";
import SelectedPanel from "./components/SelectedPanel.vue";

const left = ref(leftItems);
const right = ref(rightItems);

const selectedLeftIds = ref([]);
const selectedRightId = ref(null);

const leftLimitReached = computed(() => selectedLeftIds.value.length >= 6);

function toggleLeft(id) {
  const idx = selectedLeftIds.value.indexOf(id);

  if (idx !== -1) {
    selectedLeftIds.value.splice(idx, 1);
    return;
  }

  if (selectedLeftIds.value.length >= 6) return;

  selectedLeftIds.value.push(id);
}

function selectRight(id) {
  selectedRightId.value = selectedRightId.value === id ? null : id;
}

function clearRight() {
  selectedRightId.value = null;
}

const selectedLeftItems = computed(() => {
  const map = new Map(left.value.map((i) => [i.id, i]));
  return selectedLeftIds.value.map((id) => map.get(id)).filter(Boolean);
});

const selectedRightItem = computed(() => {
  return right.value.find((i) => i.id === selectedRightId.value) || null;
});
</script>
