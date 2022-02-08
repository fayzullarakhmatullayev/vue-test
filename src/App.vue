<template>
  <main class="main">
    <the-sidebar
      @handleSubmit="handleSubmit"
      @closeSidebar="isSidebarOpen = !isSidebarOpen"
      :class="{ 'sidebar-open': isSidebarOpen }"
    />
    <section class="items">
      <filters
        @getValue="getValue"
        @search="search"
        @openSidebar="isSidebarOpen = !isSidebarOpen"
        :isByName="isByName"
        :items="items"
      />
      <div class="empty" v-if="!items.length">
        Окно товаров пустое. Добавьте товар.
      </div>
      <div class="items-wrapper" v-else>
        <card
          v-for="(item, index) in filteredItems"
          :key="item.id"
          :item="item"
          :index="index"
          @deleteItem="deleteItem"
        />
      </div>
    </section>
  </main>
  <teleport to="body">
    <modal v-if="isModalOpen" />
  </teleport>
</template>

<script>
import Card from "./components/Card.vue";
import TheSidebar from "./components/TheSidebar.vue";
import Filters from "./components/Filters.vue";
import goods from "./goods.json";
import Modal from "./components/Modal.vue";
export default {
  components: { TheSidebar, Card, Filters, Modal },
  name: "App",
  data() {
    return {
      items: localStorage.items ? JSON.parse(localStorage.items) : goods,
      isByName: false,
      value: null,
      searchedValue: "",
      isModalOpen: false,
      counter: 0,
      isSidebarOpen: false,
    };
  },
  computed: {
    filteredItems() {
      return this.sortItems();
    },
  },
  methods: {
    handleSubmit(items) {
      this.isModalOpen = true;
      this.isSidebarOpen = false;
      const interval = setInterval(() => {
        this.counter++;
        if (this.counter === 1) {
          clearInterval(interval);
          this.isModalOpen = false;
        }
      }, 2000);
      this.counter = 0;
      this.items.push(items);
    },
    deleteItem(idx) {
      this.items.splice(idx, 1);
    },
    getValue(e) {
      this.value = +e.target.value;
      +e.target.value === 3 ? (this.isByName = true) : (this.isByName = false);
    },
    sortItems() {
      if (this.value === 0 || this.value === null) {
        return this.items.sort((a, b) => (a.id < b.id ? -1 : 1));
      } else if (this.value === 1) {
        return this.items.sort((a, b) => (+a.price < +b.price ? -1 : 1));
      } else if (this.value === 2) {
        return this.items.sort((a, b) => (+a.price > +b.price ? -1 : 1));
      } else if (this.value === 3 && this.searchedValue !== "") {
        return this.items.filter((item) =>
          item.title.toLowerCase().includes(this.searchedValue.toLowerCase())
        );
      } else {
        return this.items;
      }
    },
    search(e) {
      this.searchedValue = e.target.value;
    },
  },
  watch: {
    items: {
      handler(newItem) {
        localStorage.items = JSON.stringify(newItem);
      },
      deep: true,
    },
  },
  created() {
    if (!localStorage.items) {
      localStorage.items = JSON.stringify(goods);
    }
  },
};
</script>
