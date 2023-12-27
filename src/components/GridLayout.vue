<template>
  <div class="grid-container">
    <div class="item1" @click="toggleComponentList('Item 1')">
      <template v-if="!item1Component">item 1</template>
      <component :is="item1Component" v-else></component>
    </div>
    <div class="item2" @click="openModal('Item 2')">item 2</div>
    <div class="item3" @click="openModal('Item 3')">item 3</div>
    <div class="item4" @click="openModal('Item 4')">item 4</div>
    <div class="item5" @click="openModal('Item 5')">item 5</div>
  </div>

  <div class="modal-background" v-if="selectedModalItem">
    <div class="modal-content">
      <h2>{{ selectedModalItem }}</h2>
    <div class="component-list" v-if="showComponentList">
      <div class="component" @click="selectComponent('ComponentA')">
        ComponentA
      </div>
      <div class="component" @click="selectComponent('ComponentB')">
        ComponentB
      </div>
    </div>
    <button @click="closeModal">Đóng</button>
    <button @click="resetItem1">Reset</button>
  </div>
  </div>
</template>

<script>
import ComponentA from "./ComponentA.vue";
import ComponentB from "./ComponentB.vue";

export default {
  data() {
    return {
      selectedModalItem: null,
      showComponentList: false,
      item1Component: null,
      item1ComponentContent: null,
      item1ComponentSelected: false,
    };
  },
  components: {
    ComponentA,
    ComponentB,
  },
  methods: {
    toggleComponentList(itemName) {
      if (this.showComponentList) {
        this.showComponentList = false;

        this.item1Component = null;
        this.item1ComponentContent = null;
        this.selectedModalItem = null;
      } else {
        this.selectedModalItem = itemName;
        this.showComponentList = true;
      }
    },
    selectComponent(componentName) {
      if (componentName === this.item1Component) {
        this.item1Component = null;
        this.item1ComponentContent = null;
        this.item1ComponentSelected = false;
      } else {
        this.item1Component = componentName;
        this.item1ComponentSelected = true;
      }

      this.showComponentList = false;
    },
    closeModal() {
      this.selectedModalItem = null;
      this.showComponentList = false;

      if (!this.item1ComponentSelected) {
        this.item1Component = null;
        this.item1ComponentContent = null;
      }
    },
    resetItem1() {
      this.item1Component = null;
      this.item1ComponentContent = null;
      this.item1ComponentSelected = false;
    },
  },
};
</script>
<style>
.item1 {
  grid-area: header;
}
.item2 {
  grid-area: menu;
}
.item3 {
  grid-area: main;
}
.item4 {
  grid-area: right;
}
.item5 {
  grid-area: footer;
}

.grid-container {
  display: grid;
  grid-template-areas:
    "header header header header header header"
    "menu main main main right right"
    "menu footer footer footer footer footer";
  gap: 10px;
  background-color: #2196f3;
  padding: 10px;
}

.grid-container > div {
  background-color: rgba(255, 255, 255, 0.8);
  text-align: center;
  padding: 20px 0;
  font-size: 30px;
}

.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

/* Modal content */
.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
}

.component-list {
  display: flex;
  flex-direction: column;
}

.component {
  cursor: pointer;
  padding: 10px;
}
</style>
