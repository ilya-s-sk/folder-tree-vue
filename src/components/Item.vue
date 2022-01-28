<template>
  <div class="item">
    <div class="item-content"
    :style="{marginLeft: margin + 'px'}"
    >
      <p>{{name}}</p>
      <div class="item-content__btn-group">
        <button
          class="item-content__btn"
          @click="$emit('addItem', id)"
        >
          add
        </button>
        <button
          class="item-content__btn"
          @click="$emit('deleteItem', id)"
        >
          delete
        </button>
        <button
          :class="buttonClasses"
          :disabled="!(children.length > 0)"
          @click="toggle"
        >{{open ? 'close' : 'open'}}</button>
      </div>
    </div>
    <div v-if="open" class="subitems">
      <div v-for="item in children" :key="item.id" >
        <item 
          :name="item.name" 
          :children="item.children"
          :id="item.id"
          :margin="item.level * 20"
          @addItem="emitAddItem"
          @deleteItem="emitDeleteItem" />
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Item',
  props: {
    name: {
      type: String,
      default: ''
    },
    children: {
      type: Array,
      default: () => ([])
    },
    id: {
      type: Number
    },
    margin: {
      type: Number
    }
  },
  data() {
    return {
      open: true
    }
  },
  methods: {
    toggle() {
      this.open = !this.open;
    },
    emitAddItem(id) {
      this.$emit('addItem', id)
    },
    emitDeleteItem(id) {
      this.$emit('deleteItem', id)
    }
  },
  computed: {
    buttonClasses() {
      return [ 'item-content__btn', {
        'item-content__btn_disabled': !this.children.length
      }]
    }
  },
}
</script>


<style scoped>
.item-content {
  display: inline-flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 10px;
  gap: 20px;
  width: 350px;
  height: 50px;
  border: 1px solid #222;
  margin-bottom: 10px;
}
.item-content__btn-group {
  display: inline-flex;
  gap: 10px;
}
.item-content__btn {
  border: none;
  border-radius: 5px;
  cursor: pointer;
  width: 50px;
  color: #fff;
  background-color: #33d;
}
.item-content__btn_disabled {
  background-color: #bbb;
  cursor:auto;
}
.subitems {
  font-size: 14px;
}
</style>
