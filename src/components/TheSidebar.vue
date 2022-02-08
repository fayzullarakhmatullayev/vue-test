<template>
  <aside class="sidebar">
    <h2 class="sidebar-title">Добавление товара</h2>
    <div class="sidebar-blog">
      <form class="form" @submit.prevent="handleSubmit">
        <div class="form-control">
          <label for="name" class="form-label form-label__required"
            >Наименование товара</label
          >
          <input
            type="text"
            class="form-input"
            placeholder="Введите наименование товара"
            v-model.trim="title"
          />
        </div>
        <div class="form-control">
          <label for="desc" class="form-label">Описание товара</label>
          <textarea
            placeholder="Введите описание товара"
            class="form-textarea"
            id="desc"
            v-model.trim="text"
          ></textarea>
        </div>
        <div class="form-control">
          <label for="link" class="form-label form-label__required"
            >Ссылка на изображение товара</label
          >
          <input
            type="text"
            class="form-input"
            placeholder="Введите ссылку"
            v-model.trim="img"
          />
        </div>
        <div class="form-control" :class="isNotNumber ? 'error' : ''">
          <label for="link" class="form-label form-label__required"
            >Цена товара</label
          >
          <input
            type="text"
            class="form-input"
            placeholder="Введите цену"
            v-model.trim="price"
          />
          <small v-if="isNotNumber">Поле должно содержать только число</small>
        </div>
        <button class="form-submit" :disabled="isDisabled" type="submit">
          Добавить товар
        </button>
      </form>
    </div>
    <div class="close" @click="$emit('closeSidebar')">
      <span></span>
      <span></span>
    </div>
  </aside>
</template>

<script>
export default {
  emits: ["handleSubmit", "closeSidebar"],
  data() {
    return {
      title: "",
      text: "",
      img: "",
      price: "",
      isNotNumber: false,
    };
  },
  computed: {
    isDisabled() {
      return this.title !== "" && this.img !== "" && this.price !== ""
        ? false
        : true;
    },
  },
  methods: {
    handleSubmit() {
      if (!/^\d+$/.test(this.price)) {
        this.isNotNumber = true;
      } else {
        const payload = {
          title: this.title,
          text: this.text,
          img: this.img,
          price: this.price,
          id: Date.now(),
        };
        this.$emit("handleSubmit", payload);
        this.title = "";
        this.text = "";
        this.img = "";
        this.price = "";
        this.isNotNumber = false;
      }
    },
  },
};
</script>
// var reg = /^\d+$/;
