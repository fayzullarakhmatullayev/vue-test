<template>
  <div class="card">
    <img :src="item.img" :alt="item.title" class="card-img" />
    <div class="card-body">
      <h3 class="card-title">{{ item.title }}</h3>
      <div class="card-text">
        {{ item.text }}
      </div>
      <div class="card-price">{{ prettify(item.price) }} руб.</div>
    </div>
    <div class="card-delete" @click="$emit('deleteItem', index)">
      <img src="../assets/img/icons/delete.svg" alt="delete" />
    </div>
  </div>
</template>

<script>
export default {
  emits: ["deleteItem"],
  props: ["item", "index"],
  methods: {
    prettify(num) {
      return num
        .toString()
        .replace(/(\d{1,3}(?=(?:\d\d\d)+(?!\d)))/g, "$1" + " ");
    },
  },
};
</script>

<style></style>
