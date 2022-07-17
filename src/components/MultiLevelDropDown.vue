<template>
  <!-- Large button groups (default and split) -->
  <div class="container">
    <div class="col-4">
      <div class="input-group input-group-sm ml-5">
        <!-- <span class="input-group-text" id="inputGroup-sizing-sm">Small</span> -->
        <input
          type="text"
          class="d-flex justify-content-between mb-1"
          id="dropdownMenuLink"
          v-model="searchString"
          @input="performSearch"
        />
        <span class="mt-2"
          ><font-awesome-icon
            icon="fa-solid fa-angle-down"
            @click="performSearch"
        /></span>
      </div>
      <div class="list" v-if="showlist">
        <ul>
          <ListItem
            ref="listvehicle"
            v-for="(item, index) in itemsArray"
            :key="index"
            :add-margin="item.childItems ? 0 : 10"
            :iterate-object="item"
            :search-string="searchString"
          >
          </ListItem>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { listItems } from "@/jsondata.js";
import ListItem from "./ListItem.vue";
export default {
  name: "HelloWorld",
  components: { ListItem },
  props: {
    msg: String,
  },
  data() {
    return {
      showlist: false,
      list: listItems,
      dummyList: listItems,
      searchString: "",
      itemsArray: [],
    };
  },
  methods: {
    performSearch() {
      this.showList(this.list);
    },
    showList(list) {
      let finalOutput = [];
      list.forEach((item) => {
        if (item.text.toLowerCase().indexOf(this.searchString) > -1) {
          finalOutput.push(item);
        } else if (item.childItems && item.childItems.length) {
          let listchild = this.showList(item.childItems);
          if (listchild.length)
            finalOutput.push({ text: item.text, childItems: listchild });
        }
      });
      this.itemsArray = finalOutput;
      this.showlist = true;
      this.itemsArray.length > 0
        ? (this.$refs.listvehicle.setdropdown = true)
        : (this.$refs.listvehicle.setdropdown = false);
      return finalOutput;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list {
  border: 1px solid #6c757d;
  border-radius: 5px;
}
.input-group {
  color: #555;
  display: flex;
  padding: 2px;
  border: 1px solid currentColor;
  border-radius: 5px;
}
input[type="text"] {
  background: transparent;
  margin: 0;
  padding: 7px 8px;
  font-size: 14px;
  color: inherit;
  border: 1px solid transparent;
  border-radius: inherit;
}
input[type="text"]:focus {
  box-shadow: none;
  border-color: transparent;
  outline: none;
}
span {
  overflow: hidden;
  width: 48%;
  padding-left: 23px;
  margin-top: auto;
  border: 1px solid transparent;
  border-radius: inherit;
  background: transparent;
  cursor: pointer;
  opacity: 0.7;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
  margin-bottom: 0;
}


/* li {
  display: inline-block;
  margin: 0 10px;
} */
a {
  color: #42b983;
}
</style>
