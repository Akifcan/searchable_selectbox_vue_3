<script>
import { ref, onMounted, watch } from "vue";
export default {
  props: {
    items: Array,
    limit: {
      type: Number,
    },
  },
  emits: ["changeValue"],
  setup(props, { emit }) {
    let showList = ref(false);
    const search = ref("");
    const value = ref(null);
    let lastPressedKey = null;

    function lowerCaseListItems() {
      return props.items.map((item) => {
        return {
          title: item.title.replace(/İ/g, "i").toLowerCase(),
          value: item.value,
        };
      });
    }

    function searchInItems(keyword) {
      const items = lowerCaseListItems();
      if (keyword.length > 0) {
        listItems._value = items.filter((item) => item.title.includes(keyword));
      } else {
        listItems._value = lowerCaseListItems();
      }
    }

    function hideList() {
      setTimeout(() => {
        showList.value = false;
        if (search.value.length != 0) {
          emit("change-value", value.value);
        } else {
          console.log("no");
        }
      }, 250);
    }

    function deleteSearchText(e) {
      if (e.keyCode == lastPressedKey) {
        search.value = "";
      }
      lastPressedKey = e.keyCode;
    }

    const listItems = ref(lowerCaseListItems());

    watch(search, (newVal, oldVal) => {
      searchInItems(newVal.toLowerCase());
    });

    return {
      showList,
      listItems,
      search,
      hideList,
      value,
      deleteSearchText,
    };
  },
};
</script>

<template>
  <div class="search-selectbox">
    <input
      v-model="search"
      @blur="hideList"
      @keyup="deleteSearchText"
      @focus="showList = true"
      type="text"
    />
    <div class="search-selectbox__items">
      <ul v-if="showList">
        <li
          v-for="item in listItems.slice(0, limit || listItems.length)"
          @click="
            (search = item.title), (value = item.value), (showList = false)
          "
          :key="item.value"
        >
          {{ item.title }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.search-selectbox {
  position: relative;
  background: white;
}
.search-selectbox::before {
  position: absolute;
  content: "⬇";
  right: 5px;
  font-size: 1.5rem;
  z-index: 1;
}
.search-selectbox__items {
  font-family: sans-serif;
  border-bottom-left-radius: 0.5em;
  border-bottom-right-radius: 0.5em;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.5);
}

.search-selectbox input {
  position: relative;
  text-transform: capitalize;
  border-top-left-radius: 0.5em;
  border-top-right-radius: 0.5em;
  padding: 0.5em;
  border: 1px solid #ced4da;
  background: #fff;
  color: #495057;
  font-size: 1rem;
}

.search-selectbox__items ul {
  height: 10em;
  overflow-y: scroll;
  position: relative;
  list-style: none;
}

.search-selectbox__items li {
  text-transform: capitalize;
  cursor: pointer;
  padding: 1em;
}

.search-selectbox__items li:hover {
  background: #eee;
}
</style>
