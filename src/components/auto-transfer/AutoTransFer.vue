<template>
  <div id="app">
    <div id="main-list" class="column">
      <h3>Main List</h3>
      <button
        v-for="(item, index) in items"
        :key="index"
        class="item border rounded-lg border-black p-2"
        :class="item.type.toLowerCase()"
        @click="moveToColumn(item)"
      >
        {{ item.name }}
      </button>
    </div>

    <div id="fruit-column" class="column border rounded-lg border-black p-2">
      <h3>Fruit</h3>
      <button
        v-for="(item, index) in fruitItems"
        :key="index"
        class="item fruit border rounded-lg border-black p-2"
        @click="returnToMain(item)"
      >
        {{ item.name }}
      </button>
    </div>

    <div id="vegetable-column" class="column border rounded-lg border-black p-2">
      <h3>Vegetable</h3>
      <button
        v-for="(item, index) in vegetableItems"
        :key="index"
        class="item vegetable border rounded-lg border-black p-2"
        @click="returnToMain(item)"
      >
        {{ item.name }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { type: "Vegetable", name: "Broccoli" },
        { type: "Vegetable", name: "Mushroom" },
        { type: "Fruit", name: "Apple" },
        { type: "Fruit", name: "Banana" },
        { type: "Vegetable", name: "Tomato" },
        { type: "Fruit", name: "Orange" },
        { type: "Fruit", name: "Mango" },
        { type: "Fruit", name: "Pineapple" },
        { type: "Vegetable", name: "Cucumber" },
        { type: "Fruit", name: "Watermelon" },
        { type: "Vegetable", name: "Carrot" },
      ],
      fruitItems: [],
      vegetableItems: [],
      timeouts: {}, 
    };
  },
  methods: {
    moveToColumn(item) {

      this.items = this.items.filter((i) => i !== item);

      if (item.type === "Fruit") {
        this.fruitItems.push(item);
      } else if (item.type === "Vegetable") {
        this.vegetableItems.push(item);
      }

    },
    returnToMain(item) {
      clearTimeout(this.timeouts[item.name]);

      this.items.push(item);

      if (item.type === "Fruit") {
        this.fruitItems = this.fruitItems.filter((i) => i !== item);
      } else if (item.type === "Vegetable") {
        this.vegetableItems = this.vegetableItems.filter((i) => i !== item);
      }
    },
  },
};
</script>

<style scoped>
.column {
  width: 150px;
  min-height: 300px;
  border: 1px solid #ddd;
  padding: 10px;
  margin: 10px;
  display: inline-block;
  vertical-align: top;
  background-color: #f9f9f9;
}

button.item {
  width: 100%;
  margin: 5px 0;
  cursor: pointer;
}
</style>
