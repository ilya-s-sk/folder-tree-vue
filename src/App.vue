<template>
  <div id="app">
    <div v-for="item in items.children" :key="item.id">
      <item
        :name="item.name"
        :children="item.children"
        :id="item.id"
        :margin="item.level * 10"
        @addItem="addItem"
        @deleteItem="deleteItem"
      />
    </div>
  </div>
</template>

<script>
import Item from "./components/Item.vue";

export default {
  name: "App",
  components: {
    Item,
  },
  data() {
    return {
      maxId: 6,
      items: {
        children: [
          {
            name: "1. item",
            id: 0,
            children: [],
            level: 0,
          },
          {
            name: "2. item",
            id: 1,
            children: [],
            level: 0,
          },
          {
            name: "3. item",
            id: 2,
            children: [
              {
                name: "3.1. subitem",
                id: 3,
                children: [
                  {
                    name: "3.1.1. subsubitem",
                    id: 4,
                    children: [],
                    level: 2,
                  },
                  {
                    name: "3.1.2. subsubitem",
                    id: 5,
                    children: [],
                    level: 2,
                  },
                ],
                level: 1,
              },
              {
                name: "3.2. subitem",
                id: 6,
                children: [],
                level: 1,
              },
            ],
            level: 0,
          },
        ],
      },
    };
  },
  methods: {
    findItem(arr, id) {
      for (let i = 0; i < arr.length; i++) {
        let item = arr[i];
        if (item.id === id) {
          return item;
        } else if (item.children?.length > 0) {
          let result = this.findItem(item.children, id);
          if (result) {
            return result;
          }
        }
      }
    },
    findParent(obj, id) {
      for (let i = 0; i < obj.children.length; i++) {
        let item = obj.children[i];
        if (item.id === id) {
          return obj;
        } else if (item.children.length > 0) {
          let result = this.findParent(item, id);
          if (result) {
            return result;
          }
        }
      }
    },
    addItem(id) {
      const newItem = {
        name: "new item",
        id: ++this.maxId,
        children: [],
      };
      const foundedItem = this.findItem(this.items.children, id);
      newItem.level = foundedItem.level + 1;
      foundedItem.children.push(newItem);
    },
    deleteItem(id) {
      const parent = this.findParent(this.items, id);
      parent.children = parent.children.filter(item => item.id !== id);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 20px;
}
</style>
