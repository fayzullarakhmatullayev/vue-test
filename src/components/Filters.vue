<template>
  <div class="filters">
    <div class="burger" @click="$emit('openSidebar')">
      <span></span>
      <span></span>
      <span></span>
    </div>
    <div class="filters-inner" v-if="items.length">
      <select
        class="items-filter"
        @change="$emit('getValue', $event)"
        :style="isByName ? 'max-width: 100%; width: 146px;' : ''"
      >
        <option :value="0">По умолчанию</option>
        <option :value="1">По цене min</option>
        <option :value="2">По цене max</option>
        <option :value="3">По наименованию</option>
      </select>
      <input
        v-if="isByName"
        type="text"
        class="items-filter"
        placeholder="Напишите наименование"
        @input="$emit('search', $event)"
      />
    </div>
  </div>
</template>

<script>
export default {
  emits: ["getValue", "search", "openSidebar"],
  props: ["isByName", "items"],
};
</script>

<style></style>
